# 🚀 Integración del Asistente Virtual DEVLANTIA con n8n + Ollama AI

Este proyecto incluye una integración interactiva en tiempo real entre la landing page **DEVLANTIA** y la plataforma de automatización **n8n** alojada en [https://n8n.spiderwebargapi.com.ar/](https://n8n.spiderwebargapi.com.ar/), basada en el modelo de **AI Agent (LangChain) + Ollama + Memory**.

---

## 📁 Archivos Incluidos

1. **`n8n-devlantia-assistant.json`**: Plantilla oficial del flujo de trabajo (*workflow*) construida con el modelo de LangChain AI Agent + Ollama + Buffer Memory:
   - **Webhook Chatbot DEVLANTIA** (`n8n-nodes-base.webhook`): Recibe las consultas desde la landing page.
   - **Armar catálogo DEVLANTIA** (`n8n-nodes-base.code`): Estructura el catálogo completo de las 4 soluciones (Menú QR, Concierge IA, Desarrollo Web, Auditoría de Stock, precios de setup $350k ARS y enlaces a las demos).
   - **AI Agent** (`@n8n/n8n-nodes-langchain.agent`): Agente conversacional con System Prompt especializado en DEVLANTIA.
   - **Ollama Chat Model** (`@n8n/n8n-nodes-langchain.lmChatOllama`): Conectado con el modelo `LiquidAI/lfm2.5-350m:latest` y credenciales `Ollama account` (`9hTpdwJqxD2ghaBr`).
   - **Simple Memory** (`@n8n/n8n-nodes-langchain.memoryBufferWindow`): Memoria contextual por sesión (`sessionId`, ventana de 8 mensajes).
   - **Formatear Respuesta & Chips** (`n8n-nodes-base.code`): Infiere botones interactivos contextuales (*chips*) según la respuesta generada.
   - **Responder al Webhook** (`n8n-nodes-base.respondToWebhook`): Devuelve el JSON con cabeceras CORS (`Access-Control-Allow-Origin: *`).

2. **`index.html`**: Frontend interactivo con:
   - Envío asíncrono vía `fetch` al Webhook de n8n.
   - Indicador de estado en vivo (🟢 n8n Live / 🟡 Respaldo IA Local).
   - Panel de ajustes ⚙️ integrado para alternar entre Webhook de **Producción** y **Test (Editor)**.
   - Botón de **Ping / Prueba de Conexión** directa desde la web.
   - Motor de respaldo inteligente: si el webhook está inactivo o pausado, el asistente responde localmente de inmediato sin romper la experiencia del usuario.

---

## 🛠️ Paso a Paso para Importar el Flujo en n8n

### Paso 1: Abrir n8n
Ingresa a tu instancia de n8n:  
👉 **[https://n8n.spiderwebargapi.com.ar/](https://n8n.spiderwebargapi.com.ar/)**

### Paso 2: Crear / Importar Flujo
1. En el panel izquierdo de n8n, ve a **Workflows** y haz clic en **Add Workflow** (o el botón **+**).
2. Haz clic en los **tres puntos (⋮)** de la esquina superior derecha del editor de n8n.
3. Selecciona **Import from File...** (Importar desde archivo).
4. Elige el archivo **`n8n-devlantia-assistant.json`** ubicado en la carpeta de este proyecto.

### Paso 3: Verificar la Conexión Ollama
El nodo **Ollama Chat Model** ya viene preconfigurado con tu credencial existente `Ollama account` (`9hTpdwJqxD2ghaBr`) y el modelo `LiquidAI/lfm2.5-350m:latest`.
*(Si en tu servidor tienes otro modelo instalado en Ollama como `llama3`, `mistral`, etc., puedes seleccionarlo directamente en la lista desplegable del nodo)*.

### Paso 4: Activar el Flujo
1. En la esquina superior derecha de n8n, cambia el interruptor de **Inactive** a **Active**.
2. ¡Listo! A partir de ese momento, el webhook de producción comenzará a responder con Inteligencia Artificial generativa contextualizada con tu catálogo.

---

## 🧪 Pruebas desde la Web

1. Abre `index.html` en tu navegador.
2. Abre el asistente flotante en la esquina inferior derecha.
3. Haz clic en el ícono de **⚙️ (Ajustes)** en el encabezado del chat:
   - Podrás ver la URL configurada.
   - Puedes hacer clic en **"Probar Ping"** para verificar si n8n responde en ese instante con `200 OK`.
   - Puedes alternar con un clic entre el modo **Producción** (`/webhook/`) y el modo **Test** (`/webhook-test/`) cuando estés depurando en el editor de n8n.
4. Escribe cualquier mensaje o haz clic en las opciones predeterminadas (por ejemplo: *"¿Qué soluciones tienen para una bodega?"*, *"¿Cuánto sale el Concierge con IA?"*, *"Menú QR para restaurante"*).
5. Observa la etiqueta superior:
   - 🟢 **n8n Live**: Indica que la respuesta provino en directo desde tu servidor n8n con Ollama AI.
   - 🟡 **Respaldo IA Local**: Se activa automáticamente si n8n está pausado o en mantenimiento para garantizar que tu web nunca falle ante un visitante.
