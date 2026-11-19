# Agente Conversacional — Culto Café

Asistente de recomendaciones de cafeterías para Culto Café, un directorio de cafeterías de especialidad en Malasaña, Madrid. El agente tiene personalidad propia, consulta datos reales en Supabase y mantiene contexto durante la conversación.

## Qué es

"Culto" es una taza de café azul con voz propia. Cuando un usuario le dice zona y plan (trabajar, quedar, catar), consulta la base de datos de cafeterías verificadas y devuelve recomendaciones personalizadas con datos reales: nivel de ruido, wifi, enchufes, terraza, pet-friendly, precio y links a Google Maps.

El agente está embebido en la web de Culto Café como chat público.

## Cómo funciona

```
Chat Trigger (interfaz pública embebible)
  → Agente LangChain
      ├── Sistema de personalidad (tutea, frases cortas, tono urbano)
      ├── Memoria de conversación (ventana de 3 turnos)
      ├── Google Gemini (modelo de lenguaje)
      └── Tool: consulta base de datos en Supabase
              → Tabla cafeterias_para_agente
              → Devuelve fichas reales con todos los atributos
  → Respuesta con recomendaciones + links a Google Maps
```

## Decisiones de diseño que marcan la diferencia

- **Nunca inventa datos:** el agente no responde hasta consultar Supabase. Si algo no está en la ficha, no lo menciona.
- **Ruido traducido a lenguaje natural:** el campo `ruido_1a5` se convierte en "muy tranquilo, casi de biblioteca" o "animado, con bullicio" — no se expone el número.
- **Links a Maps generados dinámicamente** con las coordenadas lat/lng de cada cafetería.
- **Límites explícitos:** si preguntan por una zona fuera de Malasaña, lo dice y ofrece alternativas dentro de la zona. No intenta cubrir lo que no tiene datos.
- **Personalidad definida y consistente:** el system prompt tiene reglas claras sobre cuándo mencionar personajes secundarios, cómo abrir y cerrar conversaciones y cómo variar los saludos.

## Stack
- **Trigger:** Chat Trigger (interfaz pública embebible)
- **Agente:** n8n LangChain Agent
- **LLM:** Google Gemini
- **Memoria:** Buffer Window Memory (3 turnos)
- **Base de datos:** Supabase (tabla `cafeterias_para_agente`)
- **Nodos:** 5
