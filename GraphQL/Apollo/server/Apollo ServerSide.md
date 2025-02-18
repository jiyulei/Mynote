#apollo #graphql 
``` shell
npm install @apollo/server graphql graphql-tag
```
Under server Folder  

需要**三要素**： 1. [[Schema（gql）]] 2. [[Resolver]] 3. [[DataSources]]
****
**index.js**
``` js
const { ApolloServer } = require('@apollo/server');
const { startStandaloneServer } = require('@apollo/server/standalone');
// 三要素：1.resolver 2.typeDefs(query) 3.DataSources
const resolvers = require("./resolvers");
const typeDefs = require('./schema');
const TrackAPI = require('./datasources/track-api');

async function startApolloServer() {
  const server = new ApolloServer({
    typeDefs,
    resolvers,
  });
  const { url } = await startStandaloneServer(server, {
    context: async() => {
      const { cache } = server;
      return {
        dataSources: {
          trackAPI: new TrackAPI({ cache }),
        },
      };
    }
  });

  console.log(`
      🚀  Server is running
      📭  Query at ${url}
    `);
}

startApolloServer();
```

![[Pasted image 20250214053433.png]]![[Pasted image 20250214053555.png]]