# Laboratorio 1 — Primer Workflow de Automatización

## 1. Crear el workflow

1. Abrir n8n en `http://localhost:5678`.
2. Seleccionar **Create Workflow**.
3. Nombrarlo:

```text
Clase 1 - Operaciones de Transporte
```

4. Agregar:

```text
Manual Trigger
```

---

## 2. Crear los datos de prueba

Agregar **Edit Fields (Set)**.

Crear:

```text
id_operacion = OP001
vehiculo = TRK001
origen = Cochabamba
destino = Santa Cruz
distancia_km = 470
tiempo_estimado_min = 480
tiempo_real_min = 475
consumo_l = 62
```

Ejecutar el nodo y verificar los datos.

---

## 3. Calcular el retraso

Agregar otro **Edit Fields (Set)**.

Crear:

```text
retraso_min
```

Usar una expresión equivalente a:

```text
{{ $json.tiempo_real_min - $json.tiempo_estimado_min }}
```

Para OP001 se espera:

```text
retraso_min = -5
```

Interpretación: la operación terminó antes del tiempo estimado.

---

## 4. Detectar retrasos

Agregar un nodo **IF**.

Configurar:

```text
Valor 1: retraso_min
Operación: greater than
Valor 2: 0
```

Resultado:

```text
TRUE  → retrasada
FALSE → no retrasada
```

Cambiar temporalmente:

```text
tiempo_real_min = 600
```

Ejecutar nuevamente y comprobar `TRUE`.

---

## 5. Clasificar prioridad

Agregar una transformación y crear:

```text
prioridad
```

Regla:

```text
SI retraso_min > 60
    ALTA

SI retraso_min <= 60
    NORMAL
```

Ejemplo:

```text
retraso_min = 120
prioridad = ALTA
```

---

## 6. Detectar consumo elevado

Crear:

```text
alerta_consumo
```

Regla:

```text
SI consumo_l > 70
    SI

SI consumo_l <= 70
    NO
```

Ejemplo:

```text
consumo_l = 82
alerta_consumo = SI
```

---

## 7. Resultado mínimo

El workflow debe terminar generando:

```text
id_operacion
vehiculo
origen
destino
distancia_km
tiempo_estimado_min
tiempo_real_min
retraso_min
consumo_l
prioridad
alerta_consumo
```

Ejemplo:

```json
{
  "id_operacion": "OP005",
  "vehiculo": "TRK005",
  "retraso_min": 110,
  "prioridad": "ALTA",
  "alerta_consumo": "SI"
}
```

---

## 8. Desafío

Modificar el workflow para utilizar tres niveles de retraso:

```text
retraso <= 0       → NORMAL
1 a 60 minutos     → MODERADO
más de 60 minutos  → CRÍTICO
```

Y dos estados energéticos:

```text
consumo_l > 70     → ALERTA_ENERGETICA
consumo_l <= 70    → NORMAL
```

Probar como mínimo:

```text
OP001
OP005
OP008
```

del archivo `datos/operaciones.csv`.

---

## 9. Evidencia

Guardar:

- captura del workflow completo;
- captura de una ejecución exitosa;
- resultado de al menos tres operaciones.

La secuencia final debe ser similar a:

```text
Manual Trigger
      ↓
Datos
      ↓
Cálculo
      ↓
Reglas
      ↓
Clasificación
      ↓
Resultado
```
