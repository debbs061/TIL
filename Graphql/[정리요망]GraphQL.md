## GraphQL

* REST API는 URI 마다 하는 것이 정해져있음
* GraphQL 은 body 에 주문서만 제대로 넣어주면 유도리 있게 요청 가능 
* 백엔드 서버 하나에 Rest API 랑 GraphQL 둘 다 구현해놓는다.
  * 각 정보랑 요청마다 유리한 걸로 골라서 클라이언트에 제공하면 된다.



graphql.api.service.ts

```typescript
query(gql: DocumentNode, variables: any = {}): Observable<ApolloQueryResult<any>> {
        // https://github.com/kamilkisiela/apollo-angular
        // https://www.apollographql.com/docs/angular/basics/queries/
        return this.apollo.query({
            query: gql,
            variables,
            fetchPolicy: 'no-cache'
        });
    }
    
mutate(gql: DocumentNode, variables: any = {}): Observable<FetchResult<any>> {
        // https://github.com/kamilkisiela/apollo-angular
        // https://www.apollographql.com/docs/angular/basics/mutations/
        return this.apollo.mutate({
            mutation: gql,
            variables,
            context: {useMultipart: true},
            fetchPolicy: 'no-cache'
        });
    }
```



## Apollo

* [Apollo](https://www.apollographql.com/docs/react/development-testing/static-typing/) supports type definitions for TypeScript.

```typescript
interface RocketInventory {
  id: number;
  model: string;
  year: number;
  stock: number;
}
```



----

### 출처

https://www.youtube.com/watch?v=EkWI6Ru8lFQ&t=0s