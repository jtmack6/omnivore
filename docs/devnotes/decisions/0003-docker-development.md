# Docker-based Development Environment

**Date**: 2024-03-03
**Status**: Accepted
**Context**: We need to provide a consistent development environment across different operating systems and ensure all developers have the same dependencies and services available locally.

## Decision

We will use Docker and Docker Compose for the development environment, providing:
- Containerized services (database, Redis, image proxy)
- Consistent Node.js environment
- Isolated development dependencies
- Easy service management
- Reproducible builds

## Consequences

### Positive

1. **Environment Consistency**
   - Same environment for all developers
   - No "it works on my machine" issues
   - Easy onboarding for new developers
   - Consistent service versions

2. **Service Isolation**
   - Each service runs in its own container
   - No conflicts with local installations
   - Easy to start/stop services
   - Clean development environment

3. **Dependency Management**
   - No need to install services locally
   - Easy version management
   - Consistent Node.js version
   - Isolated package installations

4. **Development Workflow**
   - Simple service orchestration
   - Easy debugging
   - Quick environment reset
   - Consistent build process

### Negative

1. **Resource Usage**
   - Higher memory usage
   - Increased disk space requirements
   - Potential performance overhead
   - More complex resource management

2. **Learning Curve**
   - Team needs to learn Docker
   - More complex debugging
   - Additional tooling required
   - New workflow adaptation

3. **Development Speed**
   - Initial container build time
   - Volume mounting overhead
   - Hot reload complexity
   - Build caching complexity

## Alternatives

### 1. Local Installation

**Pros**:
- Direct access to services
- Lower resource usage
- Simpler debugging
- Faster development cycle

**Cons**:
- Inconsistent environments
- Complex dependency management
- Version conflicts
- Difficult onboarding

### 2. Vagrant

**Pros**:
- Full VM environment
- Consistent OS
- Familiar to some developers
- Good isolation

**Cons**:
- Heavy resource usage
- Slower than containers
- More complex setup
- Limited service isolation

### 3. Cloud Development Environment

**Pros**:
- No local setup required
- Consistent environment
- Easy collaboration
- Scalable resources

**Cons**:
- Internet dependency
- Higher latency
- More complex security
- Cost considerations

## Implementation Details

1. **Docker Compose Configuration**
   ```yaml
   version: '3.8'
   services:
     db:
       image: postgres:14
       environment:
         POSTGRES_USER: omnivore
         POSTGRES_PASSWORD: development
       ports:
         - "5432:5432"
     
     redis:
       image: redis:6
       ports:
         - "6379:6379"
     
     imageproxy:
       build: ./imageproxy
       ports:
         - "8080:8080"
   ```

2. **Development Workflow**
   ```bash
   # Start services
   docker-compose up -d
   
   # Install dependencies
   yarn install
   
   # Start development server
   yarn dev
   ```

3. **Volume Management**
   - Node modules in containers
   - Source code mounted from host
   - Persistent database storage
   - Shared cache volumes

## Related ADRs

- [Monorepo Structure](./0002-monorepo-structure.md)
- [TypeScript Across the Stack](./0004-typescript-stack.md) 