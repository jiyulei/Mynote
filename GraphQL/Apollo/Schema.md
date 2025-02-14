#apollo #graphql 
``` shell
npm install @apollo/server graphql graphql-tag
```
Under server Folder  
**Schema.js:**
``` js
const gql = require("graphql-tag");

const typeDefs = gql`
"The query"
type Query {
  tracksForHome: [Track!]!  
}

type Track {
    id: ID!
    title: String!
    author: Author!
    thumbnail: String
    length: Int
    modulesCount: Int
}

"Author of a complete Track"
type Author {
    id: ID!
    name: String!
    photo: String
}
`
module.exports = typeDefs;
```
****
**index.js**
``` js
const { ApolloServer } = require("@apollo/server");
const { startStandaloneServer } = require("@apollo/server/standalone");
const typeDefs = require("./schema");
const resolvers = require("./resolvers");
const TrackAPI = require("./datasources/track-api");
 
async function startApolloServer() {
  const server = new ApolloServer({ typeDefs, resolvers });
  const { url } = await startStandaloneServer(server, {
    context: async () => {
      const { cache } = server;
       
      return {
        dataSources: {
          trackAPI: new TrackAPI({ cache }),
        },
      };
    },
  });
  console.log(`
    🚀  Server is running
    📭  Query at ${url}
  `);
}
 
startApolloServer();
```


