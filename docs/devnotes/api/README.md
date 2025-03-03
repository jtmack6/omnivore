# API Documentation

**Date**: 2024-03-03
**Author**: Development Team

## Overview

Omnivore uses GraphQL as its primary API interface, providing a flexible and efficient way to interact with the service. This documentation covers the API structure, authentication, and common operations.

## Authentication

All API requests require authentication using a Bearer token. Include the token in the Authorization header:

```http
Authorization: Bearer YOUR_API_TOKEN
```

To obtain an API token:
1. Log in to your Omnivore account
2. Navigate to Settings → API
3. Generate a new API token

## GraphQL Endpoint

The main GraphQL endpoint is available at:
```
https://api.omnivore.app/graphql
```

## API Structure

### Core Types

- `User` - User account information
- `Article` - Saved article content
- `Label` - Article categorization
- `Highlight` - Text highlights and annotations
- `Library` - User's article library
- `SearchResult` - Search query results

### Common Operations

1. **Article Management**
   - Save articles
   - Update article metadata
   - Delete articles
   - Archive/unarchive articles

2. **Library Organization**
   - Create/update labels
   - Organize articles
   - Manage subscriptions
   - Search and filter

3. **Content Interaction**
   - Create highlights
   - Add notes
   - Share articles
   - Export content

## Example Queries

### Fetching Articles

```graphql
query GetArticles {
  articles {
    edges {
      node {
        id
        title
        url
        description
        savedAt
        readAt
        labels {
          name
        }
      }
    }
  }
}
```

### Creating a Highlight

```graphql
mutation CreateHighlight($input: CreateHighlightInput!) {
  createHighlight(input: $input) {
    highlight {
      id
      quote
      annotation
      article {
        id
        title
      }
    }
  }
}
```

## Rate Limiting

API requests are subject to rate limiting:
- 100 requests per minute per API token
- 1000 requests per hour per API token

Rate limit headers are included in responses:
```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1614787200
```

## Error Handling

GraphQL errors follow the standard format:
```json
{
  "errors": [
    {
      "message": "Error description",
      "locations": [
        {
          "line": 2,
          "column": 3
        }
      ],
      "path": ["fieldName"],
      "code": "ERROR_CODE"
    }
  ]
}
```

Common error codes:
- `UNAUTHORIZED` - Authentication required
- `FORBIDDEN` - Insufficient permissions
- `NOT_FOUND` - Resource doesn't exist
- `VALIDATION_ERROR` - Invalid input
- `RATE_LIMIT_EXCEEDED` - Too many requests

## Best Practices

1. **Query Optimization**
   - Request only needed fields
   - Use pagination for large result sets
   - Implement proper error handling

2. **Authentication**
   - Keep API tokens secure
   - Rotate tokens regularly
   - Use environment variables for tokens

3. **Rate Limiting**
   - Implement exponential backoff
   - Cache responses when possible
   - Monitor rate limit headers

## Additional Resources

- [GraphQL Schema Reference](./schema.md)
- [Authentication Guide](./authentication.md)
- [Rate Limiting Details](./rate-limiting.md)
- [Error Handling Guide](./error-handling.md)
- [API Examples](./examples.md) 