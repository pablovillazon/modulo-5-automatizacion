# Módulo 5 — Clase 1
## Automatización End-To-End, RPA y workflows

**Duración:** 3 horas  
**Enfoque:** teoría breve + demostración + laboratorio  
**Herramienta principal:** n8n  
**Caso:** operaciones de transporte

---

# 1. Propósito de la clase

La teoría de esta clase debe responder cuatro preguntas:

1. ¿Qué es un proceso automatizable?
2. ¿Qué diferencia hay entre workflow automation y RPA?
3. ¿Cómo se representa una automatización End-To-End?
4. ¿Cómo una herramienta como n8n convierte datos, reglas y acciones en un proceso ejecutable?


---

# 2. ¿Qué vamos a automatizar?

## Caso de trabajo

Una operación de transporte genera información:

```text
Vehículo
Origen
Destino
Distancia
Tiempo estimado
Tiempo real
Consumo
```

Actualmente una persona podría:

```text
1. Recibir los datos
2. Revisarlos
3. Calcular el retraso
4. Revisar el consumo
5. Clasificar la operación
6. Registrar el resultado
7. Avisar si existe un problema
```

### Pregunta para iniciar

> ¿Cuáles de estos pasos necesitan realmente que una persona los ejecute?

---

# 3. ¿Qué es un proceso?

Un **proceso** es un conjunto ordenado de actividades que transforma entradas en resultados para alcanzar un objetivo.

Modelo simple:

```text
ENTRADA
   ↓
ACTIVIDADES
   ↓
DECISIONES
   ↓
RESULTADO
```

### Ejemplo

```text
Datos de operación
       ↓
Calcular retraso
       ↓
¿Supera el límite?
       ↓
Clasificar
       ↓
Generar alerta
```

### Idea clave

No automatizamos "tareas" de manera aislada sin entender el proceso que las conecta.

---

# 4. Tarea, actividad y proceso

### Tarea

Acción concreta:

```text
Copiar un dato
```

### Actividad

Conjunto pequeño de tareas:

```text
Validar una operación
```

### Proceso

Secuencia completa orientada a un resultado:

```text
Recibir → validar → decidir → registrar → notificar
```

### Para hablar con propiedad

Usar:

- **tarea** para una acción puntual;
- **actividad** para un bloque de trabajo;
- **proceso** para la secuencia completa.

---

# 5. ¿Qué es automatización?

La automatización consiste en utilizar tecnología para ejecutar actividades con menor intervención humana.

No significa necesariamente:

```text
"eliminar personas"
```

Sino:

```text
reducir trabajo manual repetitivo
+
estandarizar decisiones
+
aumentar velocidad
+
mejorar trazabilidad
```

La automatización empresarial puede abarcar BPA, workflow automation, RPA y otras tecnologías. citeturn0search2turn0search8

---

# 6. ¿Qué hace automatizable a un proceso?

Un proceso suele ser buen candidato cuando tiene:

- alto volumen;
- repetición;
- reglas claras;
- entradas relativamente estructuradas;
- pasos previsibles;
- bajo nivel de juicio humano;
- alto costo de error manual.

### Ejemplo

```text
SI retraso > 60 minutos
ENTONCES prioridad = ALTA
```

Esta regla es excelente para automatizar.

### En cambio

```text
"Determine si el retraso es razonable
considerando todas las circunstancias."
```

requiere más contexto y juicio.

---

# 7. Automatizar no es solo "hacer clic"

Una automatización madura considera:

```text
Entrada
   ↓
Validación
   ↓
Transformación
   ↓
Reglas
   ↓
Decisión
   ↓
Acción
   ↓
Registro
```

### Concepto nuevo: trazabilidad

La **trazabilidad** es la capacidad de saber qué ocurrió durante la ejecución:

```text
qué entró
qué se procesó
qué decisión se tomó
qué resultado se produjo
```

---

# 8. Workflow

Un **workflow** es una secuencia de pasos conectados que permite ejecutar un proceso.

Ejemplo:

```text
Entrada
   ↓
Calcular
   ↓
Evaluar
   ↓
Decidir
   ↓
Actuar
```

En herramientas de automatización, cada bloque puede representar una acción, transformación, condición o integración.

---

# 9. Nodo

En n8n, un **nodo** es un bloque que realiza una operación dentro del workflow.

Ejemplos:

```text
Manual Trigger
Edit Fields
IF
Google Sheets
HTTP Request
Email
```

