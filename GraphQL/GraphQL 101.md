#graphql #express #restful 
### **本例子的示意图**
![[Pasted image 20250210033620.png]]

**Express**: process users http request and response.
*****
**实际的数据库中数据** VS **GraphQLObjectType**
我们希望取来的`UserType`包含对应的company信息
![[Pasted image 20250210042832.png]]
希望通过以下[[query ]](顺序是重要的 id name description / id description name)：
**要什么field 就写什么**
``` graphQL
{
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
### server.js (express setup)
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
    // enable graphiql IDE
    graphiql: true,
  })
);

app.listen(4100, () => {
    console.log('Listening');
});
```

**schema**: what applications's data looks like. --- what property each object has, how each objects related to each other.
*****
### schema.js

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
        },
	    company: {
            type: CompanyType,
            args: { id: { type: GraphQLString }},
            resolve(parentValue, args) {
                return axios.get(`http://localhost:3000/companies/${args.id}`)
                .then(res => res.data);
            }
        }
    }
})

const schema = new GraphQLSchema({ query: RootQuery });
export default schema;
```


我们这里又两个`RootQuery`的入口 user 和 company，
所以我我们可以通过一下`query`得到company的信息：
``` graphql
{
  company(id: "1") {
    name
  }
}
```
根据以上代码我们又如下关系
![[Pasted image 20250210044932.png]]

但目前问题是还没有从company到User的逻辑，在RESTfulAPI里可以利用资源嵌套[[Nested Resources]] 来实现， 比如 `http://localhost:3000/companies/2/users` 会给我们所有`companyId`是2的`user`

要想实现从company到users的逻辑则需要更改`CompanyType`
注意这里的 `fields： () => {}` 之所以要写成callback function是因为 `CompanyType` 和`UserType`互相引用了（circular reference）， 所以需要利用javascript的**闭包**（[[Closure]]）来解决问题（先定义了两个Type， 之后回调函数执行）。
``` javascript
const CompanyType = new GraphQLObjectType({
    name: "Company",
    fields: () => ({
        id: { type: GraphQLString },
        name: { type: GraphQLString },
        description: { type: GraphQLString },
        // 添加以下逻辑，注意import GraphQLList, 因为可能多个user在同一公司
        users: { 
            type: new GraphQLList(UserType),
            resolve(parentValue, args) {
            // 以下的url即利用了资源嵌套 （Nested Resources）
                return axios.get(`http://localhost:3000/companies/${parentValue.id}/users`)
                .then(res => res.data);
            }
        }
    })
});

const UserType = new GraphQLObjectType({
  name: "User",
  fields: () => ({
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
  })
});
```
****
