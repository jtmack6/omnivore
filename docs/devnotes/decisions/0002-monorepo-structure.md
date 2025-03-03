# Monorepo Structure

**Date**: 2024-03-03
**Status**: Accepted
**Context**: We need to manage multiple related packages and applications (web client, mobile apps, shared libraries) while maintaining consistency, sharing code, and simplifying dependency management.

## Decision

We will use a monorepo structure with the following organization:

```
omnivore/
├── packages/           # Shared packages
│   ├── api/           # API client library
│   ├── ui/            # Shared UI components
│   ├── types/         # Shared TypeScript types
│   └── utils/         # Shared utilities
├── pkg/               # Additional packages
├── web/              # Web application
├── apple/            # iOS/macOS application
├── android/          # Android application
├── ml/               # Machine learning components
├── imageproxy/       # Image processing service
└── self-hosting/     # Self-hosting configurations
```

Key aspects of this structure:
- Use Lerna for package management
- Shared TypeScript configuration
- Unified build and test processes
- Centralized dependency management
- Consistent code style and linting

## Consequences

### Positive

1. **Code Sharing**
   - Easy to share code between projects
   - Consistent types and interfaces
   - Reduced duplication
   - Simplified dependency management

2. **Development Experience**
   - Single repository to clone
   - Unified tooling and configuration
   - Easier to make cross-project changes
   - Consistent development environment

3. **Version Control**
   - Atomic commits across projects
   - Easier to track changes
   - Better visibility of dependencies
   - Simplified release process

4. **Quality Control**
   - Shared testing infrastructure
   - Consistent code style
   - Centralized CI/CD
   - Better dependency management

### Negative

1. **Repository Size**
   - Larger repository size
   - Longer clone times
   - More complex Git operations
   - Higher storage requirements

2. **Build Complexity**
   - More complex build process
   - Longer build times
   - Need for build caching
   - More complex CI/CD setup

3. **Access Control**
   - More complex permissions management
   - Harder to restrict access to specific parts
   - Need for better documentation
   - More complex release management

## Alternatives

### 1. Multiple Repositories

**Pros**:
- Smaller, focused repositories
- Independent versioning
- Simpler access control
- Faster clone times

**Cons**:
- Harder to share code
- More complex dependency management
- Inconsistent tooling
- More complex release process

### 2. Git Submodules

**Pros**:
- Separate repositories
- Version control per module
- Flexible access control
- Reusable across projects

**Cons**:
- Complex Git operations
- Suboptimal developer experience
- Version management issues
- Limited tooling support

### 3. Package Registry

**Pros**:
- Independent versioning
- Clear package boundaries
- Standard package management
- Better dependency control

**Cons**:
- More complex publishing process
- Version management overhead
- Development workflow complexity
- Higher maintenance burden

## Implementation Details

1. **Package Management**
   ```json
   {
     "name": "@omnivore/api",
     "version": "1.0.0",
     "private": true,
     "dependencies": {
       "@omnivore/types": "workspace:*",
       "@omnivore/utils": "workspace:*"
     }
   }
   ```

2. **Build Configuration**
   ```json
   {
     "scripts": {
       "build": "lerna run build",
       "test": "lerna run test",
       "lint": "lerna run lint"
     }
   }
   ```

3. **Development Workflow**
   - Use workspaces for local development
   - Shared TypeScript configuration
   - Unified testing framework
   - Centralized CI/CD pipeline

## Related ADRs

- [TypeScript Across the Stack](./0004-typescript-stack.md)
- [Docker-based Development Environment](./0003-docker-development.md) 