# Informe Automático de Competencia

Cierra el ciclo de inteligencia competitiva: toma los hallazgos del análisis de los 10 competidores y las 5 fuentes de mercado, los procesa con Gemini AI y envía a jefatura un informe HTML ejecutivo con resumen, radar visual, análisis de mercado y recomendaciones. Todo automatizado, cada 14 días.

## El problema

Después de cada ciclo de análisis, los datos quedan en Google Sheets pero leerlos fila por fila no es una experiencia útil para tomar decisiones. Se necesitaba una forma de transformar esos datos en algo que se pudiera revisar en 5 minutos: un informe con contexto, comparaciones y acciones concretas. Redactarlo a mano tomaría otra hora encima de las 6-8 horas ya invertidas en el análisis.

## Qué recibe jefatura cada ciclo

Un email HTML con:
- **KPIs del ciclo:** páginas revisadas, amenazas altas/críticas, medias, sin cambios
- **Resumen ejecutivo** de 2-3 frases redactado por Gemini
- **Radar de competidores:** grid visual con logo de cada competidor y su nivel de amenaza
- **Hallazgos detallados:** qué se encontró, por qué importa, comparación directa con el producto propio
- **Panorama de mercado:** tendencias, gráfico de evolución del mercado, cifras clave
- **4-5 recomendaciones** ordenadas por prioridad (Alta / Media / Baja)

El informe también se adjunta como archivo `.html` para abrirlo en navegador con mejor visualización.

## Cómo funciona

```
Trigger diario (12:00)
  → Leer Ficha Viva (Google Sheets)
  → Preparar datos: filtrar hallazgos relevantes, calcular KPIs,
    armar prompt para Gemini
  → Leer fechas enviadas (n8n DataTable)
  → Comprobar: ¿ya se envió este ciclo? → salir si sí
  ├── Leer Competidores (Google Sheets) → logos y dominios
  ├── Leer Mercado (Google Sheets) → tendencias del sector
  └── Combinar todo en un único prompt enriquecido
      → Gemini AI: redactar resumen, comparaciones,
        análisis de mercado y recomendaciones en JSON
      → Descargar favicon de cada competidor
      → Construir HTML completo en JavaScript
      → Adjuntar como archivo
      → Enviar por SMTP
      → Registrar ciclo como enviado (DataTable)
```

También tiene un trigger manual para pruebas que saltea la comprobación de fecha.

## Lógica anti-duplicados

Una n8n DataTable (`informes_enviados`) guarda la fecha de cada ciclo enviado. Si el trigger diario detecta que ya se envió el informe de este ciclo, sale sin hacer nada. El trigger manual ignora esta comprobación.

## Stack
- **Trigger:** Schedule (diario, 12:00) + Manual trigger para pruebas
- **Fuentes:** Google Sheets (3 hojas: Ficha Viva, Mercado, Competidores)
- **IA:** Google Gemini (LangChain node, `gemini-3.5-flash-lite`)
- **Anti-duplicados:** n8n DataTable
- **Email:** SMTP con adjunto HTML
- **Lógica:** JavaScript (Code nodes) — filtrado, prompt, render HTML completo
- **Nodos:** 17
