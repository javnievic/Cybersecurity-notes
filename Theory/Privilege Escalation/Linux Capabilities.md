## What Are Linux Capabilities?

**Linux Capabilities** split the privileges of the superuser (root) into smaller units called **capabilities**, which can be assigned to individual binaries. This allows certain programs to perform privileged operations without needing to run as root.

### Examples of Common Capabilities

- **`CAP_NET_RAW`**: Allows sending and receiving raw network packets.
    
- **`CAP_SYS_ADMIN`**: Provides access to a wide range of administrative operations.
    
- **`CAP_SETUID`**: Allows changing the effective UID of a process.
    

---

## Identifying Binaries with Capabilities

To search for binaries on the system that have capabilities assigned, use the following command:

getcap -r / 2>/dev/null

**Example Output:**

- `/usr/bin/python3.8 = cap_setuid+ep`
    
- `/usr/bin/ping = cap_net_raw+ep`
    
- **`cap_setuid+ep`**: The binary can change its effective UID.
    
- **`cap_net_raw+ep`**: The binary can send raw network packets without restrictions.
    

---

## Privilege Escalation with Capabilities

If a binary has misconfigured capabilities, it can be exploited to gain elevated privileges. Below are some examples:

### 1. CAP_SETUID

A binary with `cap_setuid+ep` can change its UID. If it is configured to set UID 0 (root), you may obtain a privileged shell.

**Example:**  
If you find `/usr/bin/python3.8` with `cap_setuid+ep`:

python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'

This will give you a root shell.

---

### 2. CAP_NET_RAW

This capability allows sending raw network packets, which can be used to run tools like `ping` with special privileges.

**Example:**  
A binary with `cap_net_raw+ep` may allow commands such as:

ping -c 1 127.0.0.1

---

### 3. CAP_SYS_ADMIN

This capability grants access to a broad range of administrative operations, which may include mounting file systems or accessing devices.

---

## Mitigations

1. Regularly review binaries with assigned capabilities using `getcap`.
    
2. Restrict capabilities only to binaries that truly require them.
    
3. Use tools such as **LinPEAS** to audit insecure configurations.


## ESPAÑOL - ¿Qué son las Linux Capabilities?
Las **Linux Capabilities** dividen los privilegios de superusuario (root) en unidades más pequeñas, llamadas **capacidades**, que pueden asignarse a binarios individuales. Esto permite que ciertos programas ejecuten tareas privilegiadas sin necesidad de ejecutarse como root.

### Ejemplos de Capacidades Comunes
- **`CAP_NET_RAW`**: Permite enviar y recibir paquetes de red en bruto.
- **`CAP_SYS_ADMIN`**: Proporciona acceso a una amplia gama de operaciones administrativas.
- **`CAP_SETUID`**: Permite cambiar el UID efectivo de un proceso.

---

## Identificar Binarios con Capacidades
Para buscar binarios en el sistema que tengan capacidades asignadas, usa el comando:

getcap -r / 2>/dev/null

**Ejemplo de Salida:**
- `/usr/bin/python3.8 = cap_setuid+ep`
- `/usr/bin/ping = cap_net_raw+ep`

- **`cap_setuid+ep`**: El binario puede cambiar su UID efectivo.
- **`cap_net_raw+ep`**: El binario puede enviar paquetes de red sin restricciones.

---

## Escalada de Privilegios con Capacidades
Si un binario tiene capacidades mal configuradas, puede explotarse para obtener privilegios elevados. Aquí algunos ejemplos:

### 1. CAP_SETUID
Un binario con `cap_setuid+ep` puede cambiar su UID. Si se configura para usar el UID 0 (root), puedes obtener una shell privilegiada.

**Ejemplo**:  
Si encuentras `/usr/bin/python3.8` con `cap_setuid+ep`:

python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'

Esto te dará una shell como root.

---

### 2. CAP_NET_RAW
Permite enviar paquetes de red en bruto, lo que puede ser usado para ejecutar herramientas como `ping` con privilegios especiales.

**Ejemplo**:  
Un binario con `cap_net_raw+ep` puede permitir la ejecución de comandos como:

ping -c 1 127.0.0.1

---

### 3. CAP_SYS_ADMIN
Esta capacidad proporciona acceso a una amplia gama de operaciones administrativas, lo que puede incluir montaje de sistemas de archivos o acceso a dispositivos.

---

## Mitigaciones
1. Revisa regularmente los binarios con capacidades asignadas usando `getcap`.
2. Limita las capacidades solo a los binarios que realmente las necesitan.
3. Usa herramientas como **LinPEAS** para auditar configuraciones inseguras.
