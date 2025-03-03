# Development Environment Setup

**Date**: 2024-03-03
**Author**: Development Team

## Prerequisites

Before setting up the development environment, ensure you have the following installed:

- Node.js (version specified in `.node-version`)
- Yarn package manager
- Docker and Docker Compose
- Git
- Make (for using Makefile commands)

## Initial Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/omnivore-app/omnivore.git
   cd omnivore
   ```

2. Install dependencies:
   ```bash
   yarn install
   ```

3. Set up environment variables:
   ```bash
   cp .env.example .env
   ```
   Edit `.env` with your specific configuration values.

## Docker Setup

The project uses Docker for development. Start the required services:

```bash
docker-compose up -d
```

This will start:
- Database services
- Redis cache
- Image proxy service
- Other required infrastructure

## Development Workflow

### Starting the Development Server

1. Start the backend services:
   ```bash
   yarn dev:server
   ```

2. Start the frontend development server:
   ```bash
   yarn dev:client
   ```

### Running Tests

```bash
# Run all tests
yarn test

# Run specific test suite
yarn test:unit
yarn test:integration
yarn test:e2e
```

### Database Management

```bash
# Run migrations
yarn db:migrate

# Reset database
yarn db:reset

# Seed database with test data
yarn db:seed
```

## Project Structure

The project is organized as a monorepo with the following main directories:

- `packages/` - Shared packages and modules
- `pkg/` - Additional packages
- `docs/` - Documentation
- `ml/` - Machine learning components
- `apple/` - iOS/macOS specific code
- `android/` - Android specific code
- `imageproxy/` - Image proxy service
- `self-hosting/` - Self-hosting configurations

## Common Development Tasks

### Adding New Dependencies

```bash
# Add a dependency to a specific package
yarn workspace @omnivore/package-name add dependency-name

# Add a dev dependency
yarn workspace @omnivore/package-name add -D dependency-name
```

### Building the Project

```bash
# Build all packages
yarn build

# Build specific package
yarn workspace @omnivore/package-name build
```

### Code Generation

```bash
# Generate GraphQL types
yarn codegen

# Generate API documentation
yarn docs:api
```

## Troubleshooting

Common issues and their solutions are documented in the [troubleshooting guide](../troubleshooting/common-issues.md).

## Additional Resources

- [Architecture Overview](../architecture/system-overview.md)
- [API Documentation](../api/README.md)
- [Contributing Guidelines](../../CONTRIBUTING.md)
- [Project README](../../README.md) 