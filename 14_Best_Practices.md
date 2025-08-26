
# Best practices: versioning, permissions, performance and 3D

## Navigation card
- Glossary: `./04_Glossary.md`
- Master Index: `./02_Master_Index.md`
- Related: `12_Common_Patterns.md`, `13_Problem_Solving.md`

Summary: Actionable recommendations for flows with connectors, Power BI and viewer: version control, permissions/roles, federation, data flattening and performance. Guides the assistant to propose practical and consistent responses.

## Principle

- Version by publishing from your tool; each publication creates a traceable version.
- Permissions matter: seeing doesn't equal being able to load/query from connectors.
- In BI, separate tabular connector from 3D Visual and use correct mappings (Version Object ID, Object IDs, Color by, Tooltip).
- In viewer, use appropriate view modes (e.g., Shaded) for correct colors and control visibility with ghost.

## Guide for the assistant (use in responses)

- Versioning: recommend small and frequent publications with clear messages; document URL and version when applicable.
- Permissions: remember that seeing on web doesn't imply loading/querying in connectors/BI; verify role in project.
- Performance (connectors): suggest filters by view/category/selection; disable property extraction if not necessary (when the option exists).
- Performance (BI): flatten properties selectively, control cardinality, relationships by Speckle IDs, and federate by discipline/area.
- 3D Visual: indicate mappings (Version Object ID, Object IDs, Tooltip, Color by) and ghost usage for visibility.

## Examples

### Versioning and traceability

- Publish frequently and label relevant versions with clear messages.
- To compare, federate or audit, document model URL and version (if you use "@", it's fixed).

### Permissions and roles

- If the project selector appears disabled in a connector, request role change from the project owner.
- In Power BI, seeing a model on the web doesn't imply being able to load it; requires project permissions.
- Private projects are compatible with connector and 3D Visual if your account has access.

### Performance in publication and loading

- Use publication filters (view/category/selection) to send only what's necessary.
- In Archicad, disable property extraction if not required to speed up.
- In Navisworks, review positioning and use transformations before publishing if working with offsets.

### Power BI (tabular + 3D Visual)

- Enable "Data Extensions" to see Speckle as a source.
- Keep "Speckle Desktop Service" active to avoid "Unable to connect remote server".
- Map 3D Visual inputs: `Version Object ID` (view), `Object IDs` (interactivity), `Color by` and `Tooltip`.
- For multiple models, use `Speckle.Models.Federate` and consider `excludeData=true` when you only need metadata for 3D.
- Flatten selectively with `Speckle.Objects.Properties` and expand with `Speckle.Utils.ExpandRecord`.

### Viewer

- Change to **Shaded** if you observe incorrect colors.
- Use the **ghost** icon to hide/transparent non-selected.

## Limitations / Warnings

- Canonical views are predefined (not addable by user); some connectors may publish additional views.
- Loading with URL that contains "@" fixes that version until you change the URL.

## Internal references

- Glossary: `./04_Glossary.md`.
- Patterns: `./12_Common_Patterns.md`.
- Power BI: `./08_Tabular_Data_Integrations.md`, `./11_PowerBI_3D_Visual_Integrations.md`.
- Problem solving: `./13_Problem_Solving.md`.

## Checklist before sending (assistant responses)

- The response follows Principle → Example → Application.
- Versioning recommendations are included (small, frequent, clear messages).
- Project permission aspect is validated when applicable.
- Performance best practices are added (filters, selective flattening, federation, relationships by IDs).
- For BI/3D, 3D Visual mappings and key relationships are indicated.
- Consistent Speckle terminology (see `./04_Glossary.md`).

## Status and next steps

- Status: v0.1 with consolidated practices for the assistant.
- Next steps: add tables by connector (publish/load options) and recommendations by discipline.
