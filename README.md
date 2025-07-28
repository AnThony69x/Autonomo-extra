
# 🤖 Módulo de Agente Autónomo con Flujos - Análisis de Texto con Gemini AI

Este módulo es una expansión del sistema `ExposIA` desarrollada con NestJS. Implementa un agente inteligente autónomo que orquesta un flujo automático de análisis de texto, utilizando **Gemini (Google Generative AI)**, y persiste los resultados en un endpoint externo.

---

## 🚀 Funcionalidad

Este módulo ejecuta un flujo en 3 pasos automáticos:

1. **Obtención de texto** desde un endpoint externo (mock server).
2. **Análisis del texto** usando un modelo LLM (Gemini API).
3. **Persistencia de resultados** en un segundo endpoint vía POST.

---

## 🧾 Justificación del Uso de Mock Server como Tecnología de Flujo

Para cumplir con el requerimiento de utilizar una **tecnología externa de flujo**, se empleó **Postman Mock Server**. Esta herramienta permite simular de forma realista el comportamiento de endpoints externos de otros microservicios del grupo, sin depender de que estén en ejecución.

Con esto se logra:

- Orquestar un flujo **totalmente automatizado y externo** al módulo.
- **Separar responsabilidades** entre microservicios de forma limpia.
- Simular condiciones reales de producción sin necesidad de dependencias activas.
- Cumplir con el requerimiento de consumir **otro microservicio del grupo**, usando URLs públicas.

---

## 🔧 Tecnologías Utilizadas

- 🧠 Google Generative AI (Gemini API)
- 🌐 NestJS (TypeScript)
- 🧪 Postman (Mock Server para endpoints simulados)
- ⚙️ Axios / HttpService para integración HTTP
- 📦 dotenv / ConfigService para variables de entorno

---

## 🧱 Estructura del Proyecto

```
src/
├── controllers/
│   └── conversacion-ia.controller.ts
├── services/
│   └── conversacion-ia.service.ts
├── dtos/
│   └── consulta.dto.ts
├── modules/
│   └── conversacion-ia.module.ts
```

---

## 📌 Endpoints

### 1. `POST /v1/start-analysis-agent`
Activa el agente autónomo. El flujo realiza:

- GET a `/consulta-texto` (mock server)
- Análisis del contenido con Gemini
- POST a `/guardar-respuesta` (mock server)

**Respuesta esperada:**
```json
{
  "fuente_texto": "https://mockserver.com/consulta-texto",
  "pregunta_enviada_a_ia": "Texto recibido...",
  "respuesta_ia": "Respuesta generada por Gemini...",
  "estado": "ok"
}
```

---

### 2. `POST /v1/consultar-prueba-ia`
Permite probar directamente el análisis de texto enviando un cuerpo manual:

**Body:**
```json
{
  "texto": "Texto a evaluar por la IA"
}
```

**Respuesta:**
```json
{
  "pregunta": "Texto a evaluar por la IA",
  "respuesta_ia": "Evaluación detallada de Gemini",
  "estado": "ok"
}
```

---

## ⚙️ Requisitos de Entorno (`.env`)

```env
GEMINI_API_KEY=tu_api_key_de_google
GEMINI_MODEL=models/gemini-pro
PROMPT_REVISION_BASE=Evalúa el siguiente texto motivacional según criterios académicos y da sugerencias de mejora.
```

---

## 📦 Instalación y Ejecución

1. Instalar dependencias:
```bash
npm install
```

2. Configurar `.env` con tu API Key de Google AI

3. Ejecutar el servidor:
```bash
npm run start:dev
```

---

## 🧪 Simulación de Microservicios con Mock Server (Postman)

- **GET** `https://<mock>.mock.pstmn.io/consulta-texto`  
Devuelve un texto motivacional.

- **POST** `https://<mock>.mock.pstmn.io/guardar-respuesta`  
Recibe los datos generados por el LLM.

---

## 📊 Ejemplo del Flujo del Agente

```mermaid
graph TD
    A[POST /start-analysis-agent] --> B[GET /consulta-texto (Mock)]
    B --> C[Procesamiento Gemini API]
    C --> D[POST /guardar-respuesta (Mock)]
    D --> E[Respuesta Final al Cliente]
```

