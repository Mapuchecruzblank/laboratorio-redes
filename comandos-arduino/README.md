# 🚪 Sistema de Control de Acceso con ESP32 y RFID (RC522)

Este proyecto es un sistema básico de control de acceso utilizando un microcontrolador **ESP32** y un módulo lector de tarjetas **RFID-RC522**. El sistema lee el identificador único (UID) de una tarjeta y lo compara con una "llave maestra" preprogramada para conceder o denegar el acceso.

## 🛠️ Materiales Necesarios

* Placa de desarrollo **ESP32** (ej. DOIT ESP32 DEVKIT V1 de 38 pines).
* Módulo lector **RFID-RC522** (con los pines soldados a la placa para un correcto contacto).
* Tarjeta o llavero RFID (frecuencia 13.56 MHz).
* Cables Jumper hembra-hembra.
* Cable Micro USB (con soporte para transferencia de datos).
* *(Opcional)* LED externo para usar como indicador visual de acceso concedido.

## 🔌 Conexiones de Hardware (Wiring)

El lector RFID se comunica con el ESP32 a través del protocolo **SPI**. Es crítico realizar las conexiones a los pines correctos y respetar los voltajes.

| Pin RFID-RC522 | Pin ESP32 | Descripción |
| :--- | :--- | :--- |
| **3.3V** | **3V3** | Alimentación (¡ADVERTENCIA: NUNCA conectar a 5V o VIN!) |
| **RST** | **D22** | Reset |
| **GND** | **GND** | Tierra / Negativo |
| **IRQ** | *No conectado* | Interrupción (No se usa en este proyecto) |
| **MISO** | **D19** | Master In Slave Out |
| **MOSI** | **D23** | Master Out Slave In |
| **SCK** | **D18** | Serial Clock |
| **SDA (SS)**| **D5** | Slave Select / Data |

*(Opcional)* Conexión de luz LED indicadora: Conectar la pata larga (positivo) al pin **D4** del ESP32, y la pata corta (negativo) a cualquier pin **GND**.

## 💻 Requisitos de Software

Para ejecutar este proyecto, necesitas configurar el entorno de Arduino:
1. **Arduino IDE**: Descargado e instalado en tu PC.
2. **Placas ESP32**: Haber instalado la URL de Espressif en el gestor de tarjetas.
3. **Librerías**: Tener instalada la librería **MFRC522** (por GithubCommunity) desde el Gestor de Bibliotecas.

## 🚀 Instalación y Configuración

### Paso 1: Descubrir el UID de tu tarjeta
1. Conecta tu ESP32 al computador vía USB.
2. Abre el código en Arduino IDE. Ve a `Herramientas`, selecciona la placa **DOIT ESP32 DEVKIT V1** y el puerto **COM** correspondiente.
3. Sube el código. *(Nota: Si la consola se queda en "Connecting...", mantén presionado el botón físico **BOOT** de la placa hasta que empiece a cargar).*
4. Abre el **Monitor Serie** y configúralo a **115200 baudios**.
5. Acerca tu tarjeta al lector azul. La consola imprimirá un código similar a: `UID de tu tarjeta es: 7A B8 C9 2D`. Copia este código.

### Paso 2: Registrar la Llave Maestra
1. En el código de Arduino, busca la línea de validación (aproximadamente en la línea 42):
   ```cpp
   if (content.substring(1) == "AQUI_VA_TU_CLAVE") 