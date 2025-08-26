
# Changelog

## v0.2 - 2025-08-18

### Added
- **Complete support for comments/commentThreads** in GraphQL
- Verified comment query example in `10_API_and_GraphQL.md`
- Complete use case for comment dashboard in `15_Use_Cases.md`
- Model + comments integration pattern in `12_Common_Patterns.md`
- `commentThreads` entry in official glossary
- **GraphQL Explorer guide** in `01_General_Instructions.md` and `13_Problem_Solving.md`
- Example 6 of comment integration in `11_PowerBI_3D_Visual_Integrations.md`

### Fixed
- Non-functional discussions example replaced by verified code (`commentThreads`)
- References to non-existent GraphQL fields
- Inconsistent terminology: migrated "discussions" to `commentThreads` in all documents
- Section structure in `15_Use_Cases.md` (Case 6 moved to correct location)

### Updated
- **04_Glossary.md**: v0.1 → v0.2 (new `commentThreads` entry)
- **10_API_and_GraphQL.md**: v0.1 → v0.2 (verified example + Explorer guide)
- **11_PowerBI_3D_Visual_Integrations.md**: v0.1 → v0.2 (commentThreads examples)
- **12_Common_Patterns.md**: v0.1 → v0.2 (Pattern 7 + references)
- **15_Use_Cases.md**: v0.1 → v0.2 (complete Case 6)
- **02_Master_Index.md**: v0.1 → v0.2 (updated versions)
- **01_General_Instructions.md**: v0.2 (terminology + Explorer guide)
- **13_Problem_Solving.md**: v0.1 (Explorer guide added)

### Source
- Direct verification in project 945eae7438 via GraphQL Explorer
- Tested and functional Power Query code
- [GraphQL Explorer](https://app.speckle.systems/explorer) for live validation

### Impact
- Knowledge base updated with verified comment functionality
- Unified terminology: `commentThreads` as official standard
- Guide for live validation when there are schema/permission doubts
- Functional Power Query examples for collaboration dashboards
