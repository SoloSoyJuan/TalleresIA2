# Resultados

Se realizan los ejercicios del taller 1 (Advanced prompt engineering)

## Parte 1:

Para la realización de este taller se utilizó como LLM de preferencia __Gemini 2.5 Flash__. En esta primera parte vamos a soliciatar al LLM que genere una función de Python que valide si una contraseña cumple con ciertos criterios de seguridad. En el primer ejercicio (1) se realizará con un *prompt simple* y en el segundo ejercicio (2) con un *prompt avanzado*. Finalmente, comparar los resultados con los criterios de evaluación.


### Ejercicio 1A

Este es el prompt simple utilizado:
```
Haz una función en Python para validar una contraseña.
```

Esta es la respuesta generada por __Gemini 2.5 Flash__ al utilizar el *prompt simple*:

---
#### Validación de contraseñas con Python
Aquí tienes una función en Python para validar contraseñas. Esta función te permite establecer varios criterios para una contraseña segura, como la longitud mínima y la inclusión de diferentes tipos de caracteres.

```
import re

def validar_contrasena(contrasena, min_longitud=8, requiere_mayuscula=True, requiere_minuscula=True, requiere_numero=True, requiere_simbolo=True):
    """
    Valida una contraseña según criterios específicos.

    Args:
        contrasena (str): La contraseña a validar.
        min_longitud (int): Longitud mínima requerida.
        requiere_mayuscula (bool): Si la contraseña debe contener al menos una letra mayúscula.
        requiere_minuscula (bool): Si la contraseña debe contener al menos una letra minúscula.
        requiere_numero (bool): Si la contraseña debe contener al menos un número.
        requiere_simbolo (bool): Si la contraseña debe contener al menos un símbolo.

    Returns:
        tuple: Una tupla con un booleano que indica si la contraseña es válida y una lista 
               de los errores encontrados.
    """
    errores = []

    # 1. Validar la longitud
    if len(contrasena) < min_longitud:
        errores.append(f"La contraseña debe tener al menos {min_longitud} caracteres.")

    # 2. Validar tipos de caracteres
    if requiere_mayuscula and not re.search(r'[A-Z]', contrasena):
        errores.append("La contraseña debe contener al menos una letra mayúscula.")
    
    if requiere_minuscula and not re.search(r'[a-z]', contrasena):
        errores.append("La contraseña debe contener al menos una letra minúscula.")
    
    if requiere_numero and not re.search(r'[0-9]', contrasena):
        errores.append("La contraseña debe contener al menos un número.")
        
    if requiere_simbolo and not re.search(r'[^A-Za-z0-9]', contrasena):
        errores.append("La contraseña debe contener al menos un símbolo (carácter especial).")
    
    es_valida = len(errores) == 0
    return es_valida, errores

# Ejemplo de uso
contrasena_prueba = "MiContrasena123!"
es_valida, errores = validar_contrasena(contrasena_prueba)

if es_valida:
    print("La contraseña es válida.")
else:
    print("La contraseña no es válida. Errores:")
    for error in errores:
        print(f"- {error}")

# Otro ejemplo con criterios diferentes
contrasena_prueba_2 = "corta"
es_valida_2, errores_2 = validar_contrasena(contrasena_prueba_2, min_longitud=10)

print("\n---")
if es_valida_2:
    print("La segunda contraseña es válida.")
else:
    print("La segunda contraseña no es válida. Errores:")
    for error in errores_2:
        print(f"- {error}")
```
#### Explicación del código
- Importamos re: Este módulo de Python es para expresiones regulares (regex). Las regex nos permiten buscar patrones de texto de manera muy eficiente. En este caso, las usamos para verificar si la contraseña contiene mayúsculas, minúsculas, números o símbolos.

- Definición de la función validar_contrasena:

    - contrasena: Es el parámetro principal, la cadena de texto que queremos validar.
    
    - Los siguientes parámetros (min_longitud, requiere_mayuscula, etc.) tienen valores por defecto, lo que te permite usarlos de forma opcional. Por ejemplo, si solo quieres verificar la longitud, puedes llamar a la función sin especificar los otros argumentos.

- Validación de la longitud: Se usa len() para verificar si la longitud de la contraseña cumple con el mínimo establecido. Si no es así, se agrega un mensaje de error a la lista errores.

- Validación de tipos de caracteres: Aquí es donde entran en juego las expresiones regulares (re.search).
    - r'[A-Z]': Busca cualquier carácter que sea una letra mayúscula de la 'A' a la 'Z'.

    - r'[a-z]': Busca cualquier letra minúscula.

    - r'[0-9]': Busca cualquier dígito.

    - r'[^A-Za-z0-9]': Este es un poco más complejo. El ^ dentro de los corchetes niega el conjunto de caracteres, lo que significa que busca cualquier carácter que no sea una letra (mayúscula o minúscula) ni un número. Esto nos sirve para identificar un símbolo.

- Retorno de valores: La función devuelve una tupla. El primer elemento es un booleano (True o False) que te dice si la contraseña es válida. El segundo elemento es una lista con todos los mensajes de error encontrados, lo cual es muy útil para darle una retroalimentación precisa al usuario.

Esta función es muy flexible y fácil de modificar. Si en el futuro quieres añadir más reglas de validación (por ejemplo, evitar espacios en blanco o la presencia de la palabra "password"), solo tienes que agregar más verificaciones a la función.

---

### Ejercicio 1B
Este es el prompt avanzado utilizado:
```
**Rol:** Eres un desarrollador de software senior especializado en ciberseguridad.

**Tarea:** Genera una función en Python llamada `validar_contrasena`.

**Contexto:** La función recibirá una cadena de texto (la contraseña) y debe retornar `True` si cumple con TODAS las siguientes reglas, o `False` si falla al menos una.

**Reglas de Validación (Criterios):**
1.  Debe tener una longitud mínima de 12 caracteres.
2.  Debe contener al menos una letra mayúscula (A-Z).
3.  Debe contener al menos una letra minúscula (a-z).
4.  Debe contener al menos un número (0-9).
5.  Debe contener al menos un carácter especial (ej: @, #, $, %, &, !).

**Formato de Salida:**
-   El bloque de código de la función en Python.
-   Debe incluir docstrings que expliquen qué hace la función, sus parámetros y qué retorna.
-   Añade 3 ejemplos de uso: uno con una contraseña válida y dos con contraseñas inválidas que demuestren diferentes fallos.

**Restricciones:**
-   No uses librerías externas, solo el core de Python.
-   El código debe ser legible y seguir las convenciones de estilo de PEP 8.
```

Esta es la respuesta generada por __Gemini 2.5 Flash__ al utilizar el *prompt avanzado*:

---

```
def validar_contrasena(contrasena: str) -> bool:
    """
    Valida una contraseña basándose en un conjunto de criterios de seguridad.

    Args:
        contrasena: La cadena de texto que representa la contraseña a validar.

    Returns:
        True si la contraseña cumple con todas las reglas de validación,
        False en caso contrario.
    """
    # Regla 1: Longitud mínima de 12 caracteres.
    if len(contrasena) < 12:
        return False

    # Reglas 2-5: Contiene al menos una mayúscula, minúscula, número y carácter especial.
    has_mayuscula = any(c.isupper() for c in contrasena)
    has_minuscula = any(c.islower() for c in contrasena)
    has_numero = any(c.isdigit() for c in contrasena)
    has_especial = any(not c.isalnum() for c in contrasena)

    return all([has_mayuscula, has_minuscula, has_numero, has_especial])

# --- Ejemplos de uso ---
# Contraseña válida
contrasena_valida = "MiContraseñaSegura123!"
print(f'"{contrasena_valida}" es válida? {validar_contrasena(contrasena_valida)}')

# Contraseña inválida: No tiene la longitud mínima.
contrasena_corta = "Corta12#"
print(f'"{contrasena_corta}" es válida? {validar_contrasena(contrasena_corta)}')

# Contraseña inválida: No tiene un carácter especial.
contrasena_sin_especial = "MiContrasenaSinEspecial1"
print(f'"{contrasena_sin_especial}" es válida? {validar_contrasena(contrasena_sin_especial)}')
```

---

## Criterios de evaluación Ejercicio 1:

- Completitud: ¿La función del prompt simple incluyó todas las reglas?
- Claridad: ¿El código está bien documentado? ¿Es fácil de entender?
- Utilidad: ¿El prompt avanzado proporcionó ejemplos de uso que facilitan la prueba y la integración?
- Robustez: ¿La función maneja casos borde que el prompt simple podría haber ignorado?

---

## Parte 2:

En esta segunda parte vamos a soliciatar al LLM que  Extraer datos específicos de un texto no estructurado y presentarlos en formato JSON. En el primer ejercicio (A) se realizará con un *prompt simple* y en el segundo ejercicio (B) con un *prompt avanzado*. Finalmente, comparar los resultados con los criterios de evaluación.

### Ejercicio 2A


```
Saca los datos importantes de este texto en JSON:

"La reunión de lanzamiento del Proyecto Fénix será el 15 de octubre de 2024 a las 10:30 AM en la Sala Júpiter. Los asistentes confirmados son Ana García (Líder de Proyecto), Carlos Rodríguez (Desarrollador Principal) y Elena Fernández (Diseñadora UX). El presupuesto asignado es de $25,000 USD."
```

### Ejercicio 2B

```
**Rol:** Eres un asistente de procesamiento de datos altamente preciso.

**Tarea:** Extrae la información clave del siguiente texto y formatéala estrictamente como un objeto JSON.

**Texto de Entrada:**
La reunión de lanzamiento del Proyecto Fénix será el 15 de octubre de 2024 a las 10:30 AM en la Sala Júpiter. Los asistentes confirmados son Ana García (Líder de Proyecto), Carlos Rodríguez (Desarrollador Principal) y Elena Fernández (Diseñadora UX). El presupuesto asignado es de $25,000 USD.


**Esquema JSON de Salida (Schema):**
Debes seguir esta estructura exacta. Si un campo no se encuentra en el texto, déjalo como `null`.

```json
{
  "proyecto": {
    "nombre": "string",
    "presupuesto": {
      "monto": "number",
      "moneda": "string"
    }
  },
  "reunion": {
    "fecha": "string (formato YYYY-MM-DD)",
    "hora": "string (formato HH:MM)",
    "ubicacion": "string"
  },
  "asistentes": [
    {
      "nombre": "string",
      "rol": "string"
    }
  ]
}

Restricciones:

- El monto del presupuesto debe ser un número, no un string.
- La fecha y hora deben ser normalizadas a los formatos especificados.
- No incluyas campos que no estén en el esquema.
```

## Criterios de evaluación Ejercicio 2:
- Completitud: ¿La función del prompt simple incluyó todas las reglas?
- Claridad: ¿El código está bien documentado? ¿Es fácil de entender?
- Utilidad: ¿El prompt avanzado proporcionó ejemplos de uso que facilitan la prueba y la integración?
- Robustez: ¿La función maneja casos borde que el prompt simple podría haber ignorado?

---