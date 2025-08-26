
# Speckle Glossary

## Navigation card
- Instructions: `./01_General_Instructions.md`
- Master Index: `./02_Master_Index.md`
- Related: `05_Speckle_Key_Elements.md`

Summary: This glossary normalizes key Speckle terminology so that the assistant uses correct and consistent vocabulary when responding. It includes concepts such as stream, branch, commit, object, commentThreads, token and viewer.

## Principle

- Terminological normalization improves clarity, traceability and knowledge reuse.  
- Terms defined here must be used consistently in all AI-ready documents.  
- Avoid ambiguous synonyms; when they exist, prefer the officially indicated term.

## Example

### Base Speckle terms

| Term | Brief definition | Synonyms/Don't use | Notes |
|---|---|---|---|
| stream | Logical data container in Speckle. Groups branches, commits and objects associated with a purpose or project. | channel, flow (avoid in technical texts) | Use in lowercase.
| branch | Work line within a stream to organize commits. | branch (acceptable only in parentheses) | Maintain consistency when referencing it.
| commit | Data snapshot/versioning within a branch. | version (avoid) | Identified by its id; do not expose real ids in docs.
| object | Data unit (geometric or not) serialized in Speckle. | element, entity (generic) | Refer as object when it's Speckle-native.
| discussion | Conversation thread associated with a project/model. | thread | Use it for structured comments.
| commentThreads | Comment threads associated with a project in Speckle. They contain text, author, responses and status (archived). | threads, discussions (avoid) | Use to refer to comment system.
| token | Authentication/authoring credential for API/CLI/SDK. | API key (avoid) | Never publish real tokens.
| viewer | Speckle 3D/2D viewer to explore models/objects. | visualizer (avoid) | Refer as viewer.
| model | Logical representation of a set of objects (e.g., view/model in the viewer). | model (generic) | Clarify if it's a Speckle entity or general concept.
| project | Superior container (e.g., groups comments (`commentThreads`), models and permissions). | project (generic) | Avoid confusion with local repos.

### Text reference style

- Correct: "Send a commit to the `main` branch of the `demo-infra` stream."  
- Avoid: "Upload a version to the main branch of the channel."

## Guide for the assistant (use in responses)

- Keep official Speckle terms in English and in backticks in the text: `stream`, `branch`, `commit`, `object`, `model`, `project`, `viewer`, `commentThreads`, `token`.
- Do not translate entity names or API fields. Only translate the narrative around them.
- Avoid synonyms: do not use "version" for `commit` or "visualizer" for `viewer`.
- In the first mention of each term in a response, link to this glossary.
- Never expose real IDs or tokens; use placeholders (`YOUR_PROJECT_ID`, `YOUR_TOKEN`).

## Application

- In assistant responses: use exclusively the terms from the table. If you need another term, add it here before using it.  
- In code and examples: use safe placeholders for ids and tokens (`YOUR_COMMIT_ID`, `YOUR_TOKEN`).  
- In cross-references: link this glossary when you introduce these concepts for the first time.

## Limitations / Warnings

- This glossary focuses on terminology; it does not describe endpoints or API contracts.  
- If a definition requires implementation nuances, cite the specific AI-ready document where detailed information is found.

## Best practices

- Keep entries short, precise and stable.  
- Update the table when new concepts are incorporated or official names change.  
- Reuse these terms in titles, tables and legends for uniformity.

## Internal references

- See also: [01_General_Instructions.md](./01_General_Instructions.md) · [02_Master_Index.md](./02_Master_Index.md)

## Status and next steps

- Status: v0.1 initial with base terms.  
- Next steps: expand with specific entries by connector (Revit, Rhino, Grasshopper, QGIS, Blender) citing relevant AI-ready documents.
