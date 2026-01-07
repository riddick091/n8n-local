# n8n-local

Repositorio para proyectos locales de n8n y documentaciones importantes.

## 📁 Estructura del Proyecto

```
n8n-local/
├── workspace/          # Espacio de trabajo para flujos de n8n
│   ├── workflows/      # Flujos de trabajo exportados (.json)
│   ├── credentials/    # Documentación de credenciales necesarias
│   └── tests/          # Flujos de prueba y experimentación
│
└── docs/              # Documentación importante del proyecto
```

## 🚀 Uso

### Workspace de n8n

El directorio `workspace/` está diseñado para:
- **Probar flujos de n8n localmente**: Guarda y versiona tus workflows
- **Experimentar con automatizaciones**: Crea y prueba nuevos flujos
- **Compartir workflows**: Exporta/importa flujos en formato JSON

Ver [workspace/README.md](workspace/README.md) para más detalles.

### Documentación

El directorio `docs/` es para:
- Guías de uso y configuración
- Documentación técnica
- Referencias importantes
- Procedimientos y mejores prácticas

Ver [docs/README.md](docs/README.md) para más información.

## ⚠️ Seguridad

**Importante**: Este repositorio NO debe contener:
- Credenciales reales
- API keys
- Contraseñas
- Tokens de acceso
- Datos sensibles

Todos estos elementos están excluidos en `.gitignore`.

## 📝 Contribuir

1. Exporta tus workflows desde n8n en formato JSON
2. Guárdalos en `workspace/workflows/`
3. Documenta las credenciales necesarias (sin incluir valores reales)
4. Agrega documentación relevante en `docs/`
