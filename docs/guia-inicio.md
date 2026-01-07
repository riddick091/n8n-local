# Guía de Inicio - n8n

## ¿Qué es n8n?

n8n es una herramienta de automatización de flujos de trabajo (workflow automation) que permite conectar diferentes aplicaciones y servicios.

## Instalación Local

### Con Docker

```bash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

### Con npm

```bash
npm install n8n -g
n8n
```

## Acceso

Una vez iniciado, accede a n8n en: `http://localhost:5678`

## Recursos Útiles

- [Documentación oficial](https://docs.n8n.io/)
- [Comunidad](https://community.n8n.io/)
- [Ejemplos de workflows](https://n8n.io/workflows)
- [Nodos disponibles](https://n8n.io/integrations)

## Conceptos Básicos

### Workflows (Flujos de Trabajo)
Son secuencias de nodos conectados que automatizan tareas.

### Nodos
Son los bloques de construcción de los workflows. Cada nodo realiza una tarea específica.

### Conexiones
Los nodos se conectan entre sí para pasar datos de uno a otro.

### Credenciales
Son las configuraciones de autenticación necesarias para conectar con servicios externos.

## Mejores Prácticas

1. **Nombra tus workflows descriptivamente**
2. **Usa notas** para documentar partes complejas
3. **Versiona tus workflows** exportándolos a JSON
4. **Prueba en un entorno separado** antes de producción
5. **NO guardes credenciales** en los archivos de workflow
