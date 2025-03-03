# GraphQL Schema Reference

**Date**: 2024-03-03
**Author**: Development Team

## Core Types

### User

```graphql
type User {
  id: ID!
  email: String!
  name: String
  profile: Profile
  preferences: UserPreferences
  createdAt: DateTime!
  updatedAt: DateTime!
}

type Profile {
  username: String!
  bio: String
  picture: String
  socialLinks: [SocialLink!]
}

type UserPreferences {
  theme: Theme
  fontSize: Int
  lineHeight: Float
  margins: Int
  fontFamily: String
  readingDirection: ReadingDirection
}

enum Theme {
  LIGHT
  DARK
  SYSTEM
}

enum ReadingDirection {
  LTR
  RTL
}
```

### Article

```graphql
type Article {
  id: ID!
  title: String!
  url: String!
  description: String
  content: String
  savedAt: DateTime!
  readAt: DateTime
  archivedAt: DateTime
  labels: [Label!]!
  highlights: [Highlight!]!
  author: String
  publishedAt: DateTime
  siteName: String
  image: String
  wordCount: Int
  readingTime: Int
  isArchived: Boolean!
  isFavorite: Boolean!
  isPublic: Boolean!
  createdAt: DateTime!
  updatedAt: DateTime!
}

type Label {
  id: ID!
  name: String!
  color: String
  description: String
  createdAt: DateTime!
  updatedAt: DateTime!
}
```

### Highlight

```graphql
type Highlight {
  id: ID!
  quote: String!
  annotation: String
  article: Article!
  createdAt: DateTime!
  updatedAt: DateTime!
  position: Int
  color: String
  note: String
}
```

### Library

```graphql
type Library {
  id: ID!
  name: String!
  description: String
  articles: ArticleConnection!
  labels: [Label!]!
  createdAt: DateTime!
  updatedAt: DateTime!
}

type ArticleConnection {
  edges: [ArticleEdge!]!
  pageInfo: PageInfo!
}

type ArticleEdge {
  cursor: String!
  node: Article!
}

type PageInfo {
  hasNextPage: Boolean!
  hasPreviousPage: Boolean!
  startCursor: String
  endCursor: String
}
```

## Queries

### User Queries

```graphql
type Query {
  me: User!
  user(id: ID!): User
  users: [User!]!
}
```

### Article Queries

```graphql
type Query {
  article(id: ID!): Article
  articles(
    first: Int
    after: String
    last: Int
    before: String
    filter: ArticleFilter
    sort: ArticleSort
  ): ArticleConnection!
  searchArticles(
    query: String!
    first: Int
    after: String
  ): ArticleConnection!
}

input ArticleFilter {
  isArchived: Boolean
  isFavorite: Boolean
  label: ID
  dateRange: DateRange
}

input DateRange {
  start: DateTime
  end: DateTime
}

enum ArticleSort {
  SAVED_AT
  READ_AT
  PUBLISHED_AT
  TITLE
}
```

### Library Queries

```graphql
type Query {
  library(id: ID!): Library
  libraries: [Library!]!
}
```

## Mutations

### Article Mutations

```graphql
type Mutation {
  createArticle(input: CreateArticleInput!): Article!
  updateArticle(input: UpdateArticleInput!): Article!
  deleteArticle(id: ID!): Boolean!
  archiveArticle(id: ID!): Article!
  unarchiveArticle(id: ID!): Article!
  favoriteArticle(id: ID!): Article!
  unfavoriteArticle(id: ID!): Article!
}

input CreateArticleInput {
  url: String!
  title: String
  description: String
  labels: [ID!]
  isPublic: Boolean
}

input UpdateArticleInput {
  id: ID!
  title: String
  description: String
  labels: [ID!]
  isPublic: Boolean
}
```

### Highlight Mutations

```graphql
type Mutation {
  createHighlight(input: CreateHighlightInput!): Highlight!
  updateHighlight(input: UpdateHighlightInput!): Highlight!
  deleteHighlight(id: ID!): Boolean!
}

input CreateHighlightInput {
  articleId: ID!
  quote: String!
  annotation: String
  position: Int
  color: String
  note: String
}

input UpdateHighlightInput {
  id: ID!
  annotation: String
  position: Int
  color: String
  note: String
}
```

### Label Mutations

```graphql
type Mutation {
  createLabel(input: CreateLabelInput!): Label!
  updateLabel(input: UpdateLabelInput!): Label!
  deleteLabel(id: ID!): Boolean!
}

input CreateLabelInput {
  name: String!
  color: String
  description: String
}

input UpdateLabelInput {
  id: ID!
  name: String
  color: String
  description: String
}
```

## Subscriptions

```graphql
type Subscription {
  articleUpdated(id: ID!): Article!
  articleDeleted(id: ID!): ID!
  highlightCreated(articleId: ID!): Highlight!
  highlightUpdated(id: ID!): Highlight!
  highlightDeleted(id: ID!): ID!
}
```

## Scalar Types

```graphql
scalar DateTime
scalar JSON
scalar Upload
```

## Additional Resources

- [API Overview](./README.md)
- [Authentication Guide](./authentication.md)
- [API Examples](./examples.md) 