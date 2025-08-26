
# Problem solving: connectors, Power BI and viewer

## Navigation card
- Glossary: `./04_Glossary.md`
- Master Index: `./02_Master_Index.md`
- Related: `06_General_Connectors.md`, `14_Best_Practices.md`

Summary: Guided diagnosis for common failures in connectors (empty UI, permissions, loading by URL), Power BI (desktop service, fixed versions with "@", private projects) and viewer (colors/view). Includes practical steps with sources in AI-ready documents.

## Principle

- Prioritize frequent causes confirmed in local documentation: floating UI not docked, log review, permissions/roles, dependencies (Desktop Service), and URLs with fixed version.
- Maintain traceability (project/model/version URL) and record it along with the report.

## Guide for the assistant (use in responses)

- Clarify the context: connector/tool involved, operation (Publish/Load/Get Data/Viewer) and problem scope.
- Verify common prerequisites: session started in DUI3, adequate permissions in the project, Speckle Desktop Service active (if applicable), Data Extensions enabled (Power BI).
- Differentiate between fixed version problems (URLs with "@") vs latest version.
- Always ask for minimal evidence: model URL, version (if applicable), host app and connector version, logs (if applicable).
- Refer to specific documents: `./06_General_Connectors.md`, `./07_Modeling_Connectors.md`, `./08_Tabular_Data_Integrations.md`, `./11_PowerBI_3D_Visual_Integrations.md`.
- For API schema/permission doubts: ask to validate the query in the server's GraphQL Explorer ([GraphQL Explorer](https://app.speckle.systems/explorer)). Requires login and uses real data.

## Diagnostic examples

### Connectors — Empty or unresponsive UI

Symptoms: blank connector window, unresponsive inputs, or app crash.

Steps:
1) If the window is floating, dock the panel to the host; fixes blank UIs.
2) Check logs in `AppData\Roaming\Speckle\Logs`.
3) If it persists, open a ticket in the forum with: host app version, connector version and attach the log.

### Connectors — Load by URL

Steps:
1) Use the "Add model by URL" option in the selection dialog (when available).
2) If the URL contains "@", loads a specific version; to follow the latest, use a URL without "@".
3) If you can't select the project, validate your role/permissions with the owner.

### Power BI — Speckle doesn't appear in Get Data

Steps:
1) Enable third-party sources in File → Options and settings → Options → Security → Data Extensions.
2) Restart Power BI Desktop.

### Power BI — "Unable to connect remote server"

Steps:
1) Verify that "Speckle Desktop Service" is active and running.
2) Retry the connection.

### Power BI — Private models

Confirmation: the connector and 3D Visual support private projects if your account has permissions.

### Viewer — Incorrect colors

Steps:
1) In the viewer, open "View Modes" and change to "Shaded".
2) If it persists, report in the help forum.

## Limitations / Warnings

- Functions and options depend on connector/host app version.
- Canonical views are predefined; it's not possible to add more by the user.
- Loading a version with "@" fixes that version and won't follow the latest automatically.

## Best practices / Performance

- Always attach: host app version, connector version and relevant logs.
- Document the URL and version of the involved model to repeat the problem.
- In publication, disable properties if not necessary to speed up (when the option exists).

## Checklist before sending (assistant responses)

- The response follows a diagnostic flow: context → prerequisites → steps → verification.
- Minimal evidence is requested (URL, versions, logs when applicable).
- URLs with "@" and project permissions are considered.
- Reference is made to related documents (`06`, `07`, `08`, `11`) if applicable.
- Real tokens/IDs or absolute paths are not exposed.
- Consistent Speckle terminology (see `./04_Glossary.md`).

## Internal references

- Glossary: `./04_Glossary.md`.
- API/GraphQL (to investigate comments/activity): `./10_API_and_GraphQL.md`.
- Power BI (tabular and 3D Visual): `./08_Tabular_Data_Integrations.md`, `./11_PowerBI_3D_Visual_Integrations.md`.

## Status and next steps

- Status: v0.1 with confirmed cases in `llms-full.txt`.
- Next steps: incorporate error matrix by connector (Archicad/Revit/Navisworks/Grasshopper) and direct links to FAQ sections on the web.
