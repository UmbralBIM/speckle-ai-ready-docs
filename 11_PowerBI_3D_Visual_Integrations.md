
# Power BI Integrations — 3D Visual and connector

## Navigation card
- Glossary: `./04_Glossary.md`
- Master Index: `./02_Master_Index.md`
- Related: `08_Tabular_Data_Integrations.md`, `10_API_and_GraphQL.md`

Summary: Complete technical guide for dual Speckle + Power BI integration: tabular connector for data analysis and 3D Visual for interactive visualization. Covers Data Extensions, Desktop Service, field configuration (Version Object ID, Object IDs, Color by, Tooltip), table relationships, model federation, dynamic coloring by properties, comment example (`commentThreads`), ghost mode for visibility, and troubleshooting common problems.

**Technical keywords**: Power BI 3D Visual, Data Extensions, Desktop Service, Version Object ID, Object IDs, Color by field, Tooltip field, model federation, dynamic coloring, ghost mode, table relationships, Speckle connector Power BI, interactive 3D visualization, property-based coloring, commentThreads integration, model versioning, 3D analytics, BIM visualization, performance optimization, visual troubleshooting.

## Principle

- The connection with Power BI has two parts: connector (tables) and 3D Visual (visualization/coloring in 3D).
- Requirements in Power BI Desktop:
  - Enable "Data Extensions" for third-party sources if Speckle doesn't appear as origin.
  - Have "Speckle Desktop Service" running for the connector to work.
- Private project support: connector and 3D Visual can work with private projects if your account has permissions.
- In the new generation connector, the "Model URL" field is no longer used (ignore it if you see it mentioned in previous interfaces).

## Guide for the assistant (use in responses)

- Prerequisites: confirm Data Extensions enabled, Desktop Service active and permissions in the project.
- 3D Visual fields: clearly explain what goes in Version Object ID, Object IDs, Tooltip and Color by.
- Relationships: emphasize Speckle keys for table↔3D synchronization.
- Federation: refer to `08_Tabular_Data_Integrations.md` for combining tables and flattening properties.
- Comments (`commentThreads`) in BI: refer to the example (GraphQL + Power Query) when the user wants to bring comment threads.

Internal references: AI-ready documents.

## Example

### Example 1 — Load model with Power BI connector

1) In Power BI Desktop: "Get Data" → search "Speckle" → "Connect to Speckle" → "Connect".
2) If it doesn't appear, go to File → Options and settings → Options → Security → Data Extensions and enable third-party sources.
3) Make sure "Speckle Desktop Service" is active if "Unable to connect remote server" appears.
4) Private models: supported if you have permissions.
5) If you use a URL with "@", you're loading a fixed version (won't follow the latest when refreshing).

### Example 2 — Import the 3D Visual

1) In the "Visualizations" panel, select "…" → "Import a visual from a file".
2) Go to `Documents/Power BI Desktop/Custom Visuals` and choose `Speckle 3D Visual.pbiviz`.
3) Add the visual to the canvas to link it with tabular data loaded from Speckle.

### Example 3 — Coloring and relationships with the 3D Visual

1) Ensure relationships between tables (Speckle identifiers) to synchronize selections between tables and 3D Visual.
2) Configure visual inputs according to `llms-full.txt`:
   - Drag the "Version Object ID" column to the **Version Object ID** input (required for visualization).
   - Drag the "Object IDs" column to **Object IDs** (required for interactivity).
   - Drag any column to **Tooltip** to show tooltips.
   - Drag any column to **Color by** to color the model.
3) Use the ghost icon in the 3D Visual to control visibility of non-selected (ghosted vs completely hidden).

Tips: control cardinality and property flattening with helper functions (`Speckle.Objects.Properties`, `Speckle.Utils.ExpandRecord`).

### Example 4 — Federate multiple models in Power BI

```powerquery
// After loading several model tables (ModelA, ModelB, ModelC)
let
  Federated = Speckle.Models.Federate({ModelA, ModelB, ModelC}, false)
in
  Federated
```

