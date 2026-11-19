# Cold Email → Notion (Semanal)

Extrae métricas de campañas de cold email desde la API de Instantly y las consolida en Notion con vista semanal por campaña y acumulado mensual.

## Problema que resuelve

Medir el rendimiento de campañas de cold email requiere entrar a Instantly, revisar cada campaña manualmente y copiar los datos. Este workflow conecta directamente con la API de Instantly cada semana y registra todas las campañas activas en Notion automáticamente.

## Cómo funciona

```
Trigger (lunes 9:00)
  → Instantly API: analytics de la semana anterior
  → Filtrar solo campañas con emails enviados (> 0)
  → Calcular métricas (tasas en %)
  → Por cada campaña:
      ├── Buscar si ya existe esa semana+campaña en Notion
      │     ├── Sí → Actualizar
      │     └── No → Crear nueva fila
      └── Acumular en tabla mensual (sumar o crear)
```

## Métricas que captura

**Por semana y campaña:**
- Emails enviados, aperturas, respuestas, respuestas positivas, rebotes, oportunidades
- Tasa de apertura (%), tasa de respuesta (%), tasa de respuesta positiva (%), tasa de rebote (%)

**Acumulado mensual:** suma ponderada de todas las semanas, con tasas recalculadas sobre el total

## Stack
- **Trigger:** Schedule (lunes 9:00)
- **Fuente de datos:** Instantly API v2
- **Destino:** Notion API (2 bases de datos: semanal + mensual)
- **Lógica:** JavaScript (nodos Code + Set)
- **Nodos:** 14

## Resultado

Seguimiento completo de cold email en Notion con histórico por semana y por mes, sin entrar a Instantly.
