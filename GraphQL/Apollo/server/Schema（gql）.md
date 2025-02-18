#graphql 
### 注意区分gql和query的查询语句 [[GraphQL Qurery(查询语句)]]
##### **以下为gql**
A **schema** is like a contract between the server and the client. It defines what a GraphQL API can and can't do, and how clients can request or change data.
****
![[Pasted image 20250211042800.png]]
### **在server中 schema.js 文件**
``` javascript
const gql = require("graphql-tag");
 // 注释写在""中
const typeDefs = gql`
"The query"
type Query {
  tracksForHome: [Track!]!
  track(id: ID!): Track
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