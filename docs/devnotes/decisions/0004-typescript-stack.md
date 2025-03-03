# TypeScript Across the Stack

**Date**: 2024-03-03
**Status**: Accepted
**Context**: We need to ensure type safety, better developer experience, and maintainable code across all parts of the application, from frontend to backend, including shared libraries and tools.

## Decision

We will use TypeScript as the primary programming language across the entire stack:
- Frontend applications (web, mobile)
- Backend services
- Shared libraries
- Development tools
- Build scripts

Key aspects:
- Strict type checking
- Shared type definitions
- Consistent configuration
- Type-safe API contracts
- Automated type generation

## Consequences

### Positive

1. **Type Safety**
   - Catch errors at compile time
   - Better code quality
   - Reduced runtime errors
   - Improved maintainability

2. **Developer Experience**
   - Better IDE support
   - Intelligent code completion
   - Refactoring capabilities
   - Documentation through types

3. **Code Organization**
   - Clear interfaces
   - Better code structure
   - Self-documenting code
   - Easier to understand

4. **API Integration**
   - Type-safe API calls
   - Automatic type generation
   - Better error handling
   - Consistent data structures

### Negative

1. **Development Overhead**
   - Additional type annotations
   - More complex build process
   - Longer compilation times
   - Learning curve for new developers

2. **Build Complexity**
   - TypeScript compilation step
   - More complex build pipeline
   - Additional dependencies
   - Version management

3. **Migration Effort**
   - Converting existing code
   - Updating dependencies
   - Training team members
   - Tooling setup

## Alternatives

### 1. JavaScript with JSDoc

**Pros**:
- No compilation step
- Familiar syntax
- Existing code compatibility
- Simpler setup

**Cons**:
- Limited type checking
- No IDE integration
- Manual type documentation
- Less reliable

### 2. Flow

**Pros**:
- Gradual typing
- React integration
- Good type inference
- Facebook backing

**Cons**:
- Less mature ecosystem
- Limited tooling
- Smaller community
- Less documentation

### 3. Pure JavaScript

**Pros**:
- No type overhead
- Faster development
- Simpler tooling
- No compilation

**Cons**:
- No type safety
- More runtime errors
- Harder to maintain
- Poor IDE support

## Implementation Details

1. **TypeScript Configuration**
   ```json
   {
     "compilerOptions": {
       "target": "es2020",
       "module": "esnext",
       "strict": true,
       "esModuleInterop": true,
       "skipLibCheck": true,
       "forceConsistentCasingInFileNames": true
     }
   }
   ```

2. **Shared Types**
   ```typescript
   // @omnivore/types
   export interface Article {
     id: string;
     title: string;
     url: string;
     description?: string;
     savedAt: Date;
     readAt?: Date;
     labels: Label[];
   }
   ```

3. **API Type Generation**
   ```typescript
   // Generated from GraphQL schema
   export interface CreateArticleInput {
     url: string;
     title?: string;
     description?: string;
     labels?: string[];
     isPublic?: boolean;
   }
   ```

4. **Development Workflow**
   - Type checking in CI/CD
   - Automated type generation
   - Shared type definitions
   - Strict type checking

## Related ADRs

- [GraphQL as Primary API Interface](./0001-graphql-as-primary-api.md)
- [Monorepo Structure](./0002-monorepo-structure.md) 