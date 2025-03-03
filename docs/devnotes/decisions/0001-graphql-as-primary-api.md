# GraphQL as Primary API Interface

**Date**: 2024-03-03
**Status**: Accepted
**Context**: We need to provide a flexible and efficient API interface for the Omnivore service that can support various client applications (web, mobile, desktop) while minimizing network requests and providing real-time updates.

## Decision

We will use GraphQL as our primary API interface, providing a single endpoint that allows clients to:
- Request exactly the data they need
- Perform multiple operations in a single request
- Receive real-time updates through subscriptions
- Have strongly-typed queries and responses

## Consequences

### Positive

1. **Flexible Data Fetching**
   - Clients can request only the fields they need
   - Reduces over-fetching and under-fetching
   - Minimizes network payload size

2. **Type Safety**
   - Strong typing across the entire stack
   - Better developer experience with autocomplete
   - Runtime type checking

3. **Real-time Capabilities**
   - Built-in subscription support
   - Efficient real-time updates
   - Reduced server load compared to polling

4. **API Evolution**
   - Backward compatible schema changes
   - No versioning needed
   - Easier to deprecate fields

### Negative

1. **Learning Curve**
   - Team needs to learn GraphQL concepts
   - More complex than REST for simple CRUD
   - Requires additional tooling

2. **Performance Considerations**
   - Complex queries can be expensive
   - Need careful query optimization
   - Potential for N+1 query problems

3. **Caching Complexity**
   - More complex caching strategies
   - Need specialized caching solutions
   - Cache invalidation can be challenging

## Alternatives

### 1. REST API

**Pros**:
- Simpler to understand and implement
- Better caching mechanisms
- More mature tooling

**Cons**:
- Multiple endpoints needed
- Over-fetching/under-fetching issues
- No built-in real-time support
- Versioning required for changes

### 2. gRPC

**Pros**:
- High performance
- Strong typing
- Bi-directional streaming

**Cons**:
- More complex setup
- Limited browser support
- Less flexible for web clients
- Steeper learning curve

### 3. WebSocket + REST

**Pros**:
- Simple REST for CRUD
- Real-time updates via WebSocket
- Familiar technologies

**Cons**:
- Two separate protocols to maintain
- More complex client implementation
- Higher operational overhead
- No type safety across the stack

## Implementation Details

1. **Schema Design**
   ```graphql
   type Query {
     articles: [Article!]!
     article(id: ID!): Article
     # ... other queries
   }

   type Mutation {
     createArticle(input: CreateArticleInput!): Article!
     # ... other mutations
   }

   type Subscription {
     articleUpdated(id: ID!): Article!
     # ... other subscriptions
   }
   ```

2. **Client Integration**
   - Apollo Client for web applications
   - GraphQL code generation for type safety
   - Custom hooks for common operations

3. **Performance Optimization**
   - Query complexity analysis
   - Depth limiting
   - Rate limiting
   - Caching strategies

## Related ADRs

- [Real-time Updates with GraphQL Subscriptions](./0005-graphql-subscriptions.md)
- [TypeScript Across the Stack](./0004-typescript-stack.md) 