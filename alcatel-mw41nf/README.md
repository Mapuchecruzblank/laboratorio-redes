# 🔍 Alcatel MW41NF - Análisis y Diagnóstico de Red

Este repositorio documenta el proceso de auditoría, diagnóstico y bypass de configuración de un router MiFi Alcatel MW41NF, cuyo firmware fue modificado y bloqueado por el ISP (WOM).

## 🎯 Objetivos del Proyecto
1. Restaurar la conectividad a internet usando un APN de otro proveedor (Entel).
2. Intentar habilitar el servicio NAS (Network Attached Storage) oculto mediante la ranura MicroSD.
3. Realizar auditoría de puertos y directorios web para buscar vulnerabilidades en el firmware restringido.

## 🛠️ Herramientas y Comandos Utilizados

### 1. Diagnóstico de Hardware en Linux (Ubuntu)
Se utilizaron comandos del kernel para verificar la detección física del dispositivo por USB:
* `lsblk`: Para listar dispositivos de bloques e intentar montar la MicroSD.
* `lsusb`: Para verificar los buses USB y confirmar que el dispositivo solo se comunicaba como módem y no como almacenamiento masivo por falta de un cable de datos adecuado.
* `sudo dmesg | tail -n 25`: Para auditar los registros del sistema y ver cómo Ubuntu interpretaba la conexión del hardware en tiempo real.

### 2. Configuración de Red
* Bypass del bloqueo de red configurando manualmente el perfil **APN** (`bam.entelpcs.cl`) desde el panel de administración web (`192.168.1.1`).

### 3. Auditoría de Seguridad y Redes (Fuzzing & Escaneo)
Se trató al router como una caja negra para buscar puertas traseras en el firmware de la operadora:
* **Escaneo de puertos básico:** `nmap 192.168.1.1` (Descubrimiento de puertos 53 DNS y 80 HTTP).
* **Escaneo de puertos profundo:** `nmap -p- -T4 192.168.1.1` (Escaneo de los 65.535 puertos para confirmar el bloqueo total de FTP/SMB/SSH).
* **Fuzzing de directorios web:** `dirb http://192.168.1.1` (Ataque de fuerza bruta con diccionarios para buscar directorios ocultos en el panel de administración).

## 📌 Conclusiones
El firmware del ISP deshabilitó por completo los módulos de almacenamiento y transferencia de red (FTP/Samba). La única vía de acceso restante para modificar el sistema operativo interno requiere Hardware Hacking (conexión física vía UART / USB-TTL a los pines TX/RX de la placa base). El equipo quedó funcional como módem LTE de contingencia.
