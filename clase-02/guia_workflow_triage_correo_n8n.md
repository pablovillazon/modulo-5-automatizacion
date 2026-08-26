# Guía paso a paso: Triage inteligente de correo con n8n

Automatización de Procesos — Proyecto de módulo

## Objetivo

Construir un workflow en n8n que reciba correos entrantes, use un LLM para clasificarlos y redactar una respuesta sugerida, ramifique la acción según la categoría (urgente, consulta simple, venta/complejo) y registre todo en una hoja de cálculo para métricas.

## Requisitos previos

- Cuenta de n8n (self-hosted en Docker o n8n Cloud, versión gratuita alcanza para el proyecto)
- Cuenta de Gmail (o Outlook) con acceso habilitado para apps de terceros
- API key de un proveedor de LLM (OpenAI, Anthropic, o similar)
- Cuenta de Slack con un workspace de prueba (para las notificaciones de urgencias)
- Cuenta de Google con una hoja de Google Sheets creada para el registro
- Nociones básicas de JSON (n8n mueve datos en este formato entre nodos)

## Paso 1: Preparar el entorno de n8n

1. Si usarás n8n Cloud, crea una cuenta gratuita en n8n.io y accede al editor.
2. Si prefieres self-hosted, instala con Docker:
   ```
   docker run -it --rm -p 5678:5678 n8nio/n8n
   ```
3. Abre `http://localhost:5678` (o la URL de tu instancia cloud) y crea un nuevo workflow en blanco.

## Paso 2: Configurar el trigger de Gmail

1. Agrega un nodo **Gmail Trigger** (busca "Gmail" en el panel de nodos).
2. Autentica tu cuenta de Google con OAuth2 (n8n te guía con un flujo de consentimiento).
3. Configura el evento como "Message Received" y filtra por la bandeja de entrada principal.
4. Define el intervalo de polling (por ejemplo, cada 1 minuto) para pruebas.
5. Ejecuta el nodo manualmente ("Execute Node") enviándote un correo de prueba para verificar que los datos llegan correctamente (asunto, remitente, cuerpo).

## Paso 3: Agregar el nodo de clasificación con LLM

1. Agrega un nodo **OpenAI** (o **HTTP Request** si usas otro proveedor sin nodo nativo).
2. Configura las credenciales con tu API key.
3. Selecciona la operación "Message" / "Chat" y construye el prompt, por ejemplo:
   ```
   Eres un asistente de triage de correo. Clasifica el siguiente correo en una
   de estas categorías: "urgente", "consulta_simple", "venta_compleja".
   Luego redacta un borrador breve de respuesta apropiado.

   Responde SOLO en JSON con este formato:
   {"categoria": "...", "borrador": "..."}

   Asunto: {{ $json.subject }}
   Cuerpo: {{ $json.textPlain }}
   ```
4. Usa las expresiones de n8n (`{{ $json.campo }}`) para insertar dinámicamente los datos del correo recibido en el paso anterior.
5. Ejecuta el nodo y revisa que la respuesta del modelo sea un JSON válido con `categoria` y `borrador`.

## Paso 4: Parsear la respuesta del LLM

1. Agrega un nodo **Set** (o **Code** si prefieres JavaScript) para extraer `categoria` y `borrador` del texto devuelto por el LLM y convertirlos en campos estructurados del ítem.
2. Esto evita tener que parsear JSON dentro de cada nodo posterior.

## Paso 5: Crear la ramificación con el nodo Switch

1. Agrega un nodo **Switch**.
2. Configura tres salidas basadas en el valor del campo `categoria`:
   - Salida 0: `categoria` es igual a `urgente`
   - Salida 1: `categoria` es igual a `consulta_simple`
   - Salida 2: `categoria` es igual a `venta_compleja`

## Paso 6: Rama "urgente" → notificación en Slack

1. Conecta la salida 0 del Switch a un nodo **Slack**.
2. Autentica con tu workspace (OAuth2 o Webhook entrante).
3. Configura el canal de destino y el mensaje, incluyendo remitente, asunto y el borrador generado por el LLM.

## Paso 7: Rama "consulta simple" → respuesta automática

1. Conecta la salida 1 del Switch a un nodo **Gmail** (operación "Send Email" o "Reply").
2. Usa el campo `borrador` como cuerpo del correo de respuesta.
3. Para el proyecto académico, considera agregar un nodo intermedio de "espera" o revisión manual antes del envío real, para evitar respuestas automáticas no deseadas durante las pruebas.

## Paso 8: Rama "venta/compleja" → guardar borrador para revisión

1. Conecta la salida 2 del Switch a un nodo **Gmail** con la operación "Create Draft" (crear borrador, no enviar), o a un nodo de Google Docs/Sheets donde quede pendiente de revisión humana.

## Paso 9: Unificar las ramas y registrar en Google Sheets

1. Agrega un nodo **Merge** (modo "Append") para unir las tres ramas en un único flujo de salida.
2. Conecta el resultado a un nodo **Google Sheets** (operación "Append Row").
3. Autentica con tu cuenta de Google.
4. Mapea las columnas: fecha, remitente, asunto, categoría, acción tomada.

## Paso 10: Probar el workflow de extremo a extremo

1. Activa el workflow ("Active" en la esquina superior derecha).
2. Envíate tres correos de prueba, uno por cada categoría esperada, para validar que cada rama se ejecuta correctamente.
3. Revisa el panel de ejecuciones ("Executions") de n8n para depurar errores en cualquier nodo.
4. Verifica que la hoja de Google Sheets se actualice con cada ejecución.

## Paso 11: Documentar y preparar la presentación

1. Exporta el workflow como JSON (menú "..." → "Download") para incluirlo como anexo técnico.
2. Toma capturas de pantalla del canvas de n8n y de una ejecución exitosa.
3. Prepara 2-3 ejemplos de correos de entrada y sus resultados para mostrar en vivo o en video durante la presentación.

## Notas sobre factibilidad para el proyecto

- Todo el flujo usa nodos nativos de n8n (Gmail, OpenAI, Slack, Google Sheets), sin necesidad de infraestructura adicional ni aprobaciones de negocio.
- El tiempo estimado de construcción es de 3 a 5 horas, incluyendo pruebas.
- El principal costo es el uso de la API del LLM, que para pruebas de clase es mínimo (unos pocos centavos de dólar por decenas de ejecuciones).
- Para simplificar aún más el proyecto, se puede omitir la rama de Slack y quedarse solo con dos ramas (consulta simple y compleja), o reemplazar Slack por Telegram si el equipo no cuenta con un workspace propio.
