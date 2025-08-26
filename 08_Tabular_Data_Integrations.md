
# Tabular data integrations: Excel and Power BI

## Navigation card
- Glossary: `./04_Glossary.md`
- Master Index: `./02_Master_Index.md`
- Related: `11_PowerBI_3D_Visual_Integrations.md`, `10_API_and_GraphQL.md`

Summary: Practical guide to load Speckle model data in tabular format and visualize it in Power BI (connector + 3D Viewer Visual). Includes Power Query helper functions to flatten properties, federate models and derive quantities. Sources: AI-ready documents.

## Principle

- The Power BI connector loads Speckle models as tables. The 3D visual allows coloring and viewing the model within the report.
- Loading multiple models and federating them in Power BI is supported.
- Power Query helper functions in the `Speckle` namespace help handle nested properties, composite structures and material quantities.
- Key requirements and considerations:
  - Enable third-party sources in Power BI Desktop (Options → Data Extensions).
  - The connector requires Speckle Desktop Service to be running.
  - Private models are compatible if you have adequate permissions.
  - If you load a specific version (URL with "@"), subsequent updates are not reflected until changing the version.

## Guide for the assistant (use in responses)

- Prerequisites: confirm Data Extensions enabled, Speckle Desktop Service active and adequate permissions in the project.
- Versioning: if the URL contains "@", explain that it fixes a version and how to change to follow the latest.
- When to use each helper:
  - `Speckle.Objects.Properties` + `Speckle.Utils.ExpandRecord`: selective flattening of properties in columns.
  - `Speckle.Models.Federate`: combine tables from multiple models in a dataset.
  - `Speckle.Objects.CompositeStructure` (Revit/Archicad): expose layer structure.
  - `Speckle.Objects.MaterialQuantities`: derive quantities per element.
- Relationships: recommend using Speckle identifiers as consistent keys between tables and with the 3D Visual.

Internal references (AI-ready documents):
- "Get Data" steps of the Power BI connector and frequently asked questions.
- Import of the 3D Viewer Visual (`Speckle 3D Visual.pbiviz`).
- Power Query helper functions:
  - `Speckle.Objects.Properties(record, filterKeys?)`
  - `Speckle.Objects.CompositeStructure(record, outputAsList?)` (currently for Revit/Archicad)
  - `Speckle.Objects.MaterialQuantities(record, outputAsList?)`
  - `Speckle.Models.Federate(tables, excludeData?)`
  - `Speckle.Utils.ExpandRecord(table, columnName, FieldNames?, UseCombinedNames?)`

## Example

### Example 1 — Load a Speckle model in Power BI

1) In Power BI Desktop, select "Get Data" → search "Speckle" → "Connect to Speckle" → "Connect".
2) If you don't see Speckle in the sources, enable third-party sources in "File → Options and settings → Options → Security → Data Extensions".
3) For private models, make sure you have permissions in the project.
4) If the model URL contains "@", you're fixing a specific version; use another URL to follow the latest version.
5) If "Unable to connect remote server" appears, verify that "Speckle Desktop Service" is active.

### Example 2 — Flatten properties with helper functions (Power Query)

Objective: expose nested properties in tabular columns in a controlled manner.

```powerquery
// Input table: ModelTable (with a column "object" that is Speckle record)
let
  Source = ModelTable,
  // Extract relevant properties from the object record
  WithProps = Table.AddColumn(Source, "props", each Speckle.Objects.Properties([object], {"name", "category", "level", "type"})),
  // Expand the props record to columns
  Expanded = Speckle.Utils.ExpandRecord(WithProps, "props")
in
  Expanded
```

### Example 3 — Federate multiple loaded models (Power Query)

Objective: combine several already loaded model tables in a single federated dataset.

```powerquery
// Suppose you have loaded tables: ModelA, ModelB, ModelC
let
  Federated = Speckle.Models.Federate({ModelA, ModelB, ModelC}, false)
in
  Federated
```

Tips:
- You can federate models from different Speckle projects.
- Use source columns (project/model) to differentiate entities.

