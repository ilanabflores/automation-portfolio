# Monitor de Errores — Workflows n8n

Workflow de infraestructura que envía un aviso por email cuando cualquier otro workflow de n8n falla, con el nombre del workflow, el nodo donde falló y el link directo a la ejecución.

## Problema que resuelve

Los workflows automatizados pueden fallar silenciosamente: una API que cambia, un token que expira, un timeout. Sin monitoreo, el error puede pasar desapercibido días. Este workflow actúa como red de seguridad para toda la infraestructura de automatización.

## Cómo funciona

```
Error Trigger (cualquier workflow falla)
  → Enviar email con:
      - Nombre del workflow que falló
      - Nodo donde ocurrió el error
      - Mensaje de error
      - Link directo a la ejecución en n8n
```

## Stack
- **Trigger:** Error Trigger (n8n nativo)
- **Destino:** Email vía SMTP
- **Nodos:** 2

## Resultado

Aviso inmediato cuando algo falla, con toda la información para diagnosticarlo en un clic.
