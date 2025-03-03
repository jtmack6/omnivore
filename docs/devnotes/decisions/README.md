# Architecture Decision Records (ADRs)

**Date**: 2024-03-03
**Author**: Development Team

## Overview

This directory contains Architecture Decision Records (ADRs) for the Omnivore project. Each ADR documents a significant architectural decision made in the project, including the context, consequences, and alternatives considered.

## ADR Structure

Each ADR follows this structure:

```markdown
# [Title]

**Date**: YYYY-MM-DD
**Status**: [Proposed | Accepted | Deprecated | Superseded]
**Context**: What is the issue that we're seeing that is motivating this decision?
**Decision**: What is the change that we're proposing and/or doing?
**Consequences**: What becomes easier or more difficult to do because of this change?
**Alternatives**: What other approaches were considered and why were they not chosen?
```

## ADR Status

- **Proposed**: Initial state of an ADR
- **Accepted**: Decision has been implemented
- **Deprecated**: Decision has been replaced or removed
- **Superseded**: Decision has been replaced by a newer ADR

## Current ADRs

1. [GraphQL as Primary API Interface](./0001-graphql-as-primary-api.md)
2. [Monorepo Structure](./0002-monorepo-structure.md)
3. [Docker-based Development Environment](./0003-docker-development.md)
4. [TypeScript Across the Stack](./0004-typescript-stack.md)
5. [Real-time Updates with GraphQL Subscriptions](./0005-graphql-subscriptions.md)
6. [Image Processing Service](./0006-image-processing.md)
7. [Machine Learning Integration](./0007-ml-integration.md)

## Contributing

When creating a new ADR:

1. Create a new file with the next sequential number (e.g., `0008-title.md`)
2. Follow the ADR structure template
3. Include relevant diagrams or code examples
4. Link to related ADRs
5. Update this README with the new ADR

## Additional Resources

- [Architecture Overview](../architecture/system-overview.md)
- [API Documentation](../api/README.md)
- [Development Setup](../setup/development-environment.md) 