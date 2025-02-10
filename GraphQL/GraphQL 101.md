#graphql #express
### **本例子的示意图**
![[Pasted image 20250210033620.png]]

**Express**: process users http request and response.
*****
**实际的数据库中数据** VS **GraphQLObjectType**
我们希望取来的`UserType`包含对应的company信息
![[Pasted image 20250210042832.png]]
希望通过以下query (顺序是重要的 id name description / id description name)：
**要什么field 就写什么**
``` graphQL
  user(id: "23") {
  	firstName
    company {
      id 
      name 
      description
    }
  }
}
```

来取得：
``` JSON
{
  "data": {
    "user": {
      "firstName": "Bill",
      "company": {
        "id": "1",
        "name": "Apple",
        "description": "iphone"
      }
    }
  }
}
```
*****
### Data in DB:
``` json 
{
  "users": [
    { "id": "23", "firstName": "Bill", "age": 20, "companyId": "1" },
    { "id": "30", "firstName": "Jimmy", "age": 32, "companyId": "2" },
    { "id": "41", "firstName": "Nick", "age": 40, "companyId": "2" }
  ],
  "companies": [
    { "id": "1", "name": "Apple", "description": "iphone" },
    { "id": "2", "name": "Google", "description": "search" }
  ]
}
```
### **Server.js** step up
``` javascript
import express from 'express';
import { graphqlHTTP } from 'express-graphql';
import schema from './schema/schema.js';

const app = express();
// any request that starts with /graphql will be handled by graphqlHTTP
app.use(
  "/graphql",
  graphqlHTTP({
    schema,
    // IDE
    graphiql: true,
  })
);

app.listen(4100, () => {
    console.log('Listening');
});
```

**schema**: what applications's data looks like. --- what property each object has, how each objects related to each other.
*****
``` javascript
import graphql, { GraphQLInt, GraphQLString } from 'graphql';
import axios from 'axios';

const { 
    GraphQLObjectType,
    GraphQLSchema
} = graphql;

const CompanyType = new GraphQLObjectType({
    name: "Company",
    fields: {
        id: { type: GraphQLString },
        name: { type: GraphQLString },
        description: { type: GraphQLString }
    }
});
// parentValue 就是凭借rootquery中的args.id取来的这个user实例，
// 就是实际db中取来的，它包含一条companyId，所以可以用parentValue.companyId
// 传入 resolve 函数中
const UserType = new GraphQLObjectType({
  name: "User",
  fields: {
    id: { type: GraphQLString },
    firstName: { type: GraphQLString },
    age: { type: GraphQLInt },
    company: { 
        type: CompanyType,
        resolve(parentValue, args) {
            return axios.get(`http://localhost:3000/companies/${parentValue.companyId}`)
            .then(res => res.data);
        }
    }
  }
});

// Root query: entry point of the data.
const RootQuery = new GraphQLObjectType({
    name: 'RootQueryType',
    fields: {
        user: {
            type: UserType,
            args: { id: { type: GraphQLString }},
            resolve(parentValue, args) {
                return axios.get(`http://localhost:3000/users/${args.id}`)
                .then(resp => resp.data);
            }
        }
    }
})

const schema = new GraphQLSchema({ query: RootQuery });
export default schema;
```
