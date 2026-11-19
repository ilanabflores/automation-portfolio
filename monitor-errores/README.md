# Monitor de Errores — Infraestructura n8n

Red de seguridad para toda la infraestructura de automatización: envía un aviso por email cuando cualquier workflow falla, con el nombre del workflow, el nodo donde ocurrió el error y el link directo a la ejecución en n8n.

## El problema

Los workflows automatizados pueden fallar silenciosamente: una API que cambia, un token que expira, un timeout puntual. Sin monitoreo activo, el error pasa desapercibido y los datos dejan de actualizarse sin que nadie lo sepa. Este workflow actúa como red de seguridad para todos los demás.

## Cómo funciona

```
Error Trigger (cualquier workflow falla)
  → Email con:
      - Nombre del workflow que falló
      - Nodo exacto donde ocurrió el error
      - Mensaje de error completo
      - Link directo a la ejecución para inspeccionar en un clic
```

## Por qué está en el portfolio

Es el workflow más simple del conjunto — 2 nodos — pero es el que hace que los otros sean fiables en producción. Sin él, un fallo en el análisis de competencia o en los reportes semanales podría pasar semanas sin detectarse.

## Stack
- **Trigger:** Error Trigger (n8n nativo — se activa por fallos en otros workflows)
- **Destino:** Email vía SMTP
- **Nodos:** 2
