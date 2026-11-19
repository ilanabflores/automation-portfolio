# Cold Email → Notion (Semanal)

Extrae métricas de campañas de cold email desde la API de Instantly cada semana y las consolida en Notion con vista por campaña y acumulado mensual. Elimina el trabajo manual de revisar cada campaña en la plataforma y centraliza el seguimiento donde trabaja el equipo.

## El problema

Medir el rendimiento de cold email requiere entrar a Instantly, revisar cada campaña por separado y copiar los números. Con varias campañas activas, eso es tiempo que se repite cada semana. Este workflow conecta directamente con la API de Instantly y registra todo en Notion sin intervención.

## Cómo funciona

```
Trigger (lunes 9:00)
  → Instantly API: analytics de la semana anterior
  → Filtrar solo campañas con emails enviados (> 0)
  → Calcular tasas: apertura, respuesta, respuesta positiva, rebote (%)
  → Por cada campaña activa:
      ├── Buscar si ya existe esa semana+campaña en Notion
      │     ├── Sí → Actualizar fila existente
      │     └── No → Crear nueva fila
      └── Sumar al acumulado mensual (crear mes o actualizar)
```

## Métricas que captura

**Por semana y campaña:** emails enviados, aperturas, respuestas, respuestas positivas, rebotes, oportunidades generadas — más las tasas calculadas en %

**Acumulado mensual:** suma de todas las semanas con tasas recalculadas sobre el total del mes

## Stack
- **Trigger:** Schedule (lunes 9:00)
- **Fuente:** Instantly API v2
- **Destino:** Notion API (2 bases de datos: semanal + mensual)
- **Lógica:** JavaScript (Code nodes + Set) — cálculo de tasas, lógica upsert
- **Nodos:** 14

## Resultado

Seguimiento completo de cold email en Notion, con histórico por semana y por mes. Las campañas con 0 envíos se filtran automáticamente para mantener los datos limpios.
