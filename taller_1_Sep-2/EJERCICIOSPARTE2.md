# Guía Práctica de Prompt Engineering Avanzado (Ampliada) PARTE 2

## Sección 1: Fundamentos del Prompting Estructurado

### Ejercicio 1B: Cambiando el Rol a un Historiador

**Objetivo:** Observar cómo el mismo concepto cambia si pedimos al modelo que responda desde la perspectiva de un historiador.

```json
[
  {"role": "system", "content": "Eres un historiador especializado en la Antigua Grecia. Explicas cualquier tema conectándolo con ejemplos históricos o filosóficos."},
  {"role": "user", "content": "¿Qué es una variable?"}
]
```

**Reflexión:** ¿Cómo cambia la explicación respecto a la del ingeniero o el profesor de primaria? Esto muestra que el **mensaje de sistema** define el marco mental del LLM.

---

### Ejercicio 1C: El Humorista

```json
[
  {"role": "system", "content": "Eres un comediante de stand-up. Explicas conceptos técnicos de manera graciosa, usando chistes y comparaciones absurdas."},
  {"role": "user", "content": "¿Qué es una variable?"}
]
```

**Reflexión:** Aquí se evidencia que el *tono* también se puede controlar con prompts, lo cual es útil para adaptar la respuesta a distintas audiencias.

---

## Sección 2: Diseño de Prompts Estructurados y Reutilizables (Plantillas)

### Ejercicio 2B: Plantilla para Resumir Artículos Científicos

```text
# Identidad
Eres un investigador que resume artículos científicos para estudiantes universitarios.

# Instrucciones
- Haz un resumen de máximo 150 palabras.
- Usa un lenguaje accesible pero formal.
- Incluye: objetivo, método y conclusión principal.

# Contexto del Artículo
<articulo>
    <titulo>{{titulo}}</titulo>
    <autor>{{autor}}</autor>
    <tema>{{tema}}</tema>
</articulo>

Genera el resumen basado en el contexto.
```

**Ejemplo de uso:**

* Título: “Aplicaciones de la IA en medicina”
* Autor: “Dra. López”
* Tema: “Diagnóstico asistido por IA”

---

### Ejercicio 2C: Plantilla para Emails Profesionales

```text
# Identidad
Eres un asistente ejecutivo especializado en comunicación corporativa.

# Instrucciones
- Redacta un email breve, claro y profesional.
- Usa tono cordial pero directo.
- Incluye saludo, cuerpo y cierre con firma.

# Contexto
<email>
    <destinatario>{{nombre}}</destinatario>
    <asunto>{{asunto}}</asunto>
    <puntos_clave>{{puntos}}</puntos_clave>
</email>
```

Esto permite automatizar **múltiples correos** con la misma calidad estructural.

---

## Sección 3: Técnicas Avanzadas

### 3.1 Chain-of-Thought (CoT)

#### Ejercicio 3B: Problema Matemático

* **Prompt Directo:**

  ```
  ¿Cuál es la raíz cuadrada de 1764?
  ```
* **Prompt con CoT:**

  ```
  Calcula la raíz cuadrada de 1764. Piensa paso a paso, muestra tu razonamiento y luego la respuesta final.
  ```

Esto obliga al modelo a hacer la división en pasos (1764 ÷ 42 = 42), reduciendo errores.

---

#### Ejercicio 3C: Pregunta de Razonamiento

* **Directo:**

  ```
  María tiene el doble de edad que Juan. Juan tiene 12 años. ¿Qué edad tiene María?
  ```
* **Con CoT:**

  ```
  María tiene el doble de edad que Juan. Juan tiene 12 años. Razona paso a paso para calcular la edad de María y luego da la respuesta final.
  ```

---

### 3.2 ReAct

#### Ejercicio 4B: Simulación de búsqueda encadenada

**Pregunta:** “¿Cuál es la capital de Australia y en qué continente está?”

Prompt con ReAct:

```text
Thought: Necesito identificar la capital de Australia.  
Action: Search("capital de Australia")  
Observation: Canberra es la capital.  

Thought: Ahora debo identificar en qué continente está.  
Action: Search("Australia continente")  
Observation: Australia está en Oceanía.  

Final Answer: La capital de Australia es Canberra y el país se encuentra en Oceanía.
```

---

### 3.3 Self-Consistency

#### Ejercicio 5B: Pregunta Trampa de Probabilidad

“Si lanzas una moneda dos veces, ¿cuál es la probabilidad de obtener dos caras seguidas?”

* **Prompt:**

  ```
  Resuelve este problema paso a paso: Si lanzas una moneda dos veces, ¿cuál es la probabilidad de obtener dos caras seguidas?
  ```

Pide 3 cadenas de razonamiento independientes y compara:

* Algunos modelos dirán 1/2, otros 1/4.
* Con autoconsistencia, la mayoría coincidirá en 1/4 (correcto).

---

### 3.4 Tree-of-Thought (ToT)

#### Ejercicio 6B: Planificación de Viaje

Prompt ToT:

```
Quiero que explores 3 posibles planes de viaje a Europa con presupuesto limitado.  

Paso 1: Genera 3 opciones (países distintos).  
Paso 2: Evalúa cada opción en costo, cultura y accesibilidad.  
Paso 3: Elige la mejor y explica por qué.  
```

Esto obliga al modelo a **comparar alternativas** y no dar una única respuesta superficial.

---

### 3.5 JSON Enforced Output & Guardrails

#### Ejercicio 7B: Extracción de Contactos

Texto de entrada:
“Juan Pérez vive en Bogotá, Colombia. Su correo es [juanperez@mail.com](mailto:juanperez@mail.com) y su teléfono es +57 300 123 4567.”

Prompt:

```
Extrae la información y responde solo con un JSON válido en esta estructura:  

{
  "nombre": "string",
  "ciudad": "string",
  "pais": "string",
  "email": "string",
  "telefono": "string"
}
```

Esto asegura que la salida sea **estructurada y usable por un sistema**.

---


