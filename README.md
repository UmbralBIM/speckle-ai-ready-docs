# Speckle AI-Ready Documentation

[![Speckle Version](https://img.shields.io/badge/Speckle-2.x-blue.svg)](https://speckle.systems)
[![Documentation Status](https://img.shields.io/badge/Documentation-AI--Ready-green.svg)](https://github.com/UmbralBIM/speckle-ai-ready-docs)
[![Last Updated](https://img.shields.io/badge/Last%20Updated-2025--08--18-orange.svg)](16_Changelog.md)

> **Documentación estructurada y verificada para LLMs y asistentes de IA especializados en Speckle**

## 🎯 Propósito

Esta colección de documentos proporciona una base de conocimiento **AI-ready** para asistentes de IA que necesiten resolver consultas técnicas sobre Speckle. Los documentos están estructurados siguiendo principios pedagógicos específicos y contienen información verificada sobre conectores, API, Power BI, scripting y mejores prácticas.

## 📚 Documentos Disponibles

### 📖 **Documentos Principales**
- **[01_General_Instructions.md](01_General_Instructions.md)** - Instrucciones operativas para asistentes de IA
- **[02_Master_Index.md](02_Master_Index.md)** - Índice navegable de todos los documentos
- **[04_Glossary.md](04_Glossary.md)** - Terminología oficial de Speckle
- **[05_Speckle_Key_Elements.md](05_Speckle_Key_Elements.md)** - Conceptos fundamentales

### 🔌 **Conectores y Herramientas**
- **[06_General_Connectors.md](06_General_Connectors.md)** - Instalación y compatibilidades generales
- **[07_Modeling_Connectors.md](07_Modeling_Connectors.md)** - Revit, Rhino, Grasshopper, Blender, QGIS
- **[08_Tabular_Data_Integrations.md](08_Tabular_Data_Integrations.md)** - Excel y Power BI
- **[09_Scripting_and_Code_Guide.md](09_Scripting_and_Code_Guide.md)** - `specklepy` y `speckle-sharp`

### 🚀 **API y Desarrollo**
- **[10_API_and_GraphQL.md](10_API_and_GraphQL.md)** - Autenticación, consultas y ejemplos
- **[11_PowerBI_3D_Visual_Integrations.md](11_PowerBI_3D_Visual_Integrations.md)** - Visual 3D de Power BI

### 📋 **Patrones y Mejores Prácticas**
- **[12_Common_Patterns.md](12_Common_Patterns.md)** - Flujos multi-herramienta típicos
- **[13_Problem_Solving.md](13_Problem_Solving.md)** - Diagnóstico y troubleshooting
- **[14_Best_Practices.md](14_Best_Practices.md)** - Versionado, permisos y rendimiento
- **[15_Use_Cases.md](15_Use_Cases.md)** - Casos de uso AEC específicos

### 📝 **Control de Cambios**
- **[16_Changelog.md](16_Changelog.md)** - Historial de versiones y actualizaciones

## 🎓 Metodología AI-Ready

### **Principio → Ejemplo → Aplicación**
Cada documento sigue una secuencia didáctica consistente:
1. **Principio**: Explica el concepto o capacidad técnica
2. **Ejemplo**: Proporciona código funcional con placeholders
3. **Aplicación**: Describe pasos específicos para casos de uso reales

### **Características Clave**
- ✅ **Información verificada** - Basada en pruebas reales y documentación oficial
- ✅ **Terminología consistente** - Vocabulario normalizado según el glosario
- ✅ **Ejemplos reproducibles** - Código funcional con placeholders seguros
- ✅ **Referencias cruzadas** - Enlaces internos entre documentos relacionados
- ✅ **Estructura navegable** - Índice maestro para localización rápida

## 🛠️ Casos de Uso

### **Para Asistentes de IA**
- Resolver consultas técnicas sobre implementaciones de Speckle
- Guiar usuarios en flujos de trabajo multi-herramienta
- Proporcionar ejemplos de código verificados
- Diagnosticar problemas comunes

### **Para Desarrolladores**
- Implementar integraciones con la API de Speckle
- Configurar conectores para aplicaciones AEC
- Crear dashboards en Power BI con datos 3D
- Automatizar flujos con SDKs

### **Para Equipos AEC**
- Coordinación multidisciplinaria entre herramientas
- Reporting y análisis de modelos 3D
- Control de versiones y colaboración
- Mejores prácticas de implementación

## 🚀 Inicio Rápido

### **1. Consulta Básica**
```markdown
"¿Cómo enviar datos desde Revit a Speckle?"
→ Consultar: `07_Modeling_Connectors.md` (Principio → Ejemplo → Aplicación)
```

### **2. Implementación Power BI**
```markdown
"¿Cómo configurar el Visual 3D en Power BI?"
→ Consultar: `11_PowerBI_3D_Visual_Integrations.md` + `08_Tabular_Data_Integrations.md`
```

### **3. Troubleshooting**
```markdown
"El conector de Rhino no aparece"
→ Consultar: `13_Problem_Solving.md` → Checklist diagnóstico
```

## 📋 Requisitos

- **Speckle**: Versión 3.x
- **Conectores**: DUI3 activo, permisos de proyecto
- **Power BI**: Data Extensions habilitadas, Desktop Service activo
- **SDKs**: `specklepy` (Python) o `speckle-sharp` (.NET)

## 🔗 Enlaces Útiles

- **Speckle Oficial**: [speckle.systems](https://speckle.systems)
- **GraphQL Explorer**: [app.speckle.systems/explorer](https://app.speckle.systems/explorer)
- **Comunidad**: [forum.speckle.systems](https://forum.speckle.systems)
- **Documentación**: [docs.speckle.systems](https://docs.speckle.systems)

## 🤝 Contribuciones

### **Reportar Problemas**
- Usar el [issue tracker](https://github.com/UmbralBIM/speckle-ai-ready-docs/issues)
- Incluir contexto: herramienta, versión, pasos para reproducir
- Adjuntar logs cuando sea posible

### **Sugerir Mejoras**
- Proponer nuevos casos de uso o patrones
- Sugerir ejemplos de código adicionales
- Identificar inconsistencias en la documentación

### **Contribuir Contenido**
- Seguir la estructura **Principio → Ejemplo → Aplicación**
- Usar terminología del glosario oficial
- Incluir placeholders seguros (`YOUR_PROJECT_ID`, `YOUR_TOKEN`)

## 📄 Licencia

Este proyecto está bajo la licencia [MIT](LICENSE). La documentación de Speckle está sujeta a los [términos de uso oficiales](https://speckle.systems/terms).

## 🏷️ Versiones

- **v0.2** (2025-08-18): Soporte completo para comentarios/`commentThreads`
- **v0.1** (2025-08-15): Versión inicial con estructura base

Ver [Changelog](16_Changelog.md) para detalles completos.

## 🌐 Comunidad y Recursos

- **Issues**: [GitHub Issues](https://github.com/UmbralBIM/speckle-ai-ready-docs/issues)
- **Discusiones**: [GitHub Discussions](https://github.com/UmbralBIM/speckle-ai-ready-docs/discussions)
- **Comunidad**: [Speckle Forum](https://forum.speckle.systems)

---

## 🙏 Agradecimientos Especiales

### **Equipo de Speckle**
- **[Jonathon Broughton](https://www.linkedin.com/in/jonathonbroughton/)** - Por su orientación, facilitación de información y buena disposición.
- **Todo el equipo de Speckle** - Por crear y mantener esta increíble plataforma de colaboración AEC

### **Comunidad**
- **Contribuidores** - Por compartir conocimiento y mejorar esta documentación
- **Usuarios** - Por probar, reportar y sugerir mejoras

---

**¿Te gusta esta documentación?** ⭐ ¡Dale una estrella al repositorio!
