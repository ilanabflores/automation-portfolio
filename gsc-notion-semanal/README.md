# GSC → Notion (SEO Semanal)

Extrae métricas de posicionamiento orgánico de Google Search Console y las guarda en tres bases de datos de Notion: métricas globales, top 10 keywords y top 10 páginas.

## Problema que resuelve

Hacer seguimiento de SEO implica entrar a Search Console cada semana, mirar las mismas métricas y anotarlas en algún lado. Este workflow lo automatiza: cada lunes extrae la semana anterior y la registra en Notion, listo para revisar.

## Cómo funciona

```
Trigger (lunes 9:00)
  → Calcular fechas de la semana anterior + número de semana ISO
  → 3 llamadas en paralelo a la Search Console API:
      ├── Métricas globales del sitio
      ├── Top 10 queries por clicks
      └── Top 10 páginas por clicks
  → Formatear cada resultado
  → Guardar en 3 bases de datos de Notion
```

## Métricas que captura

**Global:**
- Clicks totales, impresiones, CTR (%), posición media

**Top 10 Keywords:**
- Query, clicks, impresiones, CTR (%), posición media

**Top 10 Páginas:**
- URL, clicks, impresiones, CTR (%), posición media

## Stack
- **Trigger:** Schedule (lunes 9:00)
- **Fuente de datos:** Google Search Console API (vía HTTP Request + OAuth2)
- **Destino:** Notion API (3 bases de datos)
- **Lógica:** JavaScript (nodos Code)
- **Nodos:** 11

## Resultado

Dashboard de SEO en Notion actualizado automáticamente cada semana, con histórico acumulado por número de semana ISO.
