# Agente Conversacional — Culto Café

Chatbot con personalidad propia para recomendar cafeterías de especialidad en Malasaña, Madrid. Consulta una base de datos real en tiempo real y mantiene contexto de la conversación.

## Qué es

"Culto" es una taza de café azul con voz propia. El agente recibe la consulta del usuario (zona, plan: trabajar, quedar, catar), consulta la base de datos de cafeterías verificadas y devuelve recomendaciones personalizadas con datos reales: nivel de ruido, wifi, enchufes, terraza, pet-friendly, precio y links a Google Maps.

## Cómo funciona

```
Chat Trigger (interfaz pública embebible)
  → Agente LangChain (con system prompt de personalidad)
      ├── Memoria de conversación (ventana de 3 turnos)
      ├── Google Gemini (modelo de lenguaje)
      └── Tool: consulta base de datos en Supabase
              → Devuelve fichas reales de cafeterías
  → Respuesta con recomendaciones + links a Maps
```

## Características del agente

- **Personalidad definida:** tutea, frases cortas, tono urbano y cercano, nunca corporativo
- **Datos reales:** nunca inventa cafeterías — consulta Supabase antes de responder
- **Memoria:** recuerda el contexto dentro de la conversación (zona, gustos)
- **Lógica de ruido:** traduce el campo numérico `ruido_1a5` a lenguaje natural ("muy tranquilo, casi de biblioteca")
- **Links a Maps:** genera URLs de Google Maps con las coordenadas lat/lng de cada ficha
- **Límites claros:** si preguntan por una zona fuera de Malasaña, lo dice y ofrece alternativas

## Stack
- **Trigger:** Chat Trigger (interfaz pública embebible)
- **Agente:** n8n LangChain Agent
- **LLM:** Google Gemini
- **Memoria:** Buffer Window Memory (3 turnos)
- **Base de datos:** Supabase (tabla `cafeterias_para_agente`)
- **Nodos:** 5
