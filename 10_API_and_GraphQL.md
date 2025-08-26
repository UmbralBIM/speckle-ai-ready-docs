
# API and GraphQL: authentication, queries and ready examples

## Navigation card
- Glossary: `./04_Glossary.md`
- Master Index: `./02_Master_Index.md`
- Related: `11_PowerBI_3D_Visual_Integrations.md`, `09_Scripting_and_Code_Guide.md`

Summary: Complete guide to Speckle API and GraphQL for developers. Covers authentication with tokens, `/graphql` endpoint, typed queries with .NET SDK, Power Query integration, Python `specklepy`, pagination with cursor/limit, GraphQL error handling, verified comment example (`commentThreads`), project permissions, rate limiting and security best practices.

**Technical keywords**: GraphQL endpoint, authentication token, .NET SDK, specklepy Python, Power Query integration, pagination cursor, limit parameter, GraphQL errors, commentThreads query, project permissions, rate limiting, client reuse, error handling, API versioning, token management, secure authentication, project ID, model ID, commit ID, branch queries.

## Principle

- Speckle's GraphQL endpoint is exposed under the `/graphql` route of the server (see pattern in .NET SDK tests).
- Authentication is done with Speckle account tokens; never expose real tokens in public examples.
- The `.NET` SDK provides typed GraphQL client and utilities; in Power BI you can invoke GraphQL from Power Query with helper functions.
- To validate queries live, you can use the server's GraphQL Explorer: [GraphQL Explorer](https://app.speckle.systems/explorer). Requires login and uses real data.

## Guide for the assistant (use in responses)

- Authentication: use account token but never expose it; prefer secret managers/environment variables. In Python/.NET, authenticate the client and reuse it.
- Endpoint: document that the endpoint is `/graphql` of the corresponding server.
- Choice of approach:
  - .NET SDK (typed resources) for common operations (projects/models/versions) and cursor pagination.
  - Direct GraphQL when flexibility is required (specific fields, `commentThreads` comments).
  - Power Query in BI: use the comment example (`commentThreads`) and expand `items`.
- Pagination: if there's `limit`/`cursor`, include pagination loops.
- Errors: capture and report GraphQL/middleware exceptions; include clear messages.
- Permissions: clarify that seeing a model doesn't imply permission to query/send certain resources.

Sources: AI-ready documents.

## Example

### Example 1 — Query real comments (Power Query)

Verified GraphQL query to get `commentThreads` from a project with all requested metrics.

```graphql
# threadsQuery - Verified real structure
query($projectId: String!) {
    project(id: $projectId) {
        id
        name
        commentThreads {
            totalCount
            items {
                id
                rawText
                createdAt
                updatedAt
                archived
                authorId
                author {
                    name
                    id
                }
                replies {
                    totalCount
                    items {
                        id
                        rawText
                        createdAt
                        authorId
                        author {
                            name
                            id
                        }
                    }
                }
                viewerState
            }
        }
    }
}
```

```powerquery
let
    baseUrl = "https://app.speckle.systems",
    projectId = "YOUR_PROJECT_ID",
    
    threadsQuery = "query($projectId: String!) { project(id: $projectId) { commentThreads { totalCount items { id rawText createdAt updatedAt archived author { name id } replies { totalCount items { id rawText createdAt author { name } } } viewerState } } } }",
    
    vars = [ projectId = projectId ],
    response = Speckle.Api.Fetch(baseUrl, threadsQuery, vars),
    
    // Extract and process threads
    threads = response[project][commentThreads][items],
    threadsTable = Table.FromList(threads, Splitter.SplitByNothing()),
    
    // Expand and calculate metrics (see complete code in Use Cases)
    expandedThreads = Table.ExpandRecordColumn(threadsTable, "Column1", {"id", "rawText", "createdAt", "author", "replies", "archived", "updatedAt", "viewerState"}, {"ThreadId", "RawText", "CreatedAt", "Author", "Replies", "Archived", "UpdatedAt", "ViewerState"})
in
    expandedThreads
```

Usage: Replace `YOUR_PROJECT_ID`, execute in Power Query. Resulting fields: `ThreadId`, `RawText`, `CreatedAt`, `AuthorName`, `ResponseCount`, `Status`, `ActivityLevel`.

### Example 1.1 — Pagination with cursor (.NET SDK, typed resources)

Verified pagination patterns in SDK resources (`WorkspaceResource`, `ProjectResource`, `ModelResource`, `VersionResource`, `CommentResource`): all expose `limit` and `cursor`, and return `items`, `cursor` and `totalCount` when applicable.

```csharp
// Get models from a project with cursor pagination
string? cursor = null;
do
{
  var result = await apiClient.Project.Get(
    projectId: "YOUR_PROJECT_ID",
    modelsLimit: 50,
    modelsCursor: cursor,
    modelsFilter: null,
    cancellationToken: cancellationToken
  );

  foreach (var m in result.models.items)
  {
    Console.WriteLine($"Model: {m.id}");
  }

  cursor = result.models.cursor; // if it's null/doesn't change, ends
}
while (!string.IsNullOrEmpty(cursor));
```

### Example 2 — .NET: minimal GraphQL request with the SDK

```csharp
using GraphQL;
using GraphQL.Client.Http;
using Speckle.Sdk.Api;
using Speckle.Sdk.Credentials;

var uri = new Uri("https://app.speckle.systems");
var account = new Account("user", "YOUR_TOKEN", uri, "server");

// SDK GraphQL client (pattern verified in local tests)
var client = new SpeckleGraphQLClient(
  new SpeckleGraphQLClientDependencies(
    new GraphQLHttpClient(new GraphQLHttpClientOptions { EndPoint = new(uri, "/graphql") }),
    null, null, null
  )
);

// Example query
var response = await client.ExecuteGraphQLRequest<string>(
  new GraphQLRequest("query{ serverInfo { name } }"),
  CancellationToken.None
);
```

