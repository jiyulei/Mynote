#apollo #graphql #query #mutation 

### **Query and Mutation are both GraphQL operations.**
***
#### Query
Queries are **read** operations that always **retrieve** data.

#### Mutation
Mutations are **write** operations that always **modify** data.

Both query and mutation are using **SDL** syntax ( [[Schema（gql）]])

**Query 的写法一样只是把 `Mutation`改成 `Query`**
![[Pasted image 20250218132436.png]]
***
1. 这里的mutation要用到`async` `await`的主要原因是因为返回的对象要用到track，所以必须等它解决掉。
2. dataSources里面提供了`error.extensions` 来处理status code 和 message

``` js
const resolvers = {
  Query: {
    // returns an array of Tracks that will be used to populate the homepage grid of our web client
    tracksForHome: (_, __, { dataSources }) => {
      return dataSources.trackAPI.getTracksForHome();
    },

    // get a single track by ID, for the track page
    track: (_, { id }, { dataSources }) => {
      return dataSources.trackAPI.getTrack(id);
    },

    // get a single module by ID, for the module detail page
    module: (_, { id }, { dataSources }) => {
      return dataSources.trackAPI.getModule(id);
    },
  },
  Mutation: {
    incrementTrackViews: async  (_, { trackId }, { dataSources }) => { 
      try {
	        const track = await       dataSources.trackAPI.incrementTrackViews(trackId);
        return {  
          code: 200,
          success: true,
          message: `Successfully incremented number of views for track ${trackId}`,
          track,
        };
      } catch (error) {
          return {
          code: error.extensions.response.status,
          success: false,
          message: error.extensions.response.body,
          track: null,
        };
      }
    },
  },
  Track: {
    author: ({ authorId }, _, { dataSources }) => {
      return dataSources.trackAPI.getAuthor(authorId);
    },

    modules: ({ id }, _, { dataSources }) => {
      return dataSources.trackAPI.getTrackModules(id);
    },
  },
};

module.exports = resolvers;
```