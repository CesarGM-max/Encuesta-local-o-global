# Escenario de Make — Encuesta "¿Compras local o global?"

**Estado:** el escenario **Encuesta_Cesar** ya está creado y activo ("Immediately as data arrives"). La URL del webhook ya está en `WEBHOOK_URL` dentro de `index.html`. La hoja de cálculo **Encuesta_Compras_Local_Global** (pestaña `Respuestas`, encabezados `Fecha, Comida, Ropa, Apps, Criterio, Origen, Edad`) ya existe en la cuenta de Google del tec.

## Flujo
Página web → Webhook → Router
- **Ruta 1** (`accion` = `respuesta`): Google Sheets *Add a Row* → Webhook Response ("Respuesta registrada. Gracias por participar.").
- **Ruta 2** (`accion` = `reporte`): Google Sheets *Search Rows* (máx. 200) → Text aggregator (`{{Comida}} | {{Ropa}} | {{Apps}} | {{Criterio}} | {{Origen}} | {{Edad}}`) → Make AI Agent (Make's AI Provider) → Webhook Response con el reporte.

Los dos Webhook Response usan Status 200 y los headers `Content-Type: text/plain; charset=utf-8` y `Access-Control-Allow-Origin: *`.

## Pendiente
1. Publicar `index.html` con GitHub Pages (Settings → Pages → Branch `main`).
2. Aplicar la encuesta a 20 personas desde la página publicada.
3. Presionar *Generar reporte* y guardar el resultado.
4. Verificar en Sheets al menos 2 preguntas con `COUNTIF`, por ejemplo: `=COUNTIF(Respuestas!B:B; "Mercado o tianguis") / COUNTA(Respuestas!B2:B)`.
5. Responder las 5 preguntas del informe con los resultados reales.
