# Escenario de Make — Encuesta "¿Compras local o global?"

**Estado:** el escenario **Encuesta_Cesar** ya está creado y activo ("Immediately as data arrives"). La URL del webhook ya está en `WEBHOOK_URL` dentro de `index.html`. La hoja de cálculo **Encuesta_Compras_Local_Global** (pestaña `Respuestas`, encabezados `Fecha, Comida, Ropa, Apps, Criterio, Origen, Edad`) ya existe en la cuenta de Google del tec.

## Flujo
Página web → Webhook → Router
- **Ruta 1** (`accion` = `respuesta`): Google Sheets *Add a Row* → Webhook Response ("Respuesta registrada. Gracias por participar.").
- **Ruta 2** (`accion` = `reporte`): Google Sheets *Search Rows* (máx. 200) → Text aggregator (`{{Comida}} | {{Ropa}} | {{Apps}} | {{Criterio}} | {{Origen}} | {{Edad}}`) → Make AI Agent (Make's AI Provider) → Webhook Response con el reporte.

Los dos Webhook Response usan Status 200 y los headers `Content-Type: text/plain; charset=utf-8` y `Access-Control-Allow-Origin: *`.

## Filtros
- Ruta 1: filtro **Guardar respuesta** → `{{1.accion}}` *Equal to* `respuesta`.
- Ruta 2: filtro **Generar reporte** → `{{1.accion}}` *Equal to* `reporte`.

## Detalles importantes
- Google Sheets *Search Rows* entrega las columnas con números y no con el nombre del encabezado: `{{5.`0`}}` = Fecha, `{{5.`1`}}` = Comida, `{{5.`2`}}` = Ropa, `{{5.`3`}}` = Apps, `{{5.`4`}}` = Criterio, `{{5.`5`}}` = Origen y `{{5.`6`}}` = Edad. Por eso el text aggregator usa `{{5.`1`}} | {{5.`2`}} | … | {{5.`6`}}`.
- El prompt del agente de IA explica qué significa cada columna para que no las mezcle.

## Datos usados (aviso)
Las 20 respuestas de la hoja fueron **simuladas con IA** (no son personas reales) y se cargaron a través del formulario publicado. Están en `respuestas_simuladas.csv`.

## Verificación
Porcentajes con `COUNTIF`, por ejemplo: `=COUNTIF(Respuestas!B:B; "Mercado o tianguis") / COUNTA(Respuestas!B2:B)`. Ver la comparación en `Imagenes/04-verificacion-ia-vs-sheets.png`.