Una forma sencilla de explicarlo:

> Un nodo recibe información, hace algo con ella y puede entregar información al siguiente nodo.

---

# 10. Trigger

Un **trigger** es el evento que inicia una automatización.

Ejemplos:

```text
Manual
   ↓
Ejecutar ahora

Horario
   ↓
Ejecutar cada día

Webhook
   ↓
Ejecutar cuando llega una solicitud

Nuevo correo
   ↓
Ejecutar cuando llega un mensaje
```

En nuestro laboratorio utilizamos:

```text
Manual Trigger
```

porque permite experimentar rápidamente.

---

# 11. Datos de entrada

Toda automatización necesita información sobre la cual trabajar.

Ejemplos:

```text
CSV
Excel
Google Sheets
Base de datos
API
Formulario
Correo electrónico
Webhook
```

En nuestro ejercicio:

```text
id_operacion
vehiculo
origen
destino
distancia_km
tiempo_estimado_min
tiempo_real_min
consumo_l
```

---

# 12. Transformación

Una transformación cambia los datos para obtener información útil.

Ejemplo:

```text
tiempo_real = 650
tiempo_estimado = 540
```

Transformación:

```text
650 - 540
```

Resultado:

```text
retraso = 110 minutos
```

Concepto nuevo:

> **Dato → transformación → información**

---

# 13. Regla de negocio

Una **regla de negocio** expresa una condición que determina qué debe ocurrir.

Ejemplo:

```text
SI retraso > 60
ENTONCES prioridad = ALTA
```

Otra:

```text
SI consumo > 70
ENTONCES alerta = SI
```

Las reglas convierten criterios operativos en lógica ejecutable.

---

# 14. Condición

Una condición evalúa si algo es verdadero o falso.

```text
retraso > 60
```

puede producir:

```text
TRUE
```

o:

```text
FALSE
```

Esto permite construir caminos diferentes:

```text
             ¿retraso > 60?
                  │
             ┌────┴────┐
            SI        NO
            ↓          ↓
         CRÍTICO     NORMAL
```

---

# 15. Workflow determinista

Un workflow basado en reglas es **determinista** cuando, ante la misma entrada y las mismas reglas, esperamos el mismo resultado.

Ejemplo:

```text
Entrada:
retraso = 120

Regla:
retraso > 60

Resultado:
ALTA
```

### Ventaja

Es:

- predecible;
- verificable;
- repetible;
- fácil de auditar.

Este concepto será importante cuando posteriormente comparemos reglas deterministas con IA generativa.

---

# 16. Workflow Automation vs RPA

## Workflow Automation

Orquesta procesos mediante:

```text
datos
APIs
servicios
reglas
eventos
acciones
```

## RPA

Utiliza robots de software para ejecutar tareas imitando interacciones humanas con aplicaciones, especialmente cuando existen tareas repetitivas y basadas en reglas. citeturn0search0turn0search4

### Simplificación didáctica

```text
Workflow
conecta sistemas y procesos

RPA
interactúa con aplicaciones como lo haría una persona
```

No son tecnologías excluyentes.

---

# 17. ¿Cuándo pensar en RPA?

RPA es especialmente útil cuando:

```text
No existe una API conveniente
        ↓
El sistema es antiguo
        ↓
La tarea se realiza mediante interfaz gráfica
        ↓
La actividad es repetitiva y basada en reglas
```

Ejemplos:

```text
Abrir aplicación
↓
Copiar dato
↓
Pegar dato
↓
Presionar botón
↓
Descargar archivo
```

Microsoft describe los flujos de escritorio como una forma de automatizar procesos repetitivos mediante acciones sobre aplicaciones y herramientas de escritorio. citeturn0search10

---

# 18. RPA atendida y desatendida

### RPA atendida

El usuario participa.

```text
Usuario
   ↓
Inicia
   ↓
Bot ejecuta
   ↓
Usuario continúa
```

### RPA desatendida

El bot ejecuta sin intervención humana directa.

```text
Evento / horario
       ↓
Bot
       ↓
Proceso
       ↓
Resultado
```

Esta distinción será retomada cuando trabajemos con Power Automate Desktop.

Microsoft distingue estos dos escenarios en sus materiales de RPA. citeturn0search13

---

# 19. No-Code y Low-Code

## No-Code

Permite construir automatizaciones con poca o ninguna programación tradicional.

```text
Seleccionar
Arrastrar
Configurar
Conectar
```

## Low-Code

