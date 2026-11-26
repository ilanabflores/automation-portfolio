# CRM Automatizado — Culto Café

Pipeline de onboarding de cafeterías: cuando el estado de un lead cambia, un webhook dispara en tiempo real el email correspondiente a cada etapa. Ningún email se envía manualmente ni se duplica.

## El problema

Dar de alta una cafetería en Culto Café pasa por cuatro etapas: nuevo → contactada → verificada → activa. Sin automatización, alguien tiene que estar pendiente de cuándo cambia el estado de cada lead y enviar el email correcto a tiempo. Con volumen, eso es imposible de sostener manualmente.

## Cómo funciona

```
Webhook POST /culto-cafe-crm
  (se dispara cuando un sistema externo cambia el estado de un lead)
  → Recibe: id, estado, email, contacto, nombre
  → Evalúa el estado del lead:
      ├── nuevo      → Email de bienvenida + confirmación de recepción
      ├── contactada → Email de seguimiento, invita a resolver dudas
      ├── verificada → Email "ya está verificada, falta el pago"
      └── activa     → Email de bienvenida oficial, ya aparecen en la web
  → PostgreSQL: actualizar ultimo_estado_notificado
  → Responde { "ok": true } al sistema llamante
```

## Por qué webhook en lugar de polling

La versión original consultaba PostgreSQL cada 15 minutos para detectar cambios. La versión actual usa un webhook: el sistema externo avisa a n8n en el momento exacto en que cambia el estado, sin esperas ni consultas innecesarias. El email llega en segundos, no en hasta 15 minutos.

## Emails del pipeline

| Estado | Qué comunica |
|---|---|
| nuevo | Recibimos tu solicitud, en breve te escribimos |
| contactada | Seguimos con el proceso, resuelve dudas respondiendo este correo |
| verificada | Ficha verificada, solo falta confirmar el pago |
| activa | Bienvenida oficial, ya aparecéis en la web y en el agente |

Todos los emails se personalizan con nombre del contacto y nombre del local.

## Por qué la lógica de `ultimo_estado_notificado` importa

Aunque el webhook solo se dispara cuando hay un cambio, la columna actúa como garantía: si el webhook se llama dos veces con el mismo estado, el email no se duplica.

## Stack
- **Trigger:** Webhook (POST en tiempo real)
- **Base de datos:** PostgreSQL (tabla `leads_cafeterias`)
- **Emails:** SMTP
- **Lógica:** IF nodes en cadena por estado
- **Nodos:** 11