### Example 5 — Comments (`commentThreads`) via GraphQL in Power Query

```graphql
# threadsQuery
query($projectId: String!) {
  project(id: $projectId) {
    commentThreads {
      items { id rawText createdAt author { name } archived replies { totalCount } }
    }
  }
}
```

```powerquery
let
  baseUrl = "https://app.speckle.systems",
  projectId = "YOUR_PROJECT_ID",
  threadsQuery = "query($projectId: String!) { project(id: $projectId) { commentThreads { items { id rawText createdAt author { name } archived replies { totalCount } } } } }",
  vars = [ projectId = projectId ],
  resp = Speckle.Api.Fetch(baseUrl, threadsQuery, vars),
  items = resp[project][commentThreads][items]
in
  items
```

Usage: replace `YOUR_PROJECT_ID`, execute in Power Query and expand `items` to columns (`id`, `rawText`, `createdAt`, `author.name`, `archived`, `replies.totalCount`).

### Example 6 — Integrate project comments

```powerquery
// Independent query for comments
let
    baseUrl = "https://app.speckle.systems",
    projectId = "YOUR_PROJECT_ID",
    
    threadsQuery = "query($projectId: String!) { project(id: $projectId) { commentThreads { items { id rawText createdAt author { name } archived replies { totalCount } } } } }",
    
    vars = [ projectId = projectId ],
    response = Speckle.Api.Fetch(baseUrl, threadsQuery, vars),
    items = response[project][commentThreads][items]
in
    items
```

Application: Combine model tables + comments using Project ID as common key. Useful for activity and collaboration dashboards.

## Application

### Case A — 3D report with coloring by properties

1) Load the model with the connector and add the 3D Visual.
2) Flatten key properties with `Speckle.Objects.Properties` and expand them with `Speckle.Utils.ExpandRecord`.
3) Create relationships and measures to color the 3D Visual according to attributes (level, category, material).

### Case B — Multi-project federated view with 3D

1) Use `Speckle.Models.Federate` to combine several model tables.
2) Differentiate origin/project with auxiliary columns.
3) Add the 3D Visual for navigation and synchronized highlighting between tables and geometry.

## Checklist before sending (assistant responses)

- The response follows Principle → Example → Application.
- Prerequisites are indicated (Data Extensions, Desktop Service, permissions).
- 3D Visual fields and their use are described.
- Relationship structure by Speckle IDs is recommended.
- Reference is made to `08` (helpers and federation) and `10` (comment example `commentThreads`) when applicable.
- Consistent Speckle terminology (see `./04_Glossary.md`).

## Limitations / Warnings

- Enable "Data Extensions" to see Speckle as a source.
- Requires "Speckle Desktop Service" active for the connector.
- Loading a version with "@" fixes that version (doesn't follow the latest when refreshing).
- The 3D Visual is imported from `Documents/Power BI Desktop/Custom Visuals` (`Speckle 3D Visual.pbiviz`). Check the **Visualizations** panel for **Import a visual from a file**.
- Branding: in Business plan you can hide the logo from the top bar; in Free plan it's not possible.
- Performance: flattening too many properties without filters will degrade performance.

## Best practices / Performance

- Filter early and select only necessary columns.
- Control cardinality with `Speckle.Objects.Properties` and expand selectively with `Speckle.Utils.ExpandRecord`.
- Use robust relationships (Speckle IDs) for table↔3D synchronization.
- Consider federating by discipline/area to maintain manageable datasets.

## Internal references

- Glossary: `./04_Glossary.md` (project, model, version, object, viewer, token).
- API/GraphQL: `./10_API_and_GraphQL.md` (reuses the comment example `commentThreads`).
- Tabular: `./08_Tabular_Data_Integrations.md`.

## Status and next steps

- Status: v0.1 with connector steps, 3D Visual, federation and comments (`commentThreads`).
- Next steps: add advanced coloring recipes and examples of typical relationships (facts/dimensions) with real data.
