#graphql 
### 主要用在client side [[Apollo ClientSide]]，和apollo GraphOS studio explorer

`$arg` 参数
``` graphql
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

```

``` graphql
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
}```