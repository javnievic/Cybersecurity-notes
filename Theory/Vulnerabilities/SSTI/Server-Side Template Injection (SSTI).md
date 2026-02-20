El **Server-Side Template Injection (SSTI)** es una vulnerabilidad que ocurre cuando una aplicación web permite que un atacante inserte código malicioso en una plantilla de renderizado en el servidor. Si el motor de plantillas del servidor procesa y evalúa entradas de usuario de manera insegura, un atacante puede inyectar código en esa plantilla y lograr ejecutar comandos o tomar control del servidor.

## Funcionamiento básico:

1. **Motor de plantillas**: La mayoría de las aplicaciones web utilizan motores de plantillas para generar HTML dinámico. Ejemplos populares son Jinja2 (Python), Twig (PHP), Pug (Node.js), etc.
    
2. **Inyección en la plantilla**: Si la entrada del usuario se inserta directamente en una plantilla y es evaluada sin una sanitización adecuada, un atacante puede inyectar expresiones específicas del motor de plantillas, que serán ejecutadas en el servidor.
    
3. **Impacto**: Dependiendo de la configuración del servidor y los privilegios, los ataques SSTI pueden permitir al atacante:
    
    - **Ejecutar código malicioso** en el servidor.
    - **Acceder a variables del entorno** o información sensible.
    - **Ejecutar comandos del sistema** si el lenguaje de plantillas lo permite.

## Ejemplo básico de SSTI:

Supongamos que una aplicación usa **Jinja2** (un motor de plantillas en Python) y la siguiente entrada de usuario es inyectada en la plantilla sin validación adecuada:

<h1>{{ nombre }}</h1>

Si un atacante introduce una expresión Jinja2 maliciosa como {{ 7 * 7 }}, y esta entrada no está debidamente sanitizada, el servidor evaluará el código y el resultado será 49.

En lugar de mostrar directamente la entrada del usuario, el servidor ejecuta la expresión y devuelve el resultado evaluado. Este comportamiento puede ser explotado.

### Casos más peligrosos:

Un atacante podría intentar inyectar código más avanzado, como acceso a funciones internas o ejecución de comandos del sistema, por ejemplo:

{{ config.items() }} <!-- Para leer variables de configuración -->

{{ self.**class**.**mro**[2].**subclasses**() }} <!-- Para obtener clases del sistema -->

O incluso, dependiendo del motor de plantillas y la configuración, ejecutar comandos del sistema:

{{ ''.**class**.**mro**[2].**subclasses**()59.communicate() }}

Este ejemplo intentaría ejecutar el comando `ls` en el servidor y mostrar el resultado.

### Qué se debe probar:

- **Prueba básica**: Intentar inyectar expresiones comunes del motor de plantillas. Prueba con expresiones matemáticas simples como:

`{{ 7*7 }} o {{ 1+1 }}`

`${7*7} o ${1+1} (si sospechas que es otro tipo de motor de plantillas).`

- **Sondeo del motor de plantillas**: Si obtienes alguna respuesta, identifica qué motor de plantillas está usando el servidor. Dependiendo del lenguaje backend (Python, PHP, Ruby...), podrías identificar el motor.
    
    - Python (Jinja2): {{ 1+1 }} o {{ ''.**class**.**mro**[2].**subclasses**() }}
    - PHP (Twig): {{ 1+1 }} o ${1+1}
    - Node.js (Pug): #{7*7}
- **Escalación**: Si logras ejecutar alguna expresión, prueba acceder a variables del sistema o ejecutar comandos. Para esto, necesitas entender el entorno de ejecución y el lenguaje usado en el backend.
    
- **Filtros y validaciones**: Observa si hay algún filtro o sanitización aplicada a las entradas. A veces, las plantillas están protegidas por algún tipo de validación que podría impedir la inyección.
    
- **Pruebas con diferentes formatos**: Si ves que el sitio no responde a inyecciones en el formato `{{ }}`, intenta con otras sintaxis (por ejemplo, `${ }` para otros lenguajes).


Cómo detectarlo? [GitHub - vladko312/SSTImap: Automatic SSTI detection tool with interactive interface](https://github.com/vladko312/SSTImap)

![[Capturas/Pasted image 20250115005412.png]]


Payloads: [PayloadsAllTheThings/Server Side Template Injection/README.md at master · swisskyrepo/PayloadsAllTheThings · GitHub](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md)
[Server Side Template Injection with Jinja2 - OnSecurity](https://onsecurity.io/article/server-side-template-injection-with-jinja2/)

It is possible to have access `request.application.__globals__.__builtins__`and execute some fuctions: 
- `open()`
- `print()`
- `eval()`
- `int()`
- `str()`
- `len()`

For example: `{{request.application.__globals__.__builtins__.__import__('os').popen('id').read()}}


Another commands: 
{{ ''.__class__.__mro__[1].__subclasses__() }}
{{ config.__class__.__mro__ }}
{{ request.application.__globals__ }}
{{ joiner.__globals__ }}


### Recomendaciones:

- Asegúrate de probar las inyecciones en diferentes campos o parámetros.
- Examina las respuestas del servidor con detalle para identificar posibles pistas sobre el motor de plantillas.
- Intenta identificar el lenguaje del backend para elegir el vector de inyección adecuado.