Permite combinar componentes visuales con expresiones, código o lógica técnica cuando es necesario.

### Idea clave

No-Code y Low-Code no significan:

```text
"sin lógica"
```

Significan:

```text
menos código manual
+
más componentes reutilizables
```

---

# 20. n8n

n8n es la plataforma que utilizaremos para construir los workflows.

Conceptualmente:

```text
Trigger
   ↓
Nodo
   ↓
Nodo
   ↓
Condición
   ↓
Acción
```

La documentación de n8n utiliza el concepto de workflows compuestos por nodos y permite trabajar con datos y expresiones dentro de ellos.

**Recurso oficial:** urln8n Documentationhttps://docs.n8n.io/

---

# 21. Nuestro primer workflow

```text
Manual Trigger
      ↓
Datos de operación
      ↓
Calcular y clasificar
```

Entrada:

```text
tiempo_estimado = 540
tiempo_real = 650
consumo = 82
```

Procesamiento:

```text
650 - 540 = 110
```

Reglas:

```text
110 > 60
→ ALTA

82 > 70
→ ALERTA
```

---

# 22. Observar una ejecución

No basta con comprobar:

```text
Execution successful
```

Debemos preguntar:

```text
¿Qué entró?
¿Qué transformó?
¿Qué regla aplicó?
¿Qué salió?
```

### Modelo de lectura

```text
INPUT
  ↓
PROCESS
  ↓
OUTPUT
```

Esto introduce una habilidad fundamental:

> **leer y depurar una automatización.**

---

# 23. Experimentación

Una automatización se aprende modificando sus entradas.

Ejemplo:

```text
tiempo_estimado = 480
tiempo_real = 475
```

Resultado:

```text
retraso = -5
```

Cambiar:

```text
tiempo_real = 520
```

Resultado:

```text
retraso = 40
```

Cambiar:

```text
tiempo_real = 600
```

Resultado:

```text
retraso = 120
```

### Pregunta

> ¿En qué momento cambia la decisión?

---

# 24. Umbral

Un **umbral** es un valor que determina el cambio de comportamiento de una regla.

Ejemplo:

```text
retraso > 60
```

Visualmente:

```text
0 ─────────── 60 ───────────────→
              ↑
           umbral

NORMAL                  ALTA
```

Los umbrales aparecen constantemente en automatización:

```text
stock < mínimo
tiempo > límite
costo > presupuesto
consumo > objetivo
```

---

# 25. De una operación a muchas

Primera versión:

```text
Una operación
     ↓
Workflow
     ↓
Resultado
```

Siguiente versión:

```text
CSV
 ↓
Muchas operaciones
 ↓
Workflow
 ↓
Resultados
```

La pregunta deja de ser:

> "¿Puedo automatizar esta tarea?"

y pasa a ser:

> "¿Puedo automatizar este proceso de forma repetible?"

---

# 26. End-To-End

Una automatización **End-To-End** cubre el proceso desde la entrada hasta el resultado final.

```text
ENTRADA
   ↓
VALIDACIÓN
   ↓
PROCESAMIENTO
   ↓
DECISIÓN
   ↓
ACCIÓN
   ↓
REGISTRO
   ↓
INDICADOR
```

En las siguientes clases añadiremos:

```text
IA generativa
KPI
Dashboard
RPA
```

---

# 27. Automatización + IA

En este módulo la IA no reemplaza automáticamente todas las reglas.

Podemos combinar:

```text
Reglas deterministas
+
IA generativa
+
RPA
+
Workflows
```

Ejemplo:

```text
Correo
  ↓
IA extrae información
  ↓
Reglas clasifican
  ↓
RPA actualiza sistema
  ↓
KPI registra resultado
```

Esto será el puente hacia la Clase 2.

---

# 28. Resumen de Conceptos 

Al finalizar la clase, el estudiante debe poder explicar:

| Término | Idea simple |
|---|---|
| Proceso | Secuencia para alcanzar un resultado |
| Tarea | Acción puntual |
| Workflow | Secuencia automatizada de pasos |
| Trigger | Evento que inicia el workflow |
| Nodo | Bloque que ejecuta una operación |
| Input | Información de entrada |
| Output | Resultado de un procesamiento |
| Transformación | Cambio o cálculo sobre los datos |
| Regla de negocio | Condición que determina una acción |
| Condición | Evaluación TRUE/FALSE |
| Umbral | Valor que cambia una decisión |
| RPA | Robot que ejecuta tareas digitales |
| No-Code | Construcción visual con poco/sin código |
| Low-Code | Construcción visual + lógica técnica |
| End-To-End | Automatización de principio a fin |
| Trazabilidad | Capacidad de seguir qué ocurrió |
| Determinista | Misma entrada + mismas reglas → mismo resultado |

