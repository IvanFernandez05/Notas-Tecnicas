
#### Tabla de Contenidos:

1. [[#Arquitectura general de Windows|Arquitectura general de Windows]]
2. [[#System Calls|System Calls]]
3. [[#Gestión de memoria en Windows|Gestión de memoria en Windows]]
4. [[#Procesos y subprocesos|Procesos y subprocesos]]
5. [[#Tipos de detección en antivirus|Tipos de detección en antivirus]]
	1. [[#Tipos de detección en antivirus#Detección heurística y por comportamiento|Detección heurística y por comportamiento]]
6. [[#Windows Defender|Windows Defender]]
	1. [[#Windows Defender#AMSI (Antimalware Scan Interface)|AMSI (Antimalware Scan Interface)]]

#### Arquitectura general de Windows

Windows está diseñado en capas, separando el modo de usuario del modo núcleo (kernel). Esta separación mantiene la estabilidad del sistema y protege el acceso al hardware.

Kernel Mode
- Nivel más privilegiado del sistema (Ring 0).
- Ejecuta el núcleo de Windows (NTOSKRNL.EXE) y controladores de dispositivo (drivers).
- Tiene acceso completo al hardware y a la memoria física.
- Un error en este nivel puede causar un “pantallazo azul” (BSOD).
- Aquí se gestionan los procesos, memoria, interrupciones, planificación y seguridad.

User Mode
- Nivel con privilegios restringidos (Ring 3).
- Aquí se ejecutan las aplicaciones de usuario (como Word, navegador, etc.) y los subsistemas de Windows (como Win32 o .NET).
- No pueden acceder directamente al hardware.
- Un fallo aquí solo afecta al programa, no a todo el sistema.

#### System Calls

Cuando una aplicación necesita recursos del sistema (como leer un archivo o usar la red), no accede directamente al kernel, sino que pasa por una cadena de llamadas controladas.

Flujo de ejecución:
1. La aplicación usa una API de Windows (por ejemplo, CreateFileW() en kernel32.dll).
2. Esa API llama a una función dentro de ntdll.dll, que actúa como puente.
3. ntdll.dll ejecuta la instrucción syscall, que cambia el modo de ejecución de User Mode → Kernel Mode.
4. El kernel gestiona la solicitud y devuelve el resultado.

#### Gestión de memoria en Windows

Windows usa memoria virtual para proporcionar a cada proceso su propio espacio de direcciones, aislado de los demás.

**Memoria virtual**: Cada proceso tiene su propio espacio de direcciones, esto evita interferencias entre procesos.
**Paginación**: El Virtual Memory Manager (VMM) traduce direcciones virtuales → físicas en la RAM, si una página no está en memoria, se carga desde el archivo de paginación (pagefile.sys).
**Protección de memoria**: Impide que un proceso lea o modifique la memoria de otro proceso o del kernel.

Técnicas de explotación como Buffer Overflow o Process Injection buscan precisamente romper estas barreras.

#### Procesos y subprocesos

Proceso:
- Contenedor que agrupa recursos del sistema (memoria, manejadores, variables de entorno, etc.).
- Representa un programa en ejecución.

Subproceso (Thread):
- Unidad mínima de ejecución planificada por la CPU.
- Un proceso puede tener múltiples threads ejecutándose en paralelo.
	- Ejemplo: un navegador tiene varios hilos (uno por pestaña o proceso interno).

Monitorización:
Los antivirus y EDRs (Endpoint Detection and Response) observan:
- Creación y finalización de procesos.
- Subprocesos inusuales (por ejemplo, Word.exe creando PowerShell.exe).
- Inyecciones o manipulaciones en memoria.

#### Tipos de detección en antivirus

| Tipo de Análisis                | Descripción                                                         | Ejemplo                                                 |
| ------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------- |
| **Análisis Estático**           | Examina el archivo sin ejecutarlo.                                  | Buscar cadenas, firmas o estructuras sospechosas.       |
| **Análisis por Firma**          | Compara el archivo con una base de datos de malware conocido.       | Detectar “WannaCry.exe” por hash o patrón.              |
| **Análisis Heurístico**         | Busca comportamientos sospechosos basándose en reglas y puntuación. | Archivo comprimido o con código inyectable.             |
| **Análisis por Comportamiento** | Observa el programa en ejecución (sandbox o tiempo real).           | `word.exe` lanza `powershell.exe` hacia una IP externa. |

##### Detección heurística y por comportamiento

Heurística:

Busca atributos “anormales” en el código o estructura del binario.
- Cada indicador suma una puntuación de malicia.
- Ejemplo de criterios:
	- El fichero está empaquetado u ofuscado.
	- Intenta escribir en directorios del sistema.
	- Contiene llamadas a APIs sospechosas (como CreateRemoteThread o VirtualAllocEx).

Comportamiento:

El antivirus ejecuta el programa en una sandbox o lo monitoriza en tiempo real.
Busca Indicadores de Compromiso (IOCs) como:
- Modificación del registro.
- Creación de procesos hijos inusuales.
- Conexiones de red extrañas.

#### Windows Defender

Es el antivirus integrado en Windows 10/11. 
- Combina detección por firmas, heurística, análisis en la nube (Cloud Protection) y detección de comportamiento. 
- Usa el servicio AMSI (Antimalware Scan Interface) para analizar scripts antes de su ejecución.

##### AMSI (Antimalware Scan Interface)

Una interfaz estándar que permite a cualquier aplicación o servicio integrarse con el producto antimalware del sistema.

Problema que resuelve:
- El malware moderno ya no siempre existe como archivo en disco (“fileless malware”).
- Muchos ataques se ejecutan directamente en memoria usando scripts (PowerShell, VBScript, macros de Office).
- AMSI analiza el contenido desofuscado justo antes de su ejecución, interceptando la llamada.

Funcionamiento:
1. El script se ejecuta (por ejemplo, PowerShell).
2. Antes de correr cada comando, AMSI analiza el contenido.
3. Si detecta patrones maliciosos, bloquea la ejecución o alerta al antivirus.