### Example 4 — Composite structure and material quantities (Power Query)

Objective: derive structure and quantities for BI analysis.

```powerquery
// Input table: Elements (with "object" column of record type)
let
  Source = Elements,
  // Composite structure (Revit/Archicad)
  WithComposite = Table.AddColumn(Source, "composite", each Speckle.Objects.CompositeStructure([object], true)),
  // Material quantities per element
  WithMats = Table.AddColumn(WithComposite, "materials", each Speckle.Objects.MaterialQuantities([object], true))
in
  WithMats
```

Known limitation: `CompositeStructure` is currently supported for Revit and Archicad.

### Example 5 — Import and use the 3D Viewer Visual

1) Visualizations → "…" → "Import a visual from a file".
2) Navigate to `Documents/Power BI Desktop/Custom Visuals` and select `Speckle 3D Visual.pbiviz`.
3) Use key fields (identifiers) to link colors/selections between the table and the 3D visual.

### Decision map — What to use according to need?

- Flatten properties: `Speckle.Objects.Properties` + `Speckle.Utils.ExpandRecord`.
- Federate models: `Speckle.Models.Federate`.
- Layer structure: `Speckle.Objects.CompositeStructure` (Revit/Archicad).
- Material quantities: `Speckle.Objects.MaterialQuantities`.

## Application

### Case A — Multidisciplinary dashboard with federation and 3D

1) Load several models (different disciplines or projects).
2) Federation in Power Query: `Speckle.Models.Federate({TablaDiscA, TablaDiscB, ...})`.
3) Flatten key properties with `Speckle.Objects.Properties` and expand them with `Speckle.Utils.ExpandRecord`.
4) Relate master/dimension tables by consistent IDs.
5) Incorporate the 3D Viewer Visual and synchronize highlights/colors with DAX measures or calculated columns.

### Case B — Material quantification for executive reports

1) Load the model; add `materials` column with `Speckle.Objects.MaterialQuantities`.
2) Expand materials to rows/columns, add sum measures by material/type.
3) Create slicers by level, category and phase.
4) Export snapshots or use scheduled refresh according to your policies.

## Limitations / Warnings

- `Speckle.Objects.CompositeStructure` currently supported only for Revit and Archicad.
- To see Speckle as a source, enable "Data Extensions" (third-party) in Power BI.
- Requires "Speckle Desktop Service" running for the Power BI connector.
- Loading a version with "@" fixes that version and won't follow the latest.
- Permissions: seeing a model on the web doesn't imply being able to load it in the connector; requires adequate role in the project.
- Excel: pending to incorporate specific guide when evidence exists in AI-ready documents.

## Best practices / Performance

- Filter early and select only necessary columns.
- Use `Speckle.Objects.Properties` to control cardinality and uniform keys.
- Avoid expanding giant structures without prior filters; work by batches/entities.
- Define clear relationships and stable keys between tables (Speckle identifiers).
- Consider partitioning by version or discipline for faster refresh.
- Document source URL and loaded version for traceability.

## Internal references

- Glossary: `./04_Glossary.md` (terms: project, model, version, object, viewer, token).
- 3D Visual: `./11_PowerBI_3D_Visual_Integrations.md`.
- API/GraphQL (advanced queries): `./10_API_and_GraphQL.md`.
- Patterns: `./12_Common_Patterns.md`.

## Checklist before sending (assistant responses)

- The response follows Principle → Example → Application.
- `powerquery` blocks use placeholders; do not expose real data.
- Include Limitations (e.g., `CompositeStructure` only Revit/Archicad).
- Recommend best practices for filtering, cardinality and relationships.
- Refer to `11` (3D Visual), `10` (API/GraphQL) and `12` (Patterns) as applicable.
- Consistent Speckle terminology (see `./04_Glossary.md`).

## Status and next steps

- Status: version 0.1 focused on Power BI with helper functions and 3D visual.
- Next steps: incorporate specific Excel guide when its local documentation is indexed in `Data`; add complete examples with sample data and model-view relationships.
