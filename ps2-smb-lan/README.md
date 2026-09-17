# Configuración de Servidor SMB para PlayStation 2

Este documento detalla los pasos necesarios para conectar una consola PlayStation 2 directamente a un PC mediante un cable de red (RJ45) y cargar juegos a través de la red local utilizando el protocolo SMB.

## Instrucciones de Configuración

1. **Crear el directorio raíz**
   Crea una carpeta en el almacenamiento de tu PC y nómbrala exactamente `PS2SMB`.

2. **Configurar permisos de red**
   Haz clic derecho sobre la carpeta, ve a las propiedades de uso compartido y otorga permisos de "Lectura y Escritura" para "Todos".

3. **Desactivar el Firewall**
   Desactiva temporalmente el Firewall del sistema operativo (o del antivirus) para evitar que bloquee la solicitud de conexión entrante desde la consola.

4. **Desactivar la protección por contraseña**
   En el Centro de redes y recursos compartidos del sistema, selecciona la opción para desactivar el uso compartido con protección por contraseña.

5. **Asignar IP estática al PC**
   Configura el adaptador de red de tu PC con una dirección IP manual dentro del mismo rango que utilizará la consola.
   * Ejemplo de IP: `192.168.1.11`
   * Máscara de subred: Por defecto (usualmente `255.255.255.0`)

6. **Activar el protocolo SMB 1.0**
   Ingresa a las características opcionales del sistema, busca y activa la casilla "Compatibilidad con el protocolo para compartir archivos SMB 1.0/CIFS". Reinicia el equipo para aplicar los cambios de red.

7. **Conexión física**
   Conecta la PlayStation 2 directamente al puerto Ethernet del PC utilizando el cable RJ45.

8. **Verificación de conexión**
   Enciende la consola, inicia el gestor de juegos y verifica que la conexión de red se haya establecido correctamente para mostrar el catálogo alojado en el PC.
