# 🚀 Integración del Asistente Virtual DEVLANTIA con n8n

Este proyecto incluye una integración interactiva en tiempo real entre la landing page **DEVLANTIA** y la plataforma de automatización **n8n** alojada en [https://n8n.spiderwebargapi.com.ar/](https://n8n.spiderwebargapi.com.ar/).

---

## 📁 Archivos Incluidos

1. **`n8n-devlantia-assistant.json`**: Plantilla oficial del flujo de trabajo (*workflow*) lista para importar en n8n.
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

### Paso 3: Estructura del Flujo Importado
El flujo contiene 3 nodos principales listos para funcionar sin requerir ninguna clave de API externa:
1. **Webhook Chatbot DEVLANTIA**:
   - Método: `POST`
   - Path: `devlantia-assistant`
   - URLs generadas por n8n:
     - **Producción:** `https://n8n.spiderwebargapi.com.ar/webhook/devlantia-assistant`
     - **Test:** `https://n8n.spiderwebargapi.com.ar/webhook-test/devlantia-assistant`
   - CORS habilitado para todos los orígenes (`allowedOrigins: "*"`).
2. **Cerebro IA & Lógica DEVLANTIA**:
   - Procesa y normaliza la consulta.
   - Detecta si el usuario dejó datos de contacto (email o teléfono) y lo etiqueta como Lead.
   - Responde con la información de las 4 soluciones clave (Menú QR, Concierge IA, Desarrollo Web, Auditoría de Stock, Precios y Demos).
   - Genera respuestas enriquecidas con enlaces a las páginas simuladoras y botones de acción rápida (*chips*).
3. **Responder al Chatbot**:
   - Devuelve la respuesta en formato JSON estructurado con cabeceras CORS.

### Paso 4: Activar el Flujo
1. En la esquina superior derecha de n8n, cambia el interruptor de **Inactive** a **Active**.
2. ¡Listo! A partir de ese momento, el webhook de producción comenzará a recibir y responder en tiempo real.

---

## 🧪 Pruebas desde la Web

1. Abre `index.html` en tu navegador.
2. Abre el asistente flotante en la esquina inferior derecha.
3. Haz clic en el ícono de **⚙️ (Ajustes)** en el encabezado del chat:
   - Podrás ver la URL configurada.
   - Puedes hacer clic en **"Probar Ping"** para verificar si n8n responde en ese instante con `200 OK`.
   - Puedes alternar con un clic entre el modo **Producción** (`/webhook/`) y el modo **Test** (`/webhook-test/`) cuando estés depurando en el editor de n8n.
4. Escribe cualquier mensaje o haz clic en las opciones predeterminadas (por ejemplo: *"Menú QR"*, *"Precios"*, *"Concierge con IA"*).
5. Observa la etiqueta superior:
   - 🟢 **n8n Live**: Indica que la respuesta provino en directo desde tu servidor n8n.
   - 🟡 **Respaldo IA Local**: Se activa automáticamente si n8n está pausado o en mantenimiento para garantizar que tu web nunca falle ante un visitante.
