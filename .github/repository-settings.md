# Configuraciones Recomendadas para el Repositorio

## 📋 Configuración General

### Descripción del Repositorio
```
Documentación AI-ready estructurada y verificada para LLMs y asistentes de IA especializados en Speckle. Incluye guías de conectores, API, Power BI, scripting y mejores prácticas.
```

### Temas (Topics)
```
speckle, documentation, ai-ready, llm, connectors, api, graphql, power-bi, revit, rhino, grasshopper, blender, qgis, aec, bim, 3d-visualization, specklepy, speckle-sharp
```

### Sitio Web
```
https://speckle.systems
```

## 🔒 Configuración de Seguridad

### Branch Protection Rules
- **main**: Requerir pull request antes de merge
- **develop**: Requerir pull request antes de merge
- Habilitar "Require status checks to pass before merging"
- Habilitar "Require branches to be up to date before merging"

### Security & Analysis
- Habilitar "Dependency graph"
- Habilitar "Dependabot alerts"
- Habilitar "Dependabot security updates"
- Habilitar "Code scanning"

## 📚 Configuración de Wikis

### Habilitar Wiki
- Permitir edición por colaboradores
- Usar tema "Cayman"

### Contenido del Wiki
- Guía de contribución
- Estándares de documentación
- Proceso de revisión
- Glosario de términos

## 🚀 Configuración de GitHub Pages

### Source
- Branch: `gh-pages`
- Folder: `/ (root)`

### Custom Domain
- `docs.speckle.systems` (si está disponible)

## 🔧 Configuración de Actions

### Permisos
- Actions: "Read and write permissions"
- Workflows: "Allow GitHub Actions to create and approve pull requests"

### Variables de Entorno
- `SPECKLE_API_URL`: `https://app.speckle.systems`
- `DOCS_VERSION`: `v0.2`

## 📊 Configuración de Insights

### Habilitar
- Dependency graph
- Network
- Traffic
- Commits
- Code frequency
- Punch card

## 🏷️ Configuración de Labels

### Bug
- `bug` - Algo no funciona
- `documentation` - Mejoras o adiciones a la documentación
- `good first issue` - Bueno para nuevos contribuyentes

### Enhancement
- `enhancement` - Nueva funcionalidad o solicitud
- `help wanted` - Se busca ayuda extra
- `question` - Pregunta sobre el proyecto

### Priority
- `high` - Prioridad alta
- `medium` - Prioridad media
- `low` - Prioridad baja

## 📝 Configuración de Pull Requests

### Template
- Descripción del cambio
- Tipo de cambio
- Checklist de verificación
- Información de testing
- Screenshots (si aplica)

### Merge Options
- Allow merge commits
- Allow squash merging
- Allow rebase merging
- Automatically delete head branches

## 🔍 Configuración de Issues

### Template
- Bug report
- Feature request
- Documentation improvement
- Question

### Auto-assign
- Habilitar auto-assign para issues
- Configurar equipo por defecto

## 📋 Configuración de Projects

### Project Board
- **To Do**: Nuevas tareas
- **In Progress**: Trabajo en curso
- **Review**: Pendiente de revisión
- **Done**: Completado

### Automation
- Mover issues a "In Progress" cuando se asignen
- Mover PRs a "Review" cuando se abran
- Mover a "Done" cuando se mergeen
