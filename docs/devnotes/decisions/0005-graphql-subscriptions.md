# Real-time Updates with GraphQL Subscriptions

**Date**: 2024-03-03
**Status**: Accepted
**Context**: We need to provide real-time updates to clients when articles are modified, highlights are created, or other changes occur, ensuring a responsive and interactive user experience.

## Decision

We will implement real-time updates using GraphQL subscriptions, providing:
- WebSocket-based communication
- Type-safe subscription schema
- Efficient update delivery
- Scalable subscription management
- Automatic reconnection handling

## Consequences

### Positive

1. **Real-time Updates**
   - Immediate data synchronization
   - Better user experience
   - Reduced polling overhead
   - Efficient resource usage

2. **Type Safety**
   - Consistent with GraphQL types
   - Type-safe subscription payloads
   - Better error handling
   - Improved developer experience

3. **Scalability**
   - Efficient connection management
   - Optimized message delivery
   - Better resource utilization
   - Easy to scale horizontally

4. **Developer Experience**
   - Unified API interface
   - Familiar GraphQL syntax
   - Easy to implement
   - Good tooling support

### Negative

1. **Complexity**
   - Additional infrastructure needed
   - More complex deployment
   - Connection state management
   - Error handling complexity

2. **Resource Usage**
   - WebSocket connections
   - Memory overhead
   - Network bandwidth
   - Server resources

3. **Operational Overhead**
   - Connection monitoring
   - State management
   - Error recovery
   - Performance tuning

## Alternatives

### 1. Polling

**Pros**:
- Simple to implement
- No additional infrastructure
- Works everywhere
- Easy to debug

**Cons**:
- High server load
- Network overhead
- Delayed updates
- Resource waste

### 2. WebSocket + Custom Protocol

**Pros**:
- Full control over protocol
- Potentially better performance
- Custom features possible
- No GraphQL overhead

**Cons**:
- Custom implementation needed
- No type safety
- More complex client code
- Limited tooling

### 3. Server-Sent Events (SSE)

**Pros**:
- One-way communication
- Simple to implement
- Works over HTTP
- Good browser support

**Cons**:
- No bi-directional communication
- Limited reconnection handling
- No built-in type safety
- Less flexible

## Implementation Details

1. **Subscription Schema**
   ```graphql
   type Subscription {
     articleUpdated(id: ID!): Article!
     articleDeleted(id: ID!): ID!
     highlightCreated(articleId: ID!): Highlight!
     highlightUpdated(id: ID!): Highlight!
     highlightDeleted(id: ID!): ID!
   }
   ```

2. **Client Implementation**
   ```typescript
   const client = new ApolloClient({
     link: split(
       ({ query }) => hasSubscription(query),
       new WebSocketLink({
         uri: 'ws://api.omnivore.app/graphql',
         options: {
           reconnect: true,
           connectionParams: {
             authToken: getAuthToken()
           }
         }
       }),
       httpLink
     )
   });
   ```

3. **Server Implementation**
   ```typescript
   const pubsub = new PubSub();
   
   const resolvers = {
     Subscription: {
       articleUpdated: {
         subscribe: (_, { id }) => {
           return pubsub.asyncIterator(`ARTICLE_UPDATED_${id}`);
         }
       }
     }
   };
   ```

4. **Performance Optimization**
   - Connection pooling
   - Message batching
   - Subscription filtering
   - Resource cleanup

## Related ADRs

- [GraphQL as Primary API Interface](./0001-graphql-as-primary-api.md)
- [TypeScript Across the Stack](./0004-typescript-stack.md) 