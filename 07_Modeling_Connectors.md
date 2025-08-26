
# Modeling Connectors: Revit, Rhino, Grasshopper, Blender and QGIS

## Navigation card
- Glossary: `./04_Glossary.md`
- Master Index: `./02_Master_Index.md`
- Related: `06_General_Connectors.md`, `12_Common_Patterns.md`

Summary: Complete technical guide of Speckle modeling connectors for AEC applications. Covers I/O principles (Publish/Load), unified DUI3 interface, and specific patterns for Revit (BIM authoring), Rhino (NURBS modeling), Grasshopper (parametric design), Blender (3D modeling/animation), QGIS (GIS analysis). Includes ReceiveMode settings, filters by category/view/selection, property handling, positioning, supported geometric types and limitations by application.

**Technical keywords**: DUI3, Send operation, Receive operation, ReceiveMode, UpdateExistingElements, CreateNewElements, positioning, BIM interoperability, NURBS geometry, parametric design, GIS data exchange, model federation, geometry types, material properties, view filters, category filters, selection filters, version control, commit workflow, multi-application workflows.

## Principle

- Publishing (Send/Publish) and Loading (Receive/Load) create and consume model versions in Speckle projects. Each publication generates a temporary and traceable version of the model.
- The new generation desktop UI is DUI3 and is shared between connectors, with standard actions: login, select project/model, Publish and Load.
- The model hierarchy reflects the origin: there are differences between connectors due to typical flows of each application.
- Versioning allows comparing states, collaborating and, in some legacy connectors, recovering previous versions.

## Guide for the assistant (use in responses)

- Identify the application (Revit, Rhino, Grasshopper, Blender, QGIS) and the user's intention: Publish (send) or Load (load), selection/filtering, properties, positioning.
- Clarify prerequisites: session started in DUI3, project and model selected, adequate permissions in the project.
- For iterative synchronizations, suggest appropriate `ReceiveMode`: `UpdateExistingElements` (synchronization) vs `CreateNewElements` (reference/analysis state).
- If there are positioning issues between apps, recommend publishing with the correct point/origin and reloading.
- Avoid listing compatibilities/versions without evidence; mark as "Known limitation" and refer to `./06_General_Connectors.md`.
- For multi-tool flows, refer to `./12_Common_Patterns.md`.

Sources: AI-ready documents.

## Example

### Example 1 — Send and receive with `speckle-sharp` (C#)

```csharp
using Microsoft.Extensions.DependencyInjection;
using Speckle.Sdk.Api;
using Speckle.Sdk.Models;
using Speckle.Sdk.Serialisation.V2.Send;

// Requires DI to get IOperations; example inspired by local tests
var services = new ServiceCollection()
  .AddSpeckleSdk()
  .BuildServiceProvider();

var operations = services.GetRequiredService<IOperations>();

// 1) Create a simple object (example)
var baseObj = new Base();
baseObj["@items"] = new List<object> { new Point(1, 2, 3), new Point(4, 5, 6) };

// 2) Send to a local transport (you can change to server with credentials)
var localTransport = new MemoryTransport();
var sendResult = await operations.Send(baseObj, localTransport, false);

// 3) Receive by root id
var received = await operations.Receive(sendResult.rootObjId, null, localTransport);

Console.WriteLine($"Received: {((List<object>)received["@items"]).Count} elements");
```

Notes:
- The previous `Send/Receive` pattern is tested in the SDK tests and is reproducible locally.
- To send to Speckle server, configure account/project/model with the SDK's GraphQL client.

### Example 2 — Reception modes in connectors

```csharp
public enum ReceiveMode
{
  // Updates and deletes as appropriate, in addition to creating new ones
  UpdateExistingElements,
  // Ignores updates and doesn't delete; only creates new ones
  CreateNewElements
}
```

Recommended usage:
- UpdateExistingElements: iterative synchronization where id links are preserved.
- CreateNewElements: reference imports or analysis states.

### Example 3 — DUI3: Publish and Load (UI)

Typical steps (common between Revit, Rhino, Grasshopper, Blender, QGIS):
1) Log in to your account (DUI3 UI).
2) Select project and model.
3) Publish: choose what to load/select and publish (creates new version).
4) Load: select project/model/version to load in your modeling session.

Images (documentation reference):
- Publish: `dui_publish_1.jpg` … `dui_publish_5.jpg`
- Load: `dui_load_1.jpg` … `dui_load_5.jpg`

## Application

### Case A — Rhino → Revit (design reference)

Objective: iterate reference geometry from Rhino and consume it in Revit as context or modeling guide.

Steps:
1) In Rhino, use DUI3 → Publish to send selected geometry to a Speckle model.
2) In Revit, use DUI3 → Load to load the latest version as reference.
3) Define the `ReceiveMode` according to purpose: reference (CreateNewElements) or synchronization (UpdateExistingElements).
4) Repeat the cycle publishing new versions from Rhino and loading them in Revit.

Considerations:
- Object hierarchy may differ between connectors, review categories and properties when loading in Revit.
- Coordinate units and origins to minimize displacements; republish if the author adjusts reference points.

### Case B — Revit → QGIS (context and analysis)

Objective: expose elements and attributes from Revit for review and analysis in QGIS (via Speckle).

Steps:
1) In Revit, DUI3 → Publish to send the set of elements (by selection, view or connector filter if applicable).
2) In QGIS, use the Speckle plugin for Load of the model and work with layers/attributes.
3) Version publications by discipline or zone to separate iterations.

Considerations:
- Available attributes depend on the source application; the current structure is more consistent in new generation connectors.
- Reduce attribute cardinality if you need performance in thematic views.

## Limitations / Warnings

- "Recovery" of versions is supported in specific legacy connectors (Rhino, AutoCAD and SketchUp). Verify in your current connector.
- "Canonical views" are predefined; some connectors may publish additional views.
- The model hierarchy reflects the source export and varies by connector (expected differences by flow).
- Position/Coordinates: if you load models published from Revit and observe an offset, ask the author to publish with the correct reference point and reload.

## Best practices / Performance

- Publish in small iterations with limited selection when possible.
- Standardize units and reference points between applications to minimize later adjustments.
- Document naming conventions by discipline and use separate models by purpose (context vs. production).
- Take advantage of `ReceiveMode` to control whether elements are updated or only added.

## Internal references

- Glossary: see `./04_Glossary.md` (terms: stream, branch, commit, object, discussion, token, viewer).
- Master Index: see `./02_Master_Index.md`.
- Related: `./06_General_Connectors.md`, `./12_Common_Patterns.md`

## Checklist before sending (assistant responses)

- The application and operation (Publish/Load) are specified when applicable.
- Appropriate `ReceiveMode` is recommended if applicable.
- Positioning/units considerations are included when applicable.
- Relevant Limitations and Best practices are added.
- Reference is made to related documents (`06`, `12`) and glossary.
- Consistent Speckle terminology (see `./04_Glossary.md`).

## Status and next steps

- Status: first version 0.1, based on AI-ready documents.
- Next steps: add compatibility tables by host app version and local screenshots when available; expand examples by connector (Revit, Rhino, Grasshopper, Blender, QGIS) with precise converter paths.
