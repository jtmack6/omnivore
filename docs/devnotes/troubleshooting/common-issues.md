# Common Development Issues and Solutions

**Date**: 2024-03-03
**Author**: Development Team

## Docker Issues

### Container Won't Start

**Problem**: Docker containers fail to start with port conflicts or resource issues.

**Solution**:
1. Check for running containers:
   ```bash
   docker ps -a
   ```
2. Stop conflicting containers:
   ```bash
   docker-compose down
   ```
3. Ensure required ports are available:
   - Database: 5432
   - Redis: 6379
   - API: 3000
   - Frontend: 3001

### Database Connection Issues

**Problem**: Cannot connect to the database from the application.

**Solution**:
1. Verify database container is running:
   ```bash
   docker-compose ps
   ```
2. Check database logs:
   ```bash
   docker-compose logs db
   ```
3. Ensure environment variables are correct in `.env`

## Node.js/Yarn Issues

### Dependency Installation Fails

**Problem**: `yarn install` fails with network or dependency conflicts.

**Solution**:
1. Clear yarn cache:
   ```bash
   yarn cache clean
   ```
2. Remove node_modules and yarn.lock:
   ```bash
   rm -rf node_modules yarn.lock
   ```
3. Reinstall dependencies:
   ```bash
   yarn install
   ```

### TypeScript Compilation Errors

**Problem**: TypeScript compilation fails with type errors.

**Solution**:
1. Run type checking:
   ```bash
   yarn type-check
   ```
2. Update type definitions:
   ```bash
   yarn update-types
   ```
3. Check for missing type declarations in `package.json`

## Development Server Issues

### Hot Reload Not Working

**Problem**: Changes to code don't trigger automatic reload.

**Solution**:
1. Check if file watching is enabled in your IDE
2. Verify the development server is running with the correct flags
3. Try restarting the development server

### API Requests Failing

**Problem**: API requests return errors or timeouts.

**Solution**:
1. Check API server logs
2. Verify environment variables
3. Ensure all required services are running
4. Check network connectivity between services

## Mobile Development Issues

### iOS Build Fails

**Problem**: iOS app fails to build or run.

**Solution**:
1. Update CocoaPods:
   ```bash
   cd apple
   pod install
   ```
2. Clean build folder:
   ```bash
   xcodebuild clean
   ```
3. Check for missing certificates or provisioning profiles

### Android Build Issues

**Problem**: Android app fails to build or run.

**Solution**:
1. Clean Gradle build:
   ```bash
   cd android
   ./gradlew clean
   ```
2. Update Gradle dependencies
3. Check Android SDK installation

## Getting Help

If you're still experiencing issues:

1. Check the [GitHub Issues](https://github.com/omnivore-app/omnivore/issues)
2. Join the development community chat
3. Create a new issue with:
   - Detailed error message
   - Steps to reproduce
   - Environment information
   - Relevant logs

## Additional Resources

- [Development Environment Setup](../setup/development-environment.md)
- [API Documentation](../api/README.md)
- [Architecture Overview](../architecture/system-overview.md) 