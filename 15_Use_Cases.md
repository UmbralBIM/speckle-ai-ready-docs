
# AEC use cases with Speckle

## Navigation card
- Glossary: `./04_Glossary.md`
- Master Index: `./02_Master_Index.md`
- Related: `12_Common_Patterns.md`, `11_PowerBI_3D_Visual_Integrations.md`

Summary: Collection of practical use cases for 3D coordination, Power BI reporting, publication QA and flows with Navisworks/Grasshopper. Guides the assistant to respond with clear objectives, actionable steps and reproducible fragments (Power Query) when applicable.

## Principle

- Publishing (Publish) and Loading (Load) generate and consume traceable versions of the model.
- Federation allows viewing multiple models (architecture/structure/MEP) in a single view without merging them.
- In Power BI, the connector ingests tabular data and the 3D Visual colors/interacts with the model if its inputs are mapped correctly.

## Guide for the assistant (use in responses)

- Identify the case and tools involved (web, Power BI, Navisworks, Grasshopper, Revit) and the objective (coordination, KPIs, QA, sets/properties, parametric).
- Clarify prerequisites: DUI3 active (session started), permissions in the project, project/model/version selection, and Power BI requirements (Data Extensions, Desktop Service) when applicable.
- For BI/3D: indicate 3D Visual mappings (Version Object ID, Object IDs, Tooltip, Color by) and relationships by Speckle IDs.
- For multi-tool flows: refer to `./12_Common_Patterns.md` and specific documents (`./08_Tabular_Data_Integrations.md`, `./11_PowerBI_3D_Visual_Integrations.md`).
- Include performance notes: federation by discipline/area, selective property flattening, publication filters.

## Examples

### Case 1 — Multidisciplinary coordination on the web (federation)

Objective: review in a federated view models of architecture, structure and MEP without merging files.

Steps:
1) Publish models by discipline from the corresponding connectors (DUI3 → Publish).
2) On the web, create a federated view by aggregating models (federation consists of loading multiple models in the same view).
3) Use filter/isolate/color tools to analyze by properties.

Notes and tips:
- Canonical views are predefined; some connectors may publish additional views.
- Dividing very detailed models and federating according to need improves viewer performance.

### Case 2 — Model KPI in Power BI (tabular + 3D)

Objective: build a dashboard with metrics (by level/category/material) and a synchronized 3D Visual.

Steps (Power BI Desktop):
1) "Get Data" → search "Speckle" → "Connect to Speckle" → "Connect". Enable Data Extensions if it doesn't appear.
2) Import the 3D Visual: Visualizations → "…" → "Import a visual from a file" → `Speckle 3D Visual.pbiviz`.
3) Map 3D Visual inputs:
   - `Version Object ID` (required for visualization)
   - `Object IDs` (required for interactivity)
   - `Tooltip` (optional for tooltips)
   - `Color by` (coloring by attribute)
4) Flatten key properties with helpers and expand them to columns:

```powerquery
let
  Source = ModelTable,
  WithProps = Table.AddColumn(Source, "props", each Speckle.Objects.Properties([object], {"name", "category", "level", "type"})),
  Expanded = Speckle.Utils.ExpandRecord(WithProps, "props")
in
  Expanded
```

5) If you load multiple models, federation in Power Query:

```powerquery
let
  Federated = Speckle.Models.Federate({ModelA, ModelB, ModelC}, false)
in
  Federated
```

Notes and tips:
- For only metadata necessary for 3D, use `excludeData = true` when federating.
- If the URL contains "@", you load a fixed version (doesn't update to the latest until changing the URL).
- Private projects are supported by connector and 3D Visual if you have permissions.

### Case 3 — Publication QA with connectors (UI/positioning)

Objective: publish consistent versions and correct common issues in UI or positioning.

Steps:
1) Follow the DUI3 publication flow (selection, filters by view/category when applicable) and review the publication report.
2) If the connector window remains blank, dock the panel and check logs in `AppData\Roaming\Speckle\Logs`.
3) If working with Revit and there's offset when loading, ask for re-publication with the correct reference point.

### Case 4 — Navisworks: sets and properties to Speckle

Objective: use selection/search sets to filter what to publish and expose properties in Speckle.

Steps:
1) Publish visible objects (NWC, DWG, IFC, RVT). Attached properties are included and can be seen on the web.
2) Use selection/search sets as filters (not preserved as named sets, but serve to decide what to send).
3) Positioning: Navisworks coordinate system is respected; if there's offset, apply transformation when publishing.

### Case 5 — Grasshopper: parametric publication/loading

Objective: iterate geometry/attributes from GH and share states with the team.

Steps:
1) Use Speckle nodes (connector comes packaged with Rhino). Open examples in: File → Special Folders → Components Folder → Speckle Sample Files.
2) To publish: compose collection and model ("Create Collection" + "Speckle Model URL" nodes) and press Publish.
3) To load: use "Load" node, connect the "Model Link" and process the resulting collection.
4) Access to extended fields: use "Deconstruct" node.