---

# 30. Errores frecuentes

### Error 1

Automatizar un proceso mal diseñado.

```text
Proceso malo
   ↓
Automatización
   ↓
Proceso malo más rápido
```

### Error 2

No definir qué debe ocurrir cuando algo falla.

### Error 3

No registrar resultados.

### Error 4

Usar RPA cuando existe una integración/API mucho más robusta.

### Error 5

Automatizar decisiones ambiguas sin controles.

---

# 31. Checklist de una buena automatización

Antes de construir:

```text
☐ ¿Cuál es el objetivo?
☐ ¿Cuál es la entrada?
☐ ¿Cuál es el resultado?
☐ ¿Qué pasos son repetitivos?
☐ ¿Qué reglas existen?
☐ ¿Qué puede fallar?
☐ ¿Cómo sabremos que funcionó?
☐ ¿Qué debe quedar registrado?
```

---

# 32. Actividad de cierre

Analizar un proceso propio.

Completar:

```text
Proceso:
________________________

Entrada:
________________________

Tarea repetitiva:
________________________

Regla:
SI _____________________
ENTONCES _______________

Resultado:
________________________
```

Después preguntar:

> ¿Lo resolverías con una regla, un workflow, RPA o una combinación?

---


# 34. Bibliografía y recursos

## Documentación técnica

### n8n

Documentación oficial para workflows, nodos, expresiones, integraciones y automatizaciones.

urln8n Documentation https://docs.n8n.io/

### Microsoft Power Automate

Documentación oficial sobre flujos de escritorio y RPA.

urlPower Automate — Introducción a flujos de escritoriohttps://learn.microsoft.com/es-es/power-automate/desktop-flows/introduction

### IBM — RPA

Introducción a RPA, casos de uso y relación con automatización de procesos.

urlIBM — ¿Qué es RPA?https://www.ibm.com/es-es/think/topics/rpa

### UiPath — RPA

Material introductorio sobre RPA y automatización de tareas repetitivas.

urlUiPath — What is Robotic Process Automation?https://www.uipath.com/rpa/robotic-process-automation

---

# 35. Recursos de video

Para el material del diplomado se recomienda priorizar:

### 1. n8n — YouTube

Buscar en el canal oficial:

```text
n8n workflow automation
n8n beginner tutorial
n8n nodes
```

urln8n en YouTubehttps://www.youtube.com/@n8n-io

### 2. Microsoft Power Automate

Buscar:

```text
Power Automate Desktop RPA
Desktop flows
RPA tutorial
```

urlMicrosoft Power Automate en YouTubehttps://www.youtube.com/@MicrosoftPowerAutomate

### 3. UiPath

Buscar:

```text
RPA basics
What is RPA?
RPA automation examples
```

urlUiPath en YouTubehttps://www.youtube.com/@UiPath


---

# 36. Lecturas recomendadas

### Nivel 1 — Introducción

IBM. **¿Qué es la automatización de procesos robóticos (RPA)?**

urlLectura IBM sobre RPAhttps://www.ibm.com/es-es/think/topics/rpa

### Nivel 2 — Workflow

IBM. **What Is Workflow Automation?**

urlLectura IBM sobre Workflow Automationhttps://www.ibm.com/think/topics/workflow-automation

### Nivel 3 — RPA aplicado

Microsoft Learn. **Introducción a flujos de escritorio.**

urlMicrosoft Learn — Desktop flowshttps://learn.microsoft.com/es-es/power-automate/desktop-flows/introduction

### Nivel 4 — Herramienta

n8n. **Documentación oficial.**

urln8n Documentationhttps://docs.n8n.io/

---

# 37. Referencias bibliográficas para el docente

- IBM. *¿Qué es la automatización de procesos robóticos (RPA)?*
- IBM. *¿Qué es la automatización de procesos empresariales (BPA)?*
- IBM. *What Is Workflow Automation?*
- Microsoft Learn. *Introducción a flujos de escritorio.*
- UiPath. *What is Robotic Process Automation?*
- n8n. *Documentation.*

Las definiciones deben utilizarse como apoyo y no como contenido para memorizar.

---


