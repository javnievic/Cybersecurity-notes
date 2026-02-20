# Most Important Nmap Parameters

## Basic Scanning
- `-sS`: **TCP SYN Scan**  
  Performs a fast and stealthy scan by sending SYN packets. Ideal for discovering open ports without completing full connections.

- `-sT`: **TCP Connect Scan**  
  A basic scan that uses a full three-way handshake. Less stealthy.

- `-sU`: **UDP Scan**  
  Scans for open UDP ports. Useful for services like DNS, SNMP, etc.

## Host Discovery
- `-sn`: **Ping Scan**  
  Determines which hosts are online without performing a port scan.

- `-Pn`: **No Ping**  
  Scans directly without checking if the host is alive. Useful in networks where ICMP is blocked.

- `-PS`: **TCP SYN Ping**  
  Sends SYN packets to specific ports to identify active hosts.

- `-PU`: **UDP Ping**  
  Sends UDP packets to discover active hosts.

## Advanced Scanning Options
- `-A`: **Aggressive Scan**  
  Combines OS detection, service detection, and scripts. Very complete but slower.

- `-sV`: **Service Version Detection**  
  Identifies service versions on open ports.

- `-O`: **Operating System Detection**  
  Attempts to identify the target’s operating system.

- `--script`: **Run NSE Scripts**  
  Executes specific Nmap Scripting Engine (NSE) scripts for advanced tasks.

## Speed and Performance Control
- `-T<0-5>`: **Timing Template**  
  Adjusts scan speed (0: slowest/stealthiest, 5: fastest/noisiest).

- `--min-rate <rate>`: **Minimum Rate**  
  Sets the minimum packets per second.

- `--max-rate <rate>`: **Maximum Rate**  
  Sets the maximum packets per second.

## Port Scanning
- `-p <ports>`: **Specify Ports**  
  Defines which ports to scan. Example: `-p 80,443` or `-p 1-1000`.

- `-p-`: **All Ports**  
  Scans all 65,535 ports.

## Output and Results
- `-oN <file>`: **Normal Output**  
  Saves the result in a human-readable format.

- `-oX <file>`: **XML Output**  
  Saves the result in XML format for automated analysis.

- `-oG <file>`: **Grepable Output**  
  Format suitable for tools like `grep`.

- `-v` / `-vv`: **Verbose Mode**  
  Shows detailed information during the scan.

## Other Useful Options
- `--top-ports <n>`: **Scan the Top N Ports**  
  Scans the most commonly used ports based on Nmap statistics.

- `--reason`: **Show Reasons**  
  Displays why a port or host is in a certain state.

- `--open`: **Show Only Open Ports**  
  Filters results to display only open ports.

- `-6`: **IPv6 Support**  
  Performs scans on IPv6 networks.

- `-f`: **Packet Fragmentation**  
  Splits packets to evade firewalls or IDS.

---



# Parámetros más importantes de Nmap - ESPAÑOL

## Escaneo básico
- `-sS`: **TCP SYN Scan**  
  Realiza un escaneo rápido y sigiloso al enviar paquetes SYN. Ideal para descubrir puertos abiertos sin establecer conexiones completas.

- `-sT`: **TCP Connect Scan**  
  Escaneo más básico, utiliza la conexión completa (three-way handshake). Es menos sigiloso.

- `-sU`: **UDP Scan**  
  Escanea puertos UDP abiertos. Útil para servicios como DNS, SNMP, etc.

## Descubrimiento de hosts
- `-sn`: **Ping Scan**  
  Determina qué hosts están activos sin realizar un escaneo de puertos.

- `-Pn`: **No Ping**  
  Escanea directamente sin verificar si el host está activo. Útil en redes donde el ICMP está bloqueado.

- `-PS`: **TCP SYN Ping**  
  Envía paquetes SYN a puertos específicos para identificar hosts activos.

- `-PU`: **UDP Ping**  
  Envía paquetes UDP para descubrir hosts activos.

## Opciones de escaneo avanzadas
- `-A`: **Detección avanzada**  
  Combina detección de SO, servicios y scripts. Muy completo pero más lento.

- `-sV`: **Detección de servicios**  
  Identifica versiones de servicios en los puertos abiertos.

- `-O`: **Detección de sistema operativo**  
  Intenta identificar el sistema operativo del host objetivo.

- `--script`: **Ejecutar scripts NSE**  
  Ejecuta scripts específicos de Nmap Scripting Engine (NSE) para tareas avanzadas.

## Control de velocidad y rendimiento
- `-T<0-5>`: **Timing Template**  
  Ajusta la velocidad del escaneo (0: más lento y sigiloso, 5: más rápido y ruidoso).

- `--min-rate <rate>`: **Velocidad mínima**  
  Define la cantidad mínima de paquetes enviados por segundo.

- `--max-rate <rate>`: **Velocidad máxima**  
  Define la cantidad máxima de paquetes enviados por segundo.

## Escaneo de puertos
- `-p <puertos>`: **Especificar puertos**  
  Define los puertos a escanear. Ejemplo: `-p 80,443` o `-p 1-1000`.

- `-p-`: **Todos los puertos**  
  Escanea los 65,535 puertos.

## Salida y resultados
- `-oN <archivo>`: **Salida normal**  
  Guarda el resultado en formato legible para humanos.

- `-oX <archivo>`: **Salida XML**  
  Guarda el resultado en formato XML para análisis automatizado.

- `-oG <archivo>`: **Salida Grepable**  
  Formato para usar con herramientas como `grep`.

- `-v` / `-vv`: **Modo verbose**  
  Muestra información detallada durante el escaneo.

## Otras opciones útiles
- `--top-ports <n>`: **Escanear los N puertos más comunes**  
  Escanea los puertos más usados según las estadísticas de Nmap.

- `--reason`: **Mostrar razones**  
  Explica por qué un puerto o host está en cierto estado.

- `--open`: **Mostrar solo puertos abiertos**  
  Filtra los resultados para mostrar únicamente los puertos abiertos.

- `-6`: **Soporte IPv6**  
  Realiza escaneos en redes IPv6.

- `-f`: **Fragmentación de paquetes**  
  Divide los paquetes para evadir firewalls o IDS.

---

