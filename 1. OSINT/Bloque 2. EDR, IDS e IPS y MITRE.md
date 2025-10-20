
#### Tabla de Contenidos:

1. [[#EDR (Endpoint Detection and Response)|EDR (Endpoint Detection and Response)]]
	1. [[#EDR (Endpoint Detection and Response)#¿Cómo funciona internamente un EDR?|¿Cómo funciona internamente un EDR?]]
	2. [[#EDR (Endpoint Detection and Response)#EDR Hooking|EDR Hooking]]
	3. [[#EDR (Endpoint Detection and Response)#Cómo detectan amenazas los EDR|Cómo detectan amenazas los EDR]]
	4. [[#EDR (Endpoint Detection and Response)#Ventajas y desventajas de los EDR|Ventajas y desventajas de los EDR]]
2. [[#IDS / IPS (Intrusion Detection / Prevention System)|IDS / IPS (Intrusion Detection / Prevention System)]]
	1. [[#IDS / IPS (Intrusion Detection / Prevention System)#Tipos de detección en IDS/IPS|Tipos de detección en IDS/IPS]]
	2. [[#IDS / IPS (Intrusion Detection / Prevention System)#Ubicación estratégica de un IDS/IPS|Ubicación estratégica de un IDS/IPS]]
3. [[#MITRE ATT&CK Framework|MITRE ATT&CK Framework]]
	1. [[#MITRE ATT&CK Framework#Estructura del framework|Estructura del framework]]
	2. [[#MITRE ATT&CK Framework#¿Cómo se aplica MITRE ATT&CK en defensa?|¿Cómo se aplica MITRE ATT&CK en defensa?]]

#### EDR (Endpoint Detection and Response)

Un EDR es una solución avanzada de seguridad que monitoriza, analiza y responde ante comportamientos sospechosos en los equipos finales (endpoints). No sustituye al antivirus, sino que lo complementa proporcionando una visión profunda y en tiempo real del sistema operativo.

**Objetivo principal:** detectar, investigar y responder ante amenazas que eluden las protecciones tradicionales.

##### ¿Cómo funciona internamente un EDR?

Los EDR recogen y almacenan telemetría detallada de lo que ocurre en los equipos.

Información que recopilan:
- Creación y finalización de procesos.
- Conexiones de red (entrantes y salientes).
- Carga y descarga de DLLs (Dynamic Link Libraries).
- Cambios en el registro de Windows.
- Modificaciones de archivos del sistema y del usuario.
- Llamadas a funciones de la API de Windows.

Toda esta información se analiza localmente y se envía a una plataforma central, donde un analista puede:
- Buscar indicadores de compromiso (IOCs).
- Detectar patrones de ataque (TTPs).
- Responder (bloquear, aislar, eliminar, restaurar).

##### EDR Hooking

Uno de los mecanismos más importantes que usan los EDR es el hooking (enganchar o interceptar llamadas).

Proceso paso a paso:

1. El EDR coloca una instrucción (hook) en funciones clave del sistema.
	- Ejemplo: CreateProcessW, LoadLibrary, NtWriteVirtualMemory, etc.
2. . Cuando una aplicación llama a una de esas funciones, la ejecución se redirige primero al EDR.
3. El EDR analiza la acción:
	- ¿Está creando un proceso sospechoso?
	- ¿Está inyectando memoria en otro proceso?
4. Si detecta un comportamiento malicioso → bloquea la acción o genera alerta.
	- Si no → permite continuar.

Este método permite detectar técnicas avanzadas como:
- Inyección de código (Process Injection).
- Cargas reflejadas en memoria (Reflective DLL Loading).
- Escalada de privilegios o persistencia oculta.

##### Cómo detectan amenazas los EDR

| Tipo de Detección                                     | Descripción                                                | Ejemplo                                                                        |
| ----------------------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------------------ |
| **Basado en firmas**                                  | Igual que el antivirus clásico: compara hashes o patrones. | Archivo malicioso conocido.                                                    |
| **Basado en comportamiento**                          | Analiza las acciones realizadas por los procesos.          | `Word.exe` lanza `PowerShell.exe` → conexión externa.                          |
| **Basado en inteligencia de amenazas (Threat Intel)** | Consulta bases de datos de IOCs y campañas conocidas.      | IPs o dominios asociados a un grupo APT.                                       |
| **Detección por correlación**                         | Une eventos para detectar una cadena de ataque completa.   | `cmd.exe` + `whoami` + `net user` + `powershell` → patrón de post-explotación. |
##### Ventajas y desventajas de los EDR

| Ventajas                                         | Desventajas                        |
| ------------------------------------------------ | ---------------------------------- |
| Visibilidad completa del endpoint.               | Consumen recursos del sistema.     |
| Detección de ataques 0-day y fileless.           | Pueden generar falsos positivos.   |
| Permiten aislar equipos de la red.               | Requieren personal cualificado.    |
| Correlación de eventos y respuesta automatizada. | Coste elevado en entornos grandes. |

#### IDS / IPS (Intrusion Detection / Prevention System)

Los IDS/IPS son sistemas de detección o prevención de intrusiones, se instalan normalmente en la red (aunque existen versiones de host) y su misión es monitorizar el tráfico en busca de actividades maliciosas.

| Sistema                               | Función                                                        |
| ------------------------------------- | -------------------------------------------------------------- |
| **IDS (Intrusion Detection System)**  | Detecta comportamientos sospechosos y genera alertas.          |
| **IPS (Intrusion Prevention System)** | Además de detectar, bloquea o interrumpe el tráfico malicioso. |
##### Tipos de detección en IDS/IPS

**Basado en firmas**: Compara patrones de tráfico con una base de datos de amenazas conocidas.
- Ejemplo: detectar el user-agent de Gobuster en un escaneo web.

**Basado en anomalías o protocolos**: Analiza si el tráfico cumple con las reglas esperadas del protocolo.
- Ejemplo: detectar paquetes HTTP malformados o comandos FTP ilegales.

**Basado en comportamiento estadístico**: Aprende el comportamiento normal de la red y alerta ante desviaciones.
- Ejemplo: un pico de conexiones TCP inusuales en poco tiempo.

##### Ubicación estratégica de un IDS/IPS

Los IDS/IPS deben colocarse en lugares donde puedan observar tráfico relevante:
- **En el perímetro**: entre la red interna y el acceso a Internet.
- **En zonas críticas (DMZ)**: servidores públicos (web, correo).
- **Entre VLANs sensibles**: para detectar movimientos laterales internos.

#### MITRE ATT&CK Framework

MITRE ATT&CK (Adversarial Tactics, Techniques & Common Knowledge) es una base de conocimiento global y abierta mantenida por MITRE. Documenta las tácticas y técnicas reales utilizadas por grupos de ataque (APT), desde el acceso inicial hasta la exfiltración de datos.

Es una referencia estándar para:
- Analistas SOC.
- Pentesters.
- Red Teams y Blue Teams.
- Fabricantes de herramientas de detección.

##### Estructura del framework

|Nivel|Descripción|Ejemplo|
|---|---|---|
|**Táctica**|Objetivo estratégico del atacante (el “por qué”).|_Initial Access_, _Persistence_, _Privilege Escalation_.|
|**Técnica**|Método específico para lograr esa táctica (el “cómo”).|T1059 – Command and Scripting Interpreter.|
|**Subtécnica**|Variante o detalle específico dentro de una técnica.|T1059.001 – PowerShell.|
##### ¿Cómo se aplica MITRE ATT&CK en defensa?

Los equipos de seguridad y las herramientas (como EDR, SIEM o SOAR) usan ATT&CK para:
- **Mapear detecciones existentes**: saber qué tácticas están cubiertas.
- **Identificar brechas**: tácticas sin detección actual.
- **Estandarizar informes**: usar nomenclaturas comunes.
- **Simular ataques**: Red Teams pueden reproducir técnicas concretas.