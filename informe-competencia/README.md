# Informe Automático de Competencia

Genera y envía por email un informe HTML ejecutivo de inteligencia competitiva a partir de los datos recolectados por el workflow de análisis de competidores. Usa Gemini AI para redactar los textos, construye el HTML desde cero y lo adjunta como archivo.

## Problema que resuelve

Después de cada ciclo de análisis de competencia (14 días), los datos quedan en Google Sheets pero no hay una forma cómoda de leerlos de un vistazo. Este workflow cierra el loop: toma todos los hallazgos, los procesa con IA y manda un informe visual directamente al correo.

## Cómo funciona

```
Trigger diario (12:00)
  → Leer Ficha Viva (Google Sheets)
  → Preparar datos: filtrar hallazgos relevantes, calcular KPIs,
    armar prompt para Gemini
  → Leer fechas enviadas (n8n DataTable)
  → Comprobar envío: ¿ya se mandó este ciclo? → salir si sí
  ├── Leer Competidores (Google Sheets) → datos de logos y dominios
  ├── Leer Mercado (Google Sheets) → tendencias de mercado
  └── Añadir mercado: combinar todo en un solo objeto, ampliar prompt
      → Redactar textos con Gemini (resumen ejecutivo, comparaciones
        por competidor, análisis de mercado, 4-5 recomendaciones)
      → Lista de logos → Descargar logo (favicon de cada competidor)
      → Armar informe HTML: construye el email completo en JavaScript
          (cabecera con KPIs, barra de distribución, resumen IA,
           radar de competidores, hallazgos con comparación frente
           al producto propio, panorama de mercado, recomendaciones)
      → Adjuntar archivo HTML
      → Enviar informe por correo (SMTP)
      → Registrar envío + Guardar fecha enviada (DataTable)
```

También tiene un **trigger manual** para pruebas que saltea la comprobación de fecha.

## Estructura del informe HTML

El email es un HTML completo que incluye:

| Sección | Contenido |
|---|---|
| Cabecera | Fecha del ciclo, nº de competidores, páginas revisadas |
| KPIs | 4 tarjetas: páginas totales, amenazas altas/críticas, medias, sin cambios |
| Barra de distribución | Proporciones del ciclo por nivel de amenaza |
| Resumen ejecutivo | 2-3 frases generadas por Gemini |
| Radar de competidores | Grid con logo, nombre y nivel de amenaza por competidor |
| Hallazgos | Tarjetas por hallazgo: qué se encontró, por qué importa, comparación con el producto propio |
| Panorama de mercado | Resumen IA + gráfico de barras de evolución del mercado + cifras clave |
| Recomendaciones | Lista de 4-5 acciones concretas con prioridad (Alta/Media/Baja) |

El HTML se adjunta también como archivo `.html` para poder abrirlo en navegador.

## Lógica anti-duplicados

Usa una **n8n DataTable** (`informes_enviados`) que guarda la fecha del último ciclo enviado. Antes de generar nada, comprueba si ya se mandó un informe para esa fecha. El trigger manual ignora esta comprobación para facilitar las pruebas.

## Stack

- **Trigger:** Schedule (diario, 12:00) + Manual trigger para pruebas
- **Fuentes de datos:** Google Sheets (3 hojas: Ficha Viva, Mercado, Competidores)
- **IA:** Google Gemini (LangChain node, modelo `gemini-3.5-flash-lite`)
- **Anti-duplicados:** n8n DataTable (`informes_enviados`)
- **Email:** SMTP con adjunto HTML
- **Lógica:** JavaScript (Code nodes) — filtrado, construcción del prompt, render del HTML
- **Nodos:** 17