Notes:
- Keep the token in environment variables/secret manager.
- The `/graphql` endpoint pattern is validated in SDK tests.

### Example 3 — Error handling (.NET)

```csharp
try
{
  var response = await client.ExecuteGraphQLRequest<string>(
    new GraphQLRequest("query{ serverInfo { name } }"),
    CancellationToken.None
  );
}
catch (SpeckleGraphQLException ex)
{
  // GraphQL errors (FORBIDDEN, UNAUTHENTICATED, INTERNAL_SERVER_ERROR, etc.)
  Console.WriteLine($"GraphQL error: {ex.Message}");
}
catch (Exception ex)
{
  // Other errors (network, timeouts, etc.)
  Console.WriteLine($"Unhandled error: {ex.Message}");
}
```

### Example 4 — Python (`specklepy`): authentication and typed resources

```python
from specklepy.api.client import SpeckleClient
from specklepy.api.credentials import get_default_account

# Initialize client pointing to host
client = SpeckleClient(host="app.speckle.systems")

# Authenticate with local account (Speckle Manager)
account = get_default_account()
client.authenticate_with_account(account)

# Typed operations (examples):
# Get a project
proj = client.project.get("YOUR_PROJECT_ID")

# List models from a project (according to available resources)
# models = client.project.get_models("YOUR_PROJECT_ID", limit=50)  # indicative example

# List versions of a model with cursor pagination
cursor = None
while True:
    versions = client.version.list(project_id="YOUR_PROJECT_ID", model_id="YOUR_MODEL_ID", limit=50, cursor=cursor)
    for v in versions.items:
        print(v.id, v.createdAt)
    if not versions.cursor:
        break
    cursor = versions.cursor
```

Notes:
- The exact structure of methods may vary; consult `specklepy.api.resources.current` in `Data` to correctly sign calls by project/model/version.

### Example 5 — Common operations with the SDK (.NET) without writing GraphQL

When possible, use the SDK's typed resources. Example (pattern used in connectors):

```csharp
// Assuming you already have an apiClient instance (.NET SDK)
var version = await apiClient.Version.Get(versionId: "YOUR_VERSION_ID", projectId: "YOUR_PROJECT_ID", cancellationToken);
// You can also register the reception of a version
await apiClient.Version.Received(new(version.id, "YOUR_PROJECT_ID", sourceApplication: "YOUR_APP"), cancellationToken);
```

### Example 6 — List versions of a model with filter and cursor (.NET SDK)

```csharp
string? cursor = null;
do
{
  var versions = await apiClient.Version.GetVersions(
    projectId: "YOUR_PROJECT_ID",
    modelId: "YOUR_MODEL_ID",
    limit: 50,
    cursor: cursor,
    filter: null,
    cancellationToken: cancellationToken
  );

  foreach (var v in versions.items)
  {
    Console.WriteLine($"Version: {v.id} createdAt: {v.createdAt}");
  }

  cursor = versions.cursor;
}
while (!string.IsNullOrEmpty(cursor));
```

## Application

### Case A — List recent comments from a project (BI and reporting)

1) Use the comment example (`commentThreads`) to get `items`.
2) Expand to columns in Power Query and build visualizations by `createdAt`/author.
3) Filter `limit` according to need and add pagination if greater volume is required.

### Case B — Basic telemetry with .NET

1) Execute a lightweight query (e.g., `serverInfo`) to verify endpoint GraphQL connectivity and latency.
2) Record times and errors for observability.

## Checklist before sending (assistant responses)

- The response follows Principle → Example → Application.
- Uses placeholders (`YOUR_PROJECT_ID`, `YOUR_TOKEN`) and correct labels (`graphql`, `powerquery`, `csharp`, `python`).
- Includes pagination by `cursor` when applicable.
- Error handling mentioned (GraphQL exceptions / retries if applicable).
- Comment example (`commentThreads`) is referenced when BI requires it.
- Consistent terminology (see `./04_Glossary.md`).

## Limitations / Warnings

- Permissions: seeing a model on the web doesn't imply being able to load/query certain resources; requires adequate role.
- Pagination: respect `limit` limits and apply successive pages if you need more results.
- Tokens: don't share tokens in repos or reports; use *placeholders* in examples.
- Schema: consult SDK resources (`Resources/*`) to avoid assumptions of non-existent fields.

## Best practices / Performance

- Use small and specific queries; avoid requesting unnecessary fields.
- Implement retries and error handling (see `GraphQLErrorHandler` tests).
- Cache results when applicable (e.g., server metadata) to reduce calls.
- Document the server, `/graphql` endpoint and schema version used.
- Prefer typed SDK resources when they exist (projects, models, versions) and leave raw GraphQL for advanced cases.
- Implement `cursor`-based pagination following SDK resources to avoid massive loads in a single call.

## Internal references

- Glossary: `./04_Glossary.md` (terms: project, model, version, object, commentThreads, token).
- Power BI + 3D: `./11_PowerBI_3D_Visual_Integrations.md` (reuses the `commentThreads` comment example).
- Patterns: `./12_Common_Patterns.md`.

## Status and next steps

- Status: v0.1 with minimal examples in .NET and Power Query.
- Next steps: add examples of frequent queries (projects, models and versions) referencing local SDK types/resources.
