# Speckle AI-Ready Documentation

[![Speckle Version](https://img.shields.io/badge/Speckle-2.x-blue.svg)](https://speckle.systems)
[![Documentation Status](https://img.shields.io/badge/Documentation-AI--Ready-green.svg)](https://github.com/UmbralBIM/speckle-ai-ready-docs)
[![Last Updated](https://img.shields.io/badge/Last%20Updated-2025--08--18-orange.svg)](16_Changelog.md)

> **Structured and verified documentation for LLMs and AI assistants specialized in Speckle**

## 🎯 Purpose

This collection of documents provides an **AI-ready** knowledge base for AI assistants that need to solve technical queries about Speckle. The documents are structured following specific pedagogical principles and contain verified information about connectors, API, Power BI, scripting and best practices.

## 📚 Available Documents

### 📖 **Main Documents**
- **[01_General_Instructions.md](01_General_Instructions.md)** - Operational instructions for AI assistants
- **[02_Master_Index.md](02_Master_Index.md)** - Navigable index of all documents
- **[04_Glossary.md](04_Glossary.md)** - Official Speckle terminology
- **[05_Speckle_Key_Elements.md](05_Speckle_Key_Elements.md)** - Fundamental concepts

### 🔌 **Connectors and Tools**
- **[06_General_Connectors.md](06_General_Connectors.md)** - Installation and general compatibilities
- **[07_Modeling_Connectors.md](07_Modeling_Connectors.md)** - Revit, Rhino, Grasshopper, Blender, QGIS
- **[08_Tabular_Data_Integrations.md](08_Tabular_Data_Integrations.md)** - Excel and Power BI
- **[09_Scripting_and_Code_Guide.md](09_Scripting_and_Code_Guide.md)** - `specklepy` and `speckle-sharp`

### 🚀 **API and Development**
- **[10_API_and_GraphQL.md](10_API_and_GraphQL.md)** - Authentication, queries and examples
- **[11_PowerBI_3D_Visual_Integrations.md](11_PowerBI_3D_Visual_Integrations.md)** - Power BI 3D Visual

### 📋 **Patterns and Best Practices**
- **[12_Common_Patterns.md](12_Common_Patterns.md)** - Typical multi-tool workflows
- **[13_Problem_Solving.md](13_Problem_Solving.md)** - Diagnosis and troubleshooting
- **[14_Best_Practices.md](14_Best_Practices.md)** - Versioning, permissions and performance
- **[15_Use_Cases.md](15_Use_Cases.md)** - Specific AEC use cases

### 📝 **Change Control**
- **[16_Changelog.md](16_Changelog.md)** - Version history and updates

## 🎓 AI-Ready Methodology

### **Principle → Example → Application**
Each document follows a consistent didactic sequence:
1. **Principle**: Explains the technical concept or capability
2. **Example**: Provides functional code with placeholders
3. **Application**: Describes specific steps for real use cases

### **Key Features**
- ✅ **Verified information** - Based on real tests and official documentation
- ✅ **Consistent terminology** - Normalized vocabulary according to the glossary
- ✅ **Reproducible examples** - Functional code with safe placeholders
- ✅ **Cross-references** - Internal links between related documents
- ✅ **Navigable structure** - Master index for quick location

## 🛠️ Use Cases

- Resolve technical queries about Speckle implementations
- Guide users in multi-tool workflows
- Implement integrations with Speckle API
- Configure connectors for AEC applications
- Create custom 3D dashboards in Power BI
- Automate workflows with SDKs
- Implementation best practices

## 🚀 Quick Start

### **1. Basic Query**
```markdown
"How to send data from Revit to Speckle?"
→ Consult: `07_Modeling_Connectors.md` (Principle → Example → Application)
```

### **2. Power BI Implementation**
```markdown
"How to configure the 3D Visual in Power BI?"
→ Consult: `11_PowerBI_3D_Visual_Integrations.md` + `08_Tabular_Data_Integrations.md`
```

### **3. Troubleshooting**
```markdown
"Rhino connector doesn't appear"
→ Consult: `13_Problem_Solving.md` → Diagnostic checklist
```

## 📋 Requirements

- **Speckle**: Version 3.x
- **Connectors**: Active DUI3, project permissions
- **Power BI**: Data Extensions enabled, Desktop Service active
- **SDKs**: `specklepy` (Python) or `speckle-sharp` (.NET)

## 🔗 Useful Links

- **Official Speckle**: [speckle.systems](https://speckle.systems)
- **GraphQL Explorer**: [app.speckle.systems/explorer](https://app.speckle.systems/explorer)
- **Community**: [forum.speckle.systems](https://forum.speckle.systems)
- **Documentation**: [docs.speckle.systems](https://docs.speckle.systems)

## 🤝 Contributions

### **Report Issues**
- Use the [issue tracker](https://github.com/UmbralBIM/speckle-ai-ready-docs/issues)
- Include context: tool, version, steps to reproduce
- Attach logs when possible

### **Suggest Improvements**
- Propose new use cases or patterns
- Suggest additional code examples
- Identify inconsistencies in documentation

### **Contribute Content**
- Follow the **Principle → Example → Application** structure
- Use official glossary terminology
- Include safe placeholders (`YOUR_PROJECT_ID`, `YOUR_TOKEN`)
- Verify functionality before submitting

## 📄 License

This project is under the [MIT](LICENSE) license. Speckle documentation is subject to the [official terms of use](https://speckle.systems/terms).

## 🏷️ Versions

- **v0.2** (2025-08-18): Complete support for comments/`commentThreads`
- **v0.1** (2025-08-15): Initial version with base structure

See [Changelog](16_Changelog.md) for complete details.

## 🌐 Community and Resources

- **Issues**: [GitHub Issues](https://github.com/UmbralBIM/speckle-ai-ready-docs/issues)
- **Discussions**: [GitHub Discussions](https://github.com/UmbralBIM/speckle-ai-ready-docs/discussions)
- **Community**: [Speckle Forum](https://forum.speckle.systems)

---

## 🙏 Special Acknowledgments

### **Speckle Team**
- **[Jonathon Broughton](https://www.linkedin.com/in/jonathonbroughton/)** - For his guidance, information facilitation and good disposition
- **Entire Speckle Team** - For creating and maintaining this incredible AEC collaboration platform

### **Community**
- **Contributors** - For sharing knowledge and improving this documentation
- **Users** - For testing, reporting and suggesting improvements

---

**Do you like this documentation?** ⭐ Give the repository a star!

*Maintained by the Speckle community and contributors*