### Case 6 — Comments and collaboration dashboard

Objective: monitor comment activity, collaboration trends and project communication metrics.

Steps (Power BI Desktop):
1) Load comments with independent GraphQL query (see complete code in `10_API_and_GraphQL.md`).
2) Process metrics: ResponseCount, FirstCommentDate, LastCommentDate, ActivityLevel, Status.
3) Create relationships with model tables using Project ID.
4) Visualize timeline, activity by author and Open/Closed distribution.

```powerquery
// Verified complete code
let
    baseUrl = "https://app.speckle.systems",
    projectId = "YOUR_PROJECT_ID",

    threadsQuery = "query($projectId: String!) { project(id: $projectId) { commentThreads { totalCount items { id rawText createdAt updatedAt archived author { name id } replies { totalCount items { id rawText createdAt author { name } } } viewerState } } } }",

    vars = [ projectId = projectId ],
    response = Speckle.Api.Fetch(baseUrl, threadsQuery, vars),

    // Normalize thread list
    threads = response[project][commentThreads][items],
    threadsTable = Table.FromList(threads, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    expanded = Table.ExpandRecordColumn(threadsTable, "Column1", {"id", "rawText", "createdAt", "updatedAt", "archived", "author", "replies"}, {"ThreadId", "RawText", "CreatedAt", "UpdatedAt", "Archived", "Author", "Replies"}),
    authorExpanded = Table.ExpandRecordColumn(expanded, "Author", {"name"}, {"AuthorName"}),
    repliesCount = Table.AddColumn(authorExpanded, "ResponseCount", each [Replies][totalCount], Int64.Type),
    status = Table.AddColumn(repliesCount, "Status", each if [Archived] then "Closed" else "Open"),

    // Activity metrics based on dates
    addCreatedDate = Table.TransformColumns(status, {{"CreatedAt", each DateTime.From(_), type datetime}}),
    now = DateTime.LocalNow(),
    addDaysDiff = Table.AddColumn(addCreatedDate, "DaysSinceCreated", each Duration.Days(now - [CreatedAt]), Int64.Type),
    activity = Table.AddColumn(addDaysDiff, "ActivityLevel", each if [DaysSinceCreated] <= 7 then "Recent" else if [DaysSinceCreated] <= 30 then "Active" else "Old"),

    // Final column selection
    selected = Table.SelectColumns(activity, {"ThreadId", "RawText", "CreatedAt", "UpdatedAt", "Archived", "Status", "ResponseCount", "ActivityLevel", "AuthorName"})
in
    selected
```

Resulting metrics:
- **ThreadId**: Unique comment identifier
- **Title**: Title generated from RawText (truncated)
- **Status**: "Open"/"Closed" based on `archived`
- **ResponseCount**: Number of responses per thread
- **ActivityLevel**: "Recent"/"Active"/"Old" based on days
- **AuthorName**: Main comment author

Suggested visualizations:
- Comment timeline by date
- Activity by author (bar chart)
- Open vs Closed distribution (donut)
- Filterable comment list (table)
- KPIs: total comments, active comments, recent activity

## Application

- Combine the cases: use Navisworks to filter + Revit to re-publish well referenced + federation on web + dashboard in Power BI with 3D Visual.
- Use Power Query helpers to flatten only what's necessary and maintain agile refresh.

## Limitations / Warnings

- Predefined canonical views (not addable by user); some connectors publish additional views.
- Seeing on the web doesn't equal loading in connectors/BI; requires adequate permissions.
- URL with "@" fixes version; doesn't follow the latest until changing the URL.
- Power BI connector requires "Speckle Desktop Service" active.

## Best practices / Performance

- Publish by parts (view/category/selection) to reduce size and times.
- Strategic federation: break heavy models by discipline/area.
- In Power Query, filter and expand properties selectively.
- In the 3D Visual, control visibility with the "ghost" icon and use Shaded if you detect incorrect colors.

## Internal references

- Glossary: `./04_Glossary.md`.
- Patterns: `./12_Common_Patterns.md`.
- Power BI: `./08_Tabular_Data_Integrations.md`, `./11_PowerBI_3D_Visual_Integrations.md`.
- API/GraphQL: `./10_API_and_GraphQL.md`.

## Checklist before sending (assistant responses)

- The case objective and tools involved are stated.
- Actionable steps are listed and fragments are included when applicable (with placeholders).
- For BI/3D, mappings and relationships by Speckle IDs are indicated.
- Limitations and Best practices are added (federation, flattening, filters, positioning) if applicable.
- Reference is made to `12`, `08`, `11`, `10` according to the case.
- Consistent Speckle terminology (see `./04_Glossary.md`).

## Status and next steps

- Status: v0.1 oriented to the assistant with actionable cases.
- Next steps: add sample datasets and time/refresh metrics for benchmark; expand to QGIS/Blender when evidence exists in AI-ready documents.
