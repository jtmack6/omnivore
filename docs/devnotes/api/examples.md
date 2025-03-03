# API Usage Examples

**Date**: 2024-03-03
**Author**: Development Team

## Common Use Cases

### 1. Saving an Article

```graphql
mutation SaveArticle {
  createArticle(input: {
    url: "https://example.com/article"
    title: "Example Article"
    description: "A great article about example"
    labels: ["label-id-1", "label-id-2"]
    isPublic: false
  }) {
    id
    title
    url
    description
    labels {
      name
    }
    savedAt
  }
}
```

### 2. Fetching User's Library

```graphql
query GetLibrary {
  libraries {
    id
    name
    description
    articles(first: 10) {
      edges {
        node {
          id
          title
          url
          savedAt
          labels {
            name
          }
        }
      }
      pageInfo {
        hasNextPage
        endCursor
      }
    }
    labels {
      id
      name
      color
    }
  }
}
```

### 3. Creating a Highlight

```graphql
mutation CreateHighlight {
  createHighlight(input: {
    articleId: "article-id-123"
    quote: "This is an important quote"
    annotation: "This is my note about the quote"
    position: 42
    color: "#FF0000"
    note: "Additional context about this highlight"
  }) {
    id
    quote
    annotation
    position
    color
    note
    article {
      id
      title
    }
  }
}
```

### 4. Searching Articles

```graphql
query SearchArticles {
  searchArticles(
    query: "machine learning"
    first: 10
    after: "cursor-123"
  ) {
    edges {
      node {
        id
        title
        url
        description
        savedAt
        labels {
          name
        }
      }
      cursor
    }
    pageInfo {
      hasNextPage
      endCursor
    }
  }
}
```

### 5. Managing Labels

```graphql
# Create a new label
mutation CreateLabel {
  createLabel(input: {
    name: "Research"
    color: "#4CAF50"
    description: "Articles for research purposes"
  }) {
    id
    name
    color
    description
  }
}

# Update an existing label
mutation UpdateLabel {
  updateLabel(input: {
    id: "label-id-123"
    name: "Research Papers"
    color: "#2196F3"
  }) {
    id
    name
    color
    description
  }
}
```

### 6. Article Organization

```graphql
# Archive an article
mutation ArchiveArticle {
  archiveArticle(id: "article-id-123") {
    id
    isArchived
    archivedAt
  }
}

# Favorite an article
mutation FavoriteArticle {
  favoriteArticle(id: "article-id-123") {
    id
    isFavorite
  }
}
```

### 7. Real-time Updates

```graphql
# Subscribe to article updates
subscription OnArticleUpdate {
  articleUpdated(id: "article-id-123") {
    id
    title
    isArchived
    isFavorite
    labels {
      name
    }
  }
}

# Subscribe to new highlights
subscription OnNewHighlight {
  highlightCreated(articleId: "article-id-123") {
    id
    quote
    annotation
    position
    color
  }
}
```

## Error Handling Examples

### 1. Handling Authentication Errors

```typescript
try {
  const response = await fetch('https://api.omnivore.app/graphql', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${token}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      query: GET_ARTICLES_QUERY
    })
  });

  const data = await response.json();
  
  if (data.errors) {
    data.errors.forEach(error => {
      if (error.code === 'UNAUTHORIZED') {
        // Handle authentication error
        console.error('Authentication failed:', error.message);
      }
    });
  }
} catch (error) {
  console.error('Request failed:', error);
}
```

### 2. Handling Rate Limiting

```typescript
async function fetchWithRetry(query: string, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    const response = await fetch('https://api.omnivore.app/graphql', {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${token}`,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ query })
    });

    const data = await response.json();

    if (data.errors) {
      const rateLimitError = data.errors.find(
        error => error.code === 'RATE_LIMIT_EXCEEDED'
      );

      if (rateLimitError) {
        // Wait with exponential backoff
        await new Promise(resolve => 
          setTimeout(resolve, Math.pow(2, i) * 1000)
        );
        continue;
      }
    }

    return data;
  }

  throw new Error('Max retries exceeded');
}
```

## Best Practices

1. **Pagination**
   - Always use pagination for large result sets
   - Implement infinite scroll or "Load More" functionality
   - Store cursors for efficient navigation

2. **Error Handling**
   - Implement proper error handling for all API calls
   - Show user-friendly error messages
   - Log errors for debugging

3. **Caching**
   - Cache responses when appropriate
   - Implement optimistic updates
   - Use proper cache invalidation strategies

4. **Performance**
   - Request only needed fields
   - Use batch operations when possible
   - Implement proper loading states

## Additional Resources

- [API Overview](./README.md)
- [Schema Reference](./schema.md)
- [Authentication Guide](./authentication.md)
- [Rate Limiting Details](./rate-limiting.md) 