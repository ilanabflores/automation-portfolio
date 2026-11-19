# GSC → Notion (SEO Semanal)

Extrae métricas de posicionamiento orgánico de Google Search Console cada semana y las consolida en Notion: métricas globales del sitio, top 10 keywords por clicks y top 10 páginas por clicks.

## El problema

Hacer seguimiento de SEO implica entrar a Search Console cada semana, configurar el rango de fechas manualmente y anotar los datos en algún lugar. Con este workflow, el histórico se acumula solo en Notion cada lunes — y está disponible para revisión de equipo sin que nadie tenga que extraerlo.

## Cómo funciona

```
Trigger (lunes 9:00)
  → Calcular fechas de la semana anterior + número de semana ISO
  → 3 llamadas en paralelo a la Search Console API:
      ├── Métricas globales del sitio (clicks, impresiones, CTR, posición)
      ├── Top 10 queries por clicks
      └── Top 10 páginas por clicks
  → Formatear resultados
  → Guardar en 3 bases de datos de Notion
```

## Métricas que captura

**Global:** clicks totales, impresiones, CTR (%), posición media

**Top 10 Keywords:** query, clicks, impresiones, CTR (%), posición media

**Top 10 Páginas:** URL, clicks, impresiones, CTR (%), posición media

## Stack
- **Trigger:** Schedule (lunes 9:00)
- **Fuente:** Google Search Console API (OAuth2)
- **Destino:** Notion API (3 bases de datos)
- **Lógica:** JavaScript (Code nodes)
- **Nodos:** 11

## Resultado

Dashboard de SEO en Notion con histórico semanal acumulado. Las 3 vistas (global, keywords, páginas) se actualizan automáticamente cada lunes y están disponibles para el equipo sin acceso a Search Console.
