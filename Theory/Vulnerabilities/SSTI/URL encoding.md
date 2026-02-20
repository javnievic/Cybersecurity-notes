El **URL encoding**, también conocido como **percent encoding**, es una técnica utilizada para codificar caracteres especiales dentro de una URL (Uniform Resource Locator) de forma que sean compatibles con los estándares del protocolo HTTP. Algunas secuencias de caracteres tienen un significado especial en una URL y podrían causar problemas si no se codifican correctamente. Por ejemplo, espacios, símbolos como ?, &, %, o caracteres no imprimibles pueden interferir en la interpretación de la URL por el servidor.

### ¿Por qué es necesario hacer URL encoding?

1. **Carácter reservado**: Algunos caracteres tienen un significado específico en las URL. Por ejemplo:
   - El carácter & se utiliza para separar los parámetros en una consulta.
   - El carácter = se usa para asignar valores en pares clave-valor.
   - El carácter ? se utiliza para separar la parte de la URL que define el recurso y los parámetros de consulta.

   Si no se codifican correctamente, estos caracteres pueden hacer que la URL sea interpretada de manera incorrecta por el servidor.

2. **Carácter no permitido**: Algunos caracteres no son permitidos directamente en las URL según el estándar. Por ejemplo, los espacios deben ser codificados como %20, ya que las URLs no aceptan espacios directamente.

3. **Proteger la integridad de los datos**: Codificar ciertos caracteres garantiza que los datos enviados a través de una URL no sean modificados accidentalmente o malinterpretados. Esto es especialmente importante cuando se inyectan caracteres como {, }, $, o cuando se manipulan lenguajes de plantilla como en una vulnerabilidad de SSTI (Server-Side Template Injection).

### ¿Qué es el URL encoding?

El **URL encoding** convierte caracteres especiales en una representación que el navegador y el servidor pueden entender y procesar correctamente. Se realiza reemplazando cada carácter especial con un símbolo de porcentaje (%) seguido por el código hexadecimal que representa al carácter en la tabla ASCII. Por ejemplo:

- Un espacio ( ) se codifica como %20.
- El carácter % se codifica como %25.
- El símbolo $ se codifica como %24.
- El símbolo * se codifica como %2A.

### Ejemplo de URL encoding

Supongamos que quieres enviar el siguiente string como parte de una URL: 

Hello World! `$7*7`

La URL resultante, después de codificarla, sería:

Hello%20World%21%20%247%2A7

Aquí:
- El espacio ( ) es %20.
- El símbolo ! es %21.
- El símbolo $ es %24.
- El asterisco * es %2A.

### ¿Por qué se usa en vulnerabilidades como SSTI?

En el caso de la vulnerabilidad **SSTI** (Server-Side Template Injection), la codificación URL es importante porque necesitamos inyectar caracteres que tienen un significado especial en las plantillas de servidor (como {, }, $, etc.), pero que también son caracteres reservados en las URL. Si no los codificamos, el servidor HTTP podría interpretar estos caracteres de forma incorrecta o no enviarlos adecuadamente al código vulnerable.

Por ejemplo, para inyectar ${7*7} en una URL, necesitas codificar los caracteres especiales:

- ${ se convierte en %24%7B
- * se convierte en %2A
- } se convierte en %7D

La URL codificada sería:

%24%7B7%2A7%7D

Este es el motivo por el cual es necesario hacer **URL encoding**: permite enviar datos que, de otro modo, serían interpretados de manera incorrecta o causarían errores en la comunicación con el servidor.
