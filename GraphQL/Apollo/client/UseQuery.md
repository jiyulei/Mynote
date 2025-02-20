#apollo #hook #graphql #query 
##### **区别[[UseMutation]]**
The useQuery hook returns an **object** with three useful properties that we use in our app: 

`loading` : indicates whether the query has completed and results have been returned. 
`error` : is an object that contains any errors that the operation has thrown.
`data`: contains the results of the query after it has completed. 
To set variables in our query, we declare them in the second parameter of the useQuery hook, inside an options object.
``` js
import React from "react";
import { gql, useQuery } from "@apollo/client";
import { Layout, QueryResult } from "../components";
import { useParams } from "react-router-dom";
import TrackDetail from "../components/track-detail";

export const GET_TRACK = gql`
  query Track($trackId: ID!) {
  track(id: $trackId) {
    id
    title
    author {
      id
      name
      photo
    }
    thumbnail
    length
    modulesCount
    description
    numberOfViews
    modules {
      id
      title
      length
    }
  }
}
`;

const Track = () => {
    const { trackId } = useParams();
    const { loading, error, data } = useQuery(GET_TRACK, {
      variables: { trackId },
    });
    return (
      <Layout>
        <QueryResult errro={error} loading={loading} data={data}>
          <TrackDetail track={data?.track} />
        </QueryResult>
      </Layout>
    );
};

export default Track;
```
这个例子的path是从router传入的所以用`useParams`

``` js
import React from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';
/** importing our pages */
import Tracks from './tracks';
import Track from './track';

export default function Pages() {
  return (
    <BrowserRouter>
      <Routes>
        <Route element={<Tracks />} path="/" />
        <Route element={<Track />} path="/track/:trackId" />
      </Routes>
    </BrowserRouter>
  );
}
```