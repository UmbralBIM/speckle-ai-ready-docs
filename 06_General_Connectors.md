
# General Connectors

## Navigation card
- Glossary: `./04_Glossary.md`
- Master Index: `./02_Master_Index.md`
- Related: `07_Modeling_Connectors.md`

Summary: General overview of Speckle connectors that link AEC applications with the platform. Directed to the assistant to guide responses about objectives, typical flow, compatibility and operation recommendations. Always reference specific AI-ready documents for details by connector.

## Principle

- Connectors allow sending/receiving data between AEC applications and Speckle, managing streams, branches, commits and objects.  
- Installation, compatibility and capabilities vary by application; for concrete statements, cite specific AI-ready documents.  
- The base didactic sequence is maintained: Principle → Example → Application, with normalized terminology (stream, branch, commit, object, discussion, token, viewer).

## Guide for the assistant (use in responses)

- Identify the application (Revit, Rhino, Grasshopper, Blender, QGIS, Navisworks) and the user's need (Publish/Load, filters, properties, compatibility).
- Indicate supported operations in general terms and refer to `./07_Modeling_Connectors.md` for details by connector.
- Clarify common prerequisites: session started in DUI3, project/model selection, adequate permissions.
- If applicable, suggest best practices: small commits, clear nomenclature, viewer verification.
- Avoid claiming compatibilities/versions without evidence; mark as "Known limitation" and refer to the specific document.

## Example

### Compatibility matrix (consult specific documents)

| Application | App version | Supported operations | Notes | Detailed document |
|---|---|---|---|---|
| Revit | See document | Send/Receive | Verified limitations | `07_Modeling_Connectors.md` |
| Rhino | See document | Send/Receive | Key formats/attributes | `07_Modeling_Connectors.md` |
| Grasshopper | See document | Send/Receive | Typing/parametrics | `07_Modeling_Connectors.md` |
| Blender | See document | Send/Receive | Types and materials | `07_Modeling_Connectors.md` |
| QGIS | See document | Send/Receive | Fields/CRS | `07_Modeling_Connectors.md` |
| Navisworks | See document | Send | Review/coord. | `07_Modeling_Connectors.md` |

> Consult the specific document for verified detailed information.

### Typical usage flow (generic)

1) Select the work stream (or create one).  
2) Choose target branch (e.g., `main` or thematic).  
3) Prepare selection of elements/objects in the application.  
4) Send commit with clear message and placeholders where applicable.  
5) Validate in the viewer and record findings.

## Application

Recommended steps to adopt connectors in a team:

1) Inventory and compatibility
- Identify applications and versions in use.  
- Complete the compatibility matrix consulting specific documents.

2) Installation/update procedures
- Document the procedure by application citing AI-ready documents.  
- Maintain a record of tested versions and responsible parties.

3) Operation and quality control
- Define object selection criteria and minimum metadata.  
- Standardize commit messages and branch nomenclature.

4) Viewer verification and feedback
- Validate commits received visually and semantically.  
- Record incidents/limitations and their reference to AI-ready documents.

> For specific code examples or parameters, consult the corresponding connector's AI-ready document.

## Limitations / Warnings

- Do not list capabilities/versions without local evidence.  
- Some operations depend on types/attributes specific to each application; document only with verifiable references.  
- Do not expose real tokens/IDs in examples.

## Best practices / Performance

- Small and frequent commits with useful messages.  
- Consistent use of streams/branches to separate disciplines/stages.  
- Reduce unnecessary geometry/attributes before sending.  
- Verify in viewer before chaining tabular integrations or analysis.  
- Maintain a living table of compatibilities and limitations.

## Checklist before sending (assistant responses)

- The response follows Principle → Example → Application.
- The application and operation (Publish/Load) are specified when applicable.
- Relevant Limitations and Best practices are included.
- Reference is made to `./07_Modeling_Connectors.md` for details by connector.
- Versions/capabilities are not listed without reference.
- Consistent Speckle terminology (see `./04_Glossary.md`).

## Internal references

- See also: [04_Glossary.md](./04_Glossary.md) · [02_Master_Index.md](./02_Master_Index.md) · [07_Modeling_Connectors.md](./07_Modeling_Connectors.md)

## Status and next steps

- Status: v0.1, general base created.  
- Next steps: complete the matrix and detail procedures by application citing AI-ready documents.
