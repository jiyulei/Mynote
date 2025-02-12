#apollo #graphql 
``` shell
npm install graphql @apollo/client
```

``` javascript
import React from "react";
import { createRoot } from "react-dom/client";
import GlobalStyles from "./styles";
import Pages from "./pages";
// 1.import 
import { ApolloProvider, ApolloClient, InMemoryCache } from "@apollo/client";
// 2. create new client
const client = new ApolloClient({
  // Location of GraphQL server
  uri: "http://localhost:4000",
  // store some query result in cahce, reduce network request
  cache: new InMemoryCache(),
});

const root = createRoot(document.getElementById("root"));

root.render(
  <React.StrictMode>
  // 3. Wrap React in Provider
    <ApolloProvider client={client}>
      <GlobalStyles />
      <Pages />
    </ApolloProvider>
  </React.StrictMode>
);

```

在实际需要用到数据的组件中这样写：
``` js
import React from 'react';
import { Layout } from '../components';
// 1.引入gql 和 userQuery Hook
import { gql, useQuery } from '@apollo/client';
import TrackCard from "../containers/track-card";
import QueryResult from "../components/query-result";
// 2. gql里面写query，最好在apollo GraphOS studio explorer里测试好复制过来
const TRACKS = gql`
  query GetTracks {
    tracksForHome {
      id
      title
      thumbnail
      length
      modulesCount
      author {
        id
        name
        photo
      }
    }
  }
`;

const Tracks = () => {
 // 3. 使用hook，hook里面提供了 loading error data
  const { loading, error, data } = useQuery(TRACKS);
  return (
    <Layout grid>  
      // 这是例子提供的一个组件 确保loading和error时候页面好看（比如旋转加载）
      <QueryResult loading={loading} error={error} data={data}>
	//4. 渲染 data
        {data?.tracksForHome?.map((track) => (
          <TrackCard key={track.id} track={track} />
        ))}
      </QueryResult>
    </Layout>
  );
};

export default Tracks;
```

test