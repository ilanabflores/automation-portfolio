# Análisis de Competencia con IA

Monitoreo automatizado de 10 competidores directos cada 14 días: scraping de sus páginas web, análisis con Gemini AI y detección de cambios reales frente al ciclo anterior. Los resultados alimentan el workflow de informe ejecutivo que se envía a jefatura.

## El problema

Revisar manualmente 10 competidores — página por página, funcionalidades, precios, mensajes — más 5 fuentes de mercado y redactar un resumen estructurado con hallazgos y recomendaciones toma entre 6 y 8 horas por ciclo. Sin automatización, ese tiempo compite con otras prioridades y el monitoreo se vuelve irregular. Con este workflow, el ciclo completo corre solo cada 14 días sin intervención humana.

## Qué detectó en producción

- **Lanzamientos de nuevas funcionalidades** de competidores antes de que fueran anunciados públicamente
- **Cambios de precios y planes** con comparación directa frente al ciclo anterior
- **Tendencias del mercado** (a través de 5 fuentes especializadas) que informaron decisiones de roadmap de producto

Los hallazgos se entregan a jefatura en cada ciclo como parte del proceso de inteligencia competitiva del equipo.

## Cómo funciona

```
Trigger (diario 10:00 — ejecuta el análisis solo cada ≥14 días)
  → Leer lista de competidores + URLs desde Google Sheets
  → Leer hallazgos del ciclo anterior (para comparar)
  → Por cada fuente (10 competidores + 5 fuentes de mercado):
      → HTTP Request: scraping de la URL
      → Code: limpiar HTML, extraer texto relevante
      → Gemini AI: ¿qué cambió respecto al ciclo anterior?
          ├── Competidor → análisis de producto, precios, comunicación
          └── Mercado → tendencias del sector, tamaño, crecimiento
      → Clasificar nivel de amenaza: Crítica / Alta / Media / Baja
      → Guardar en Google Sheets (Hallazgos o Mercado)
  → Registrar fecha del ciclo completado
  → Trigger: lanzar workflow de informe automático
```

## Por qué solo reporta cambios reales

Gemini tiene instrucciones explícitas para no reportar una fuente si no cambió nada desde el ciclo anterior. Esto evita el ruido de recibir "sin novedades" en cada fila y mantiene los datos limpios con solo lo que requiere atención.

## Output generado

Cada fila en Google Sheets incluye: tipo de cambio, funcionalidades detectadas, precio publicado, hallazgo concreto, por qué importa para el negocio, acción recomendada (Ignorar / Investigar / Comunicar / Valorar para producto / Trasladar a ventas) y nivel de amenaza.

## Stack
- **Trigger:** Schedule diario + lógica de ciclos (≥14 días desde el último)
- **Scraping:** HTTP Request (una petición por URL de fuente)
- **IA:** Google Gemini (via HTTP Request)
- **Almacenamiento:** Google Sheets (hojas Hallazgos y Mercado)
- **Lógica:** JavaScript (Code nodes) — parseo de HTML, comparación entre ciclos, clasificación
- **Nodos:** 22
