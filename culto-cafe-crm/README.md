# CRM Automatizado — Culto Café

Automatización del pipeline de onboarding de cafeterías: detecta cambios de estado en la base de datos cada 15 minutos y dispara emails personalizados según la etapa del lead.

## Problema que resuelve

El proceso de dar de alta una cafetería en Culto Café pasa por varias etapas (nuevo → contactada → verificada → activa). Sin automatización, habría que enviar cada email manualmente al detectar el cambio de estado. Este workflow lo hace solo.

## Cómo funciona

```
Trigger (cada 15 minutos)
  → PostgreSQL: consulta leads cuyo estado cambió
     (estado distinto de ultimo_estado_notificado)
  → Por cada lead con cambio:
      ├── Estado = nuevo      → Email de bienvenida
      ├── Estado = contactada → Email de seguimiento
      ├── Estado = verificada → Email "ficha verificada"
      └── Estado = activa     → Email de bienvenida oficial
  → PostgreSQL: actualizar ultimo_estado_notificado
     (evita enviar el mismo email dos veces)
```

## Emails del flujo

| Estado | Asunto | Contenido |
|---|---|---|
| nuevo | "Ya tenemos tu solicitud" | Confirmación de recepción, explica el proceso de revisión |
| contactada | "Seguimos con lo tuyo" | Seguimiento, invita a resolver dudas |
| verificada | "Tu ficha ya está verificada" | Avisa que falta solo el paso de pago |
| activa | "¡Ya eres parte de Culto Café!" | Bienvenida oficial, confirma que aparecen en la web |

Todos los emails están personalizados con nombre del contacto y nombre del local.

## Stack
- **Trigger:** Schedule (cada 15 minutos)
- **Base de datos:** PostgreSQL (tabla `leads_cafeterias`)
- **Emails:** SMTP
- **Lógica:** IF nodes en cadena por estado
- **Nodos:** 10
