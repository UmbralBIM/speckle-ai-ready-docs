
# Speckle Key Elements

## Navigation card
- Glossary: `./04_Glossary.md`
- Master Index: `./02_Master_Index.md`
- Related: `06_General_Connectors.md`

Summary: This document presents the fundamental concepts of Speckle (streams, branches, commits, objects, viewer and models) and their relationships. It serves as a conceptual foundation for the assistant to use precise and consistent terminology in its responses.

## Principle

- Stream: logical data container that groups branches, commits and permissions.  
- Branch: work line within a stream to organize evolution and releases.  
- Commit: data snapshot/versioning associated with a branch; reference to one or several objects.  
- Object: Speckle data unit (geometric or not) serialized and addressable.  
- Viewer: viewer to explore models/objects associated with streams/branches/commits.  
- Model: logical/visual representation of a set of objects (e.g., a model in the viewer within a project).

Basic relationship:

- A stream contains multiple branches.  
- Each branch contains a sequence of commits.  
- Each commit references one or several objects.  
- A project can aggregate models that reference commits/branches of a stream and are visualized in the viewer.

## Example

### Minimal conceptual structure

```json
{
  "streamId": "YOUR_STREAM_ID",
  "branchName": "main",
  "commit": {
    "id": "YOUR_COMMIT_ID",
    "message": "Initial commit",
    "objectIds": ["YOUR_OBJECT_ID_1", "YOUR_OBJECT_ID_2"]
  },
  "model": {
    "name": "YOUR_MODEL_NAME",
    "source": {
      "type": "commit",
      "id": "YOUR_COMMIT_ID"
    }
  }
}
```

Notes:
- Identifiers and names are placeholders; do not expose real values.  
- The model can reference a commit (or branch) that groups objects.

## Guide for the assistant (use in responses)

- Use official terms in English with backticks: `stream`, `branch`, `commit`, `object`, `model`, `project`, `viewer`.
- In the first mention, link to `./04_Glossary.md`.
- Do not confuse `commit` with "version" or `viewer` with "visualizer".
- When describing relationships, use the canonical graph: `stream` → `branch` → `commit` → `object`; and `project` → `model` → (reference `commit`/`branch`).
- Use placeholders in examples (`YOUR_STREAM_ID`, `YOUR_COMMIT_ID`, `YOUR_MODEL_ID`).

## Application

Recommended steps to structure data in a Speckle workflow:

1) Define the stream
- Name the stream according to project/purpose and agree on permissions.  
- Establish naming conventions and visibility.

2) Organize branches
- Create a main branch (e.g., `main`) and thematic branches (discipline/feature).  
- Define merge/advance rules (e.g., who publishes to `main`).

3) Publish commits
- Send small, frequent commits with clear messages.  
- Reuse objects when possible to save storage/transmission.

4) Manage objects
- Keep objects with minimal necessary properties and consistent types.  
- Document expected schemas when using tabular integrations or analysis.

5) Visualize in the viewer
- Create/update models associated with relevant commits/branches.  
- Verify loading, filters and coloring for dataset validation before analysis.

> When including specific code or commands, cite the AI-ready document from which the example comes.

## Limitations / Warnings

- This document is conceptual. Specific SDK/API/GraphQL calls must be cited from the corresponding AI-ready document.  
- If a capability is not verified in the local source, declare it as "Known limitation" and add the closest reference.

## Best practices / Performance

- Consistent nomenclature for streams/branches; avoid spaces and ambiguous names.  
- Granular commits with informative messages; avoid mega-commits.  
- Object reuse to reduce duplication and improve performance.  
- Validate in the viewer before chaining integrations (Excel/Power BI/QGIS).  
- Plan models by use case (validation, coordination, analysis) to minimize load and improve navigability.

## Internal references

- See also: [04_Glossary.md](./04_Glossary.md) · [02_Master_Index.md](./02_Master_Index.md)

## Status and next steps

- Status: v0.1 conceptual.  
- Next steps: add examples cited from AI-ready documents (creation of streams/commits/objects and configuration of models/viewer from SDKs), maintaining placeholders and reproducibility.
