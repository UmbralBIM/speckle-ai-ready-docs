
# Common AEC integration patterns with Speckle

## Navigation card
- Glossary: `./04_Glossary.md`
- Master Index: `./02_Master_Index.md`
- Related: `07_Modeling_Connectors.md`, `14_Best_Practices.md`

Summary: Cookbook of typical flows between tools (publication/loading with DUI3, federation, reporting in Power BI and visibility control in 3D). Includes reproducible examples and performance recommendations, with sources in AI-ready documents.

## Principle

- Connectors publish (Publish) and load (Load) models, generating traceable versions. The DUI3 interface standardizes actions and filters by view/category/selection according to the app.
- Federation allows combining models from different disciplines and projects in the same view (web and BI), without merging them.
- In Power BI, the connector loads tabular data and the 3D Visual allows coloring and interacting with the model.

## Guide for the assistant (use in responses)

- Identify the applications involved (e.g., Rhino→Revit, Revit→Power BI, Archicad, Navisworks, Grasshopper) and the user's objective (reference, coordination, analysis).
- Clarify prerequisites: DUI3 active (session started), permissions in the project, selection of project/model/version.
- For flows with connectors, refer to `./07_Modeling_Connectors.md`; for BI/tabular/3D, refer to `./08_Tabular_Data_Integrations.md` and `./11_PowerBI_3D_Visual_Integrations.md`.
- If there are doubts about compatibility/operations by app, refer to `./06_General_Connectors.md` and mark as "Known limitation".
- Include performance recommendations: filters by view/category/selection, selective flattening, federation by discipline/area.

Sources: AI-ready documents.

## Example

### Pattern 1 — Rhino → Revit (design reference)

1) In Rhino: DUI3 → Publish of selected geometry to a Speckle model.
2) In Revit: DUI3 → Load of the latest version as reference.
3) Iterate: publish new versions and reload them according to progress.
4) If the model loads offset, ask for re-publication from Revit with the correct reference point.

### Pattern 2 — Revit → Power BI (tabular + 3D)

1) In Revit: DUI3 → Publish (use filters by view/category if applicable).
2) In Power BI Desktop: "Get Data" → search "Speckle" → "Connect to Speckle" → "Connect".
3) Import the 3D Visual: Visualizations → "…" → "Import a visual from a file" → `Speckle 3D Visual.pbiviz`.
4) Configure the 3D Visual:
   - `Version Object ID` → required field for visualization.
   - `Object IDs` → required field for interactivity.
   - `Tooltip` → any column.
   - `Color by` → any column for coloring.
5) Flatten properties if necessary in Power Query:

```powerquery
// Example of flattening with helpers
let
  Source = ModelTable,
  WithProps = Table.AddColumn(Source, "props", each Speckle.Objects.Properties([object], {"name", "category", "level", "type"})),
  Expanded = Speckle.Utils.ExpandRecord(WithProps, "props")
in
  Expanded
```

6) Control visibility of non-selected with the "ghost" icon of the 3D Visual (ghosted/hidden).

### Pattern 3 — Multidisciplinary federation (web and BI)

1) On the web: create federated views by aggregating models by discipline (architecture, structure, MEP) in the same view.
2) In Power BI: manual federation of already loaded tables with helper function:

```powerquery
let
  Federated = Speckle.Models.Federate({ModelA, ModelB, ModelC}, false)
in
  Federated
```

Tips:
- `excludeData = true` to federate only metadata necessary for the 3D Visual (Version Object ID, Object IDs) without bringing all data.
- You can federate models from different projects.

### Pattern 4 — Archicad → Speckle (GDL as reference + performance)

1) Publish from Archicad using filters by **Views** or **Element Types** when applicable.
2) To speed up publications, disable property extraction if not required (Send Settings → Include Object Properties = off).
3) Load in Archicad: objects are received as generic GDL (mesh + render material if exists), organized in the Embedded Library.
4) In plan views, GDL generates cut lines; 2D projections are not shown.

### Pattern 5 — Navisworks → Speckle (properties and sets)

1) Publish visible objects (NWC, DWG, IFC, RVT) with attached geometry and properties.
2) Use selection/search sets as publication filters (sets are not preserved as named sets, but serve to select what to send).
3) Positioning: the coordinate system defined in Navisworks is used. For offset, apply transformation in the connector before publishing or align with a shared system.

### Pattern 6 — Grasshopper ↔ Speckle (publication and loading in GH)

1) Publish from Speckle nodes in Grasshopper; the connector is packaged with Rhino.
2) Documentation and examples of nodes are installed with the connector (Grasshopper → File → Special Folders → Components Folder → Speckle Sample Files).
3) To load, select project/model/version from the "Load" nodes and connect their output to collections for processing.
4) To access fields not present in GH properties, use the "Deconstruct" node.

### Pattern 7 — Collaboration dashboard (models + comments)

1) Load project models with the standard Power BI connector.
2) Load comments with independent GraphQL query (`commentThreads`).
3) Relate both tables using Project ID as common key.
4) Create combined metrics: model activity vs. comment activity.
5) Visualize integrated timeline of model changes and comments.

Considerations:
- Comments have `viewerState` that can relate to specific model objects.
- Use `archived` to filter active vs. closed comments.
- `replies.totalCount` indicates discussion level per thread.

## Limitations / Warnings

- Canonical views: they are predefined; connectors can publish additional views, but new canonical views are not defined from the user.
- Permissions: seeing on the web doesn't imply being able to load in connectors/Power BI; requires adequate role in the project.
- Positioning (Revit): if the load appears offset, publish from Revit with the correct reference point.
- Publication performance: disabling property extraction can speed up (according to connector, see options).
- Power BI: if the URL contains "@", loads a fixed version; won't follow the latest until changed.
- Connector/service: the Power BI connector requires "Speckle Desktop Service" active.

## Best practices / Performance

- Publish/load by stages: use publication filters (view/category/selection) to reduce volume.
- Control cardinality: isolate and flatten only necessary properties (helpers in Power Query).
- Strategic federation: divide heavy models by discipline/area to maintain viewer performance.
- Standardize reference points and units between applications to minimize reprocessing.
- Document URLs and versions used for traceability.

## Internal references

- Glossary: `./04_Glossary.md`.
- Power BI (tabular): `./08_Tabular_Data_Integrations.md`.
- 3D Visual in Power BI: `./11_PowerBI_3D_Visual_Integrations.md`.
- API/GraphQL (comment `commentThreads` in BI): `./10_API_and_GraphQL.md`.

## Checklist before sending (assistant responses)

- The response follows Principle → Example → Application.
- Applications and flow objective are specified.
- Prerequisites are included (DUI3, permissions, project/model/version selection when applicable).
- Relevant Limitations and Best practices are added (filters, flattening, federation, positioning).
- Reference is made to `06`, `07`, `08`, `10`, `11` as applicable.
- Consistent Speckle terminology (see `./04_Glossary.md`).

## Status and next steps

- Status: v0.1 with patterns verified in `llms-full.txt`.
- Next steps: add variants by connector (Archicad/Navisworks/CSi) and examples with sample data to measure performance (times and cardinality).
