#apollo #graphql #mutation #hook
##### 注意区别[[UseQuery]]

This returns an **array**, where the first element is the **mutate function** used to trigger the **mutation**. The second element is an **object** with more information about the mutation, such as `loading`, `error` and `data`. This hook takes in a GraphQL operation as the first parameter. It also takes in an options object as the second parameter, where properties like `variables`are set.
***
``` js
import React from 'react';
import styled from '@emotion/styled';
import { colors, mq } from '../styles';
import { humanReadableTimeFromSeconds } from '../utils/helpers';
import { Link } from 'react-router-dom';
// import
import { gql, useMutation } from '@apollo/client';
// 
const INCREMENT_TRACK_VIEWS = gql`
  mutation IncrementTrackViews($trackId: ID!) {
    incrementTrackViews(trackId: $trackId) {
      code
      success
      message
      track {
        id
        numberOfViews
      }
    }
  }
`;

const TrackCard = ({ track }) => {
  const { title, thumbnail, author, length, modulesCount, id } = track;
  // 返回的是数组注意区别useQuery返回的是object，第一个参数是function
  // 这里是自己命名的，但要和下面onclick里的一致
  const [incrementTrackViews, { loading, error, data }] =  useMutation(INCREMENT_TRACK_VIEWS, {
    variables: { trackId: id },
    onCompleted: (data) => {
      console.log(data);
    },  
    onError: (error) => {
      console.error(error);
    },  
  });
	// 返回的function传入onclick
  return (<CardContainer to={`/track/${id}`} onClick={incrementTrackViews}> ...)
```