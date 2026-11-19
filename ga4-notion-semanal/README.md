# GA4 → Notion (Métricas Web Semanal)

Extrae métricas semanales de Google Analytics 4 y las guarda automáticamente en tres bases de datos de Notion: resumen semanal, desglose por canal y acumulado mensual.

## Problema que resuelve

Revisar GA4 manualmente cada semana para copiar métricas a un doc de seguimiento es lento y propenso a errores. Este workflow lo hace solo, cada lunes a las 9:00, y detecta si la fila ya existe para actualizarla en vez de duplicarla.

## Cómo funciona

```
Trigger (lunes 9:00)
  → Calcular rango de fechas de la semana anterior
  → GA4: métricas generales (sesiones, usuarios, bounce rate, duración)
  → GA4: eventos (generate_lead, cta_contacto, whatsapp_click)
  → GA4: canales de tráfico (Orgánico, Directo, Social, Referral)
  → Merge y transformación de datos
  → Buscar si ya existe la semana en Notion
      ├── Sí → Actualizar entrada existente
      └── No → Crear nueva entrada
  → Guardar por canal (4 filas)
  → Acumular en tabla mensual (crear o sumar)
```

## Métricas que captura

**Resumen semanal:**
- Sesiones, usuarios nuevos
- Duración media (min), tasa de rebote (%)
- Demos solicitadas (generate_lead), CTA contacto, WhatsApp clicks
- Tasa de conversión (%)

**Por canal:** Orgánico, Directo, Social, Referral

**Acumulado mensual:** suma de todas las semanas del mes

## Stack
- **Trigger:** Schedule (lunes 9:00)
- **Fuente de datos:** Google Analytics 4 API
- **Destino:** Notion API (3 bases de datos)
- **Lógica:** JavaScript (nodos Code + Set)
- **Nodos:** 22

## Resultado

Cada lunes al arrancar el día, las métricas de la semana anterior ya están en Notion sin intervención manual.
