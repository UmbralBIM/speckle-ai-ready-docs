
# Scripting and code guide: specklepy and speckle-sharp

## Navigation card
- Glossary: `./04_Glossary.md`
- Master Index: `./02_Master_Index.md`
- Related: `10_API_and_GraphQL.md`, `07_Modeling_Connectors.md`

Summary: Practical cookbook to automate send/receive operations, work with projects/models/versions and manipulate objects with `specklepy` (Python) and `speckle-sharp` (C#). Includes reproducible examples with *placeholders* and references to AI-ready documents.

## Principle

- Two main SDKs: `specklepy` (Python) and `speckle-sharp` (.NET/C#) with send/receive capabilities, GraphQL and model operations.
- Common pattern: build objects → serialize/send → get root id → receive/transform → register version.
- Use secure local credentials and never expose real tokens in shared code.

## Guide for the assistant (use in responses)

- Choose SDK according to context: `specklepy` (Python, scripting/data) vs `speckle-sharp` (.NET/C#, desktop apps/services).
- Authentication: prefer `authenticate_with_account` (Speckle Manager). Avoid embedding tokens; use secrets/environment variables.
- Host: use `app.speckle.systems` (or the host indicated by the user) in the client.
- Transports: `MemoryTransport` for minimal examples; for production, indicate project/model/version when applicable.
- Versions and pagination: refer to `./10_API_and_GraphQL.md` for typed resource patterns and cursor pagination.
- Errors: recommend explicit exception handling and logs (especially in .NET, see GraphQL handling).

## Example

### Example 1 — Python: send and receive with `specklepy`

```python
from specklepy.api.client import SpeckleClient
from specklepy.api.credentials import get_default_account
from specklepy.objects.base import Base
from specklepy.transports.memory import MemoryTransport

# Configure client and account (placeholder)
client = SpeckleClient(host="app.speckle.systems")
account = get_default_account()
client.authenticate_with_account(account)

# Create base object
root = Base()
root["@items"] = [
    {"x": 1, "y": 2, "z": 3},
    {"x": 4, "y": 5, "z": 6},
]

# Send to memory transport (minimal example)
mem = MemoryTransport()
obj_id = client.object.create(None, root, [mem])  # placeholder of stream/model if applicable

# Receive by id
received = client.object.get(None, obj_id, mem)
print("Received items:", len(received["@items"]))
```

Notes:
- Adjust to real usage of `project/model/version` according to `specklepy.core.api.client` and `current` resources.

### Example 2 — C#: `Send/Receive` operations with `speckle-sharp`

```csharp
using Microsoft.Extensions.DependencyInjection;
using Speckle.Sdk.Api;
using Speckle.Sdk.Models;
using Speckle.Sdk.Transports;

var services = new ServiceCollection().AddSpeckleSdk().BuildServiceProvider();
var operations = services.GetRequiredService<IOperations>();

var root = new Base();
root["@items"] = new List<object> { new Point(1,2,3), new Point(4,5,6) };

var mem = new MemoryTransport();
var sendResult = await operations.Send(root, mem, false);

var received = await operations.Receive(sendResult.rootObjId, null, mem);
Console.WriteLine(((List<object>)received["@items"]).Count);
```

### Example 3 — C#: Basic GraphQL with the SDK

```csharp
using GraphQL;
using GraphQL.Client.Http;
using Speckle.Sdk.Api;
using Speckle.Sdk.Credentials;

var uri = new Uri("https://app.speckle.systems");
var account = new Account("user", "token", uri, "server");
var client = new SpeckleGraphQLClient(
  new SpeckleGraphQLClientDependencies(
    new GraphQLHttpClient(new GraphQLHttpClientOptions{ EndPoint = new(uri, "/graphql") }),
    null, null, null
  )
);

var response = await client.ExecuteGraphQLRequest<string>(new GraphQLRequest("query{ serverInfo { name } }"), CancellationToken.None);
```

## Application

### Case A — Python pipeline: generate geometry, send, register version

1) Build objects with `specklepy.objects.*`.
2) Send via `client.object.create(...)` with server or memory transport.
3) Use `current` resources (`project`, `model`, `version`) to register metadata.

### Case B — .NET service: iterative synchronization with ReceiveMode

1) Process new versions, receive and update according to `ReceiveMode` (when applicable to connectors).
2) Register activities and errors with SDK utilities.

## Limitations / Warnings

- Examples with *memory transport* are minimal; for server, accounts and project/model paths are required.
- Avoid exposing real tokens; use environment variables/secret managers.
- Object structures may vary by origin; validate units and hierarchies before automating transformations.

## Best practices / Performance

- Serialize in batches and limit graph depth when you don't need everything.
- Reuse transports and clients.
- Add logs and measurements (serialization/transfer times) to detect bottlenecks.

## Internal references

- Glossary: `./04_Glossary.md`.
- API and GraphQL: `./10_API_and_GraphQL.md`.
- Connectors (Publish/Load context): `./07_Modeling_Connectors.md`.

## Checklist before sending (assistant responses)

- The response follows Principle → Example → Application.
- Examples with placeholders; do not expose real tokens/IDs.
- Appropriate SDK is suggested to the context (Python vs .NET).
- Secure authentication and correct host are mentioned.
- Reference is made to `10` (pagination/GraphQL) and `07` (Publish/Load) when applicable.
- Consistent Speckle terminology (see `./04_Glossary.md`).

## Status and next steps

- Status: v0.1 with base Python/C# examples and references to AI-ready documents.
- Next steps: expand with `specklepy.core.api.resources.current` for CRUD of projects/models/versions; add examples of `sqlite` and `server` transports.
