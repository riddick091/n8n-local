# Workspace de n8n

Este directorio está destinado para almacenar y probar flujos de trabajo (workflows) de n8n.

## Uso

Puedes utilizar este directorio para:
- Probar flujos de n8n localmente
- Almacenar archivos de workflows (`.json`)
- Experimentar con nuevas automatizaciones
- Desarrollar y validar integraciones

## Estructura recomendada

```
workspace/
├── workflows/          # Flujos de trabajo exportados
├── credentials/        # Plantillas de credenciales (sin datos sensibles)
└── tests/             # Flujos de prueba
```

## Notas importantes

- **NO** incluyas credenciales reales o datos sensibles en este repositorio
- Exporta tus flujos desde n8n en formato JSON para compartir
- Documenta cualquier dependencia o configuración especial necesaria
