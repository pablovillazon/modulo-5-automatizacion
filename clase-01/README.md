# Módulo 5 — Clase 1
## Automatización End-To-End y RPA

### Objetivo del laboratorio

Construir un primer workflow en n8n que reciba datos de una operación de transporte, calcule el retraso, aplique reglas de negocio y genere una clasificación.

### Requisitos

- Docker Desktop
- Navegador web
- Editor de texto
- Conexión a Internet para descargar n8n la primera vez

### Preparar n8n

Verificar Docker:

```bash
docker --version
```

Descargar n8n:

```bash
docker pull docker.n8n.io/n8nio/n8n
```

Ejecutar:

```bash
docker run -it --rm --name n8n -p 5678:5678 docker.n8n.io/n8nio/n8n
```

Abrir:

```text
http://localhost:5678
```

### Archivos

- `datos/operaciones.csv`: datos de práctica.
- `laboratorio-01.md`: instrucciones paso a paso.
- `workflow-clase-01.json`: workflow base de referencia.
- `README-proyecto.md`: plantilla que continuará durante las cuatro clases.

### Resultado esperado

```text
Entrada
   ↓
Transformación
   ↓
Reglas de negocio
   ↓
Clasificación
   ↓
Resultado
```

Para detener n8n:

```text
Ctrl + C
```
