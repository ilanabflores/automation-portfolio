# CRM Automatizado — Culto Café

Pipeline de onboarding de cafeterías: detecta cambios de estado en PostgreSQL cada 15 minutos y dispara el email correspondiente a cada etapa. Ningún email se envía manualmente ni se duplica.

## El problema

Dar de alta una cafetería en Culto Café pasa por cuatro etapas: nuevo → contactada → verificada → activa. Sin automatización, alguien tiene que estar pendiente de cuándo cambia el estado de cada lead y enviar el email correcto a tiempo. Con volumen, eso es imposible de sostener manualmente.

## Cómo funciona

```
Trigger (cada 15 minutos)
  → PostgreSQL: buscar leads donde estado ≠ ultimo_estado_notificado
  → Por cada lead con cambio:
      ├── nuevo      → Email de bienvenida + confirmación de recepción
      ├── contactada → Email de seguimiento, invita a resolver dudas
      ├── verificada → Email "ya está verificada, falta el pago"
      └── activa     → Email de bienvenida oficial, ya aparecen en la web
  → PostgreSQL: actualizar ultimo_estado_notificado
     (garantiza que no se repite el mismo email si el workflow vuelve a correr)
```

## Emails del pipeline

| Estado | Qué comunica |
|---|---|
| nuevo | Recibimos tu solicitud, en breve te escribimos |
| contactada | Seguimos con el proceso, resuelve dudas respondiendo este correo |
| verificada | Ficha verificada, solo falta confirmar el pago |
| activa | Bienvenida oficial, ya aparecéis en la web y en el agente |

Todos los emails se personalizan con nombre del contacto y nombre del local.

## Por qué la lógica de `ultimo_estado_notificado` importa

El workflow corre cada 15 minutos. Sin esta columna, enviaría el mismo email decenas de veces. La columna actúa como memoria: solo hay acción cuando hay un estado nuevo que aún no se notificó.

## Stack
- **Trigger:** Schedule (cada 15 minutos)
- **Base de datos:** PostgreSQL (tabla `leads_cafeterias`)
- **Emails:** SMTP
- **Lógica:** IF nodes en cadena por estado
- **Nodos:** 10
