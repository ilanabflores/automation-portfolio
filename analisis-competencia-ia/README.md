# Análisis de Competencia con IA (Automático)

Scraping automatizado de competidores con análisis de inteligencia artificial cada 14 días. Lee las webs de los competidores, compara con la revisión anterior y genera hallazgos accionables clasificados por nivel de amenaza.

## Problema que resuelve

Hacer seguimiento manual de competidores (revisar sus webs, precios, funcionalidades, mensajes) lleva horas y se hace de forma inconsistente. Este workflow lo hace solo cada dos semanas: lee las páginas, compara con el análisis anterior y genera un informe estructurado con solo los cambios relevantes.

## Cómo funciona

```
Trigger (diario 10:00 — ejecuta el análisis cada 14 días)
  → Verificar si han pasado ≥14 días desde el último ciclo
  → Leer lista de competidores desde Google Sheets
  → Leer hallazgos previos (para comparar cambios)
  → Por cada URL de competidor (home, pricing, blog):
      ├── HTTP Scraper → extracción de texto limpio
      ├── Esperar 3s (control de rate limit)
      ├── Buscar hallazgo anterior para esta fuente
      ├── ¿Es fuente de mercado o competidor?
      │     ├── Competidor → Gemini AI: análisis de producto/comunicación
      │     └── Mercado → Gemini AI: análisis de tendencias de sector
      ├── Parsear respuesta JSON de Gemini
      └── Guardar en Google Sheets (Hallazgos / Mercado)
  → Registrar fecha del ciclo completado
```

## Output generado

**Por competidor (hoja Hallazgos):**
- Tipo: Producto / Comunicación / Sin cambios / Error de lectura
- Funcionalidades detectadas
- Precio (si está publicado)
- Tono de comunicación
- Hallazgo: qué cambió exactamente desde la revisión anterior
- Por qué importa para el negocio
- Acción recomendada: Ignorar / Investigar / Comunicar / Valorar para producto / Trasladar a ventas
- Nivel de amenaza: Baja / Media / Alta / Crítica

**Tendencias de mercado (hoja Mercado):**
- Tendencia identificada
- Tamaño/crecimiento del mercado (si se menciona)
- Implicación para el negocio
- Relevancia

## Stack
- **Trigger:** Schedule diario + lógica de ciclos (cada 14 días)
- **Fuentes:** HTTP Scraper (webs de competidores) + Google Sheets (lista de URLs y hallazgos previos)
- **IA:** Gemini AI (análisis competitivo + análisis de mercado)
- **Destino:** Google Sheets (2 hojas)
- **Lógica:** JavaScript (Function nodes)
- **Nodos:** 22

## Lo que lo hace diferente

No solo scrapeae — compara contra la revisión anterior y reporta únicamente lo que cambió. Si no hay cambios de negocio relevantes, lo dice explícitamente en vez de generar ruido.
