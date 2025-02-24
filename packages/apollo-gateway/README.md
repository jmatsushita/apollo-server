# Apollo Gateway

This package provides utilities for combining multiple GraphQL microservices into a single GraphQL endpoint.

Each microservice should implement the [federation schema specification](https://www.apollographql.com/docs/apollo-server/federation/federation-spec/). This can be done either through [Apollo Federation](https://github.com/apollographql/apollo-server/tree/master/packages/apollo-federation) or a variety of other open source products.

For complete documentation, see the [Apollo Gateway API reference](https://www.apollographql.com/docs/apollo-server/api/apollo-gateway/).

## Usage

```js
const { ApolloServer } = require("apollo-server");
const { ApolloGateway } = require("@apollo/gateway");

const gateway = new ApolloGateway({
  serviceList: [
    { name: "accounts", url: "http://localhost:4001/graphql" },
    // List of federation-capable GraphQL endpoints...
  ]
});

const server = new ApolloServer({ gateway });

server.listen().then(({ url }) => {
  console.log(`🚀 Server ready at ${url}`);
});
```


It's also possible to first `load()` the gateway and use the `exception` option to throw if the federated service fails to connect.

```js
const gateway = new ApolloGateway({
  serviceList: [
    {
      name: 'accounts',
      url: 'http://localhost:4001',
      // will throw when loading if it fails to connect
      exception: true },
    // more services
  ],
});

try {
  const { schema, executor } = await gateway.load()

  const server = new ApolloServer({
    schema,
    executor,
    subscriptions: false,
  });
} catch (error) {
  // Will catch a failure to connect to a federated service.
}
```
