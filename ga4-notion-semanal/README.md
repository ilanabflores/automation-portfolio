# GA4 → Notion (Métricas Web Semanal)

Extrae métricas semanales de Google Analytics 4 y las guarda automáticamente en Notion cada lunes. Las métricas están disponibles para revisión con jefatura sin que nadie tenga que entrar a GA4 ni copiar datos manualmente.

## El problema

Revisar GA4 cada semana para copiar métricas a un doc de seguimiento es lento, propenso a errores y fácil de olvidar. Este workflow lo hace solo, cada lunes a las 9:00, y detecta si la fila ya existe para actualizarla en vez de duplicarla — así el histórico se mantiene limpio sin intervención.

## Cómo funciona

```
Trigger (lunes 9:00)
  → Calcular rango de fechas de la semana anterior
  → 3 llamadas a GA4 en paralelo:
      ├── Métricas generales (sesiones, usuarios, bounce rate, duración)
      ├── Eventos de conversión (leads, CTA, WhatsApp clicks)
      └── Desglose por canal de tráfico
  → Merge y transformación de datos
  → Buscar si ya existe esa semana en Notion
      ├── Sí → Actualizar entrada existente
      └── No → Crear nueva entrada
  → Guardar filas por canal (Orgánico, Directo, Social, Referral)
  → Sumar al acumulado mensual
```

## Métricas que captura

**Resumen semanal:** sesiones, usuarios nuevos, duración media (min), tasa de rebote (%), leads generados, CTA contacto clicks, WhatsApp clicks, tasa de conversión (%)

**Por canal:** Orgánico, Directo, Social, Referral — con sesiones, usuarios y conversiones

**Acumulado mensual:** suma de todas las semanas del mes, recalculada cada lunes

## Stack
- **Trigger:** Schedule (lunes 9:00)
- **Fuente:** Google Analytics 4 API (vía credencial de servicio)
- **Destino:** Notion API (3 bases de datos)
- **Lógica:** JavaScript (Code nodes + Set)
- **Nodos:** 22

## Resultado

Cada lunes, las métricas de la semana anterior ya están en Notion al empezar el día — listas para revisión de equipo sin ningún paso manual.
