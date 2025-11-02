
Escanea los puertos más comunes de una IP.

```bash
nmap 192.168.1.1
```

Escanea un puerto específico.

```bash
nmap -p 80 192.168.1.1
```

Escanea un rango de puertos.

```bash
nmap -p 1-1000 192.168.1.1
```

Escaneo TCP SYN (rápido y sigiloso).

```bash
nmap -sS 192.168.1.1
```

Escaneo TCP completo (con conexión establecida).

```bash
nmap -sT 192.168.1.1
```

Escaneo de puertos UDP.

```bash
nmap -sU 192.168.1.1
```

Escaneo avanzado: detecta sistema operativo, versión y servicios.

```bash
nmap -A 192.168.1.1
```

Intenta identificar el sistema operativo.

```bash
nmap -O 192.168.1.1
```

Detecta versiones de servicios en ejecución.

```bash
nmap -sV 192.168.1.1
```

Omite la detección de hosts activos (útil si bloquean pings).

```bash
nmap -Pn 192.168.1.1
```

Escaneo de ping (solo descubre qué hosts están activos).

```bash
nmap -sn 192.168.1.0/24
```