#graphql #mutation
[完整代码](https://github.com/jiyulei/graphQLPlayground/blob/main/schema/schema.js)

![[Pasted image 20250210165915.png]]

`Mutation` 也是`GraphQLObjectType` :
	1. `GraphQLNonNull`是确保这个参数不为空，取出来用`const { GraphQLNonNull } = graphql`（validation， required）
	2. 和`RootQuery` 一样要放到schema里
	`const schema = new GraphQLSchema({ query: RootQuery, mutation });`
	3. `put` vs `patch` [[Http & API]]
``` javascript
const mutation = new GraphQLObjectType({
  name: "Mutation",
  fields: {
  // Add
    addUser: {
      type: UserType,
      // 这里的companyId是optional的
      args: {
        firstName: { type: new GraphQLNonNull(GraphQLString) },
        age: { type: new GraphQLNonNull(GraphQLInt) },
        companyId: { type: GraphQLString },
      },
      resolve(parentValue, { firstName, age }) {
        return axios
          .post(`http://localhost:3000/users`, { firstName, age })
          .then((res) => res.data);
      },
    },
    // Delete
    deleteUser: {
      type: UserType,
      // 删除必须有id
      args: {
        id: { type: new GraphQLNonNull(GraphQLString) },
      },
      resolve(parentValue, { id }) {
        return axios
          .delete(`http://localhost:3000/users/${id}`)
          .then((res) => res.data);
      },
    },
    // Partial Update (Patch)
    editUser: {
      type: UserType,
      // 提供id找到条目，更新提供的字段。
      // 不用担心id，提供了id也不会改变（jsonserver的特性）
      args: {
        id: { type: new GraphQLNonNull(GraphQLString) },
        firstName: { type: GraphQLString },
        age: { type: GraphQLInt },
        companyId: { type: GraphQLString },
      },
      resolve(parentValue, args) {
        return axios
          .patch(`http://localhost:3000/users/${args.id}`, args)
          .then((res) => res.data);
      },
    },
  },
});
```
![[Pasted image 20250210174319.png]]
