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

## 📜 Código fuente (Sketch de Arduino)

Copia este código y pégalo en tu Arduino IDE. Asegúrate de modificar la línea donde dice `"AQUI_VA_TU_CLAVE"` con el UID real de tu tarjeta.

```cpp
#include <SPI.h>
#include <MFRC522.h>

#define SS_PIN 5
#define RST_PIN 22
#define LED_PIN 2  // Cambia a 4 si usas un LED externo en el pin D4

MFRC522 mfrc522(SS_PIN, RST_PIN);

void setup() {
  Serial.begin(115200);
  SPI.begin();
  mfrc522.PCD_Init();
  
  // Configurar el LED como salida y empezar apagado
  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, LOW); 
  
  Serial.println("Sistema iniciado. Acerca tu tarjeta blanca al lector azul...");
}

void loop() {
  // Revisar si hay nuevas tarjetas presentes
  if ( ! mfrc522.PICC_IsNewCardPresent()) {
    return;
  }
  // Leer la tarjeta
  if ( ! mfrc522.PICC_ReadCardSerial()) {
    return;
  }

  Serial.print("UID de tu tarjeta es: ");
  String content= "";
  for (byte i = 0; i < mfrc522.uid.size; i++) {
     Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " ");
     Serial.print(mfrc522.uid.uidByte[i], HEX);
     content.concat(String(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " "));
     content.concat(String(mfrc522.uid.uidByte[i], HEX));
  }
  Serial.println();
  content.toUpperCase();

  // Aquí validamos la llave maestra
  if (content.substring(1) == "AQUI_VA_TU_CLAVE") {
    Serial.println("¡ACCESO CONCEDIDO! Abriendo la puerta...");
    
    digitalWrite(LED_PIN, HIGH); // Enciende la luz
    delay(3000);                 // Espera 3 segundos
    digitalWrite(LED_PIN, LOW);  // Apaga la luz
    
  } else {
    Serial.println("ACCESO DENEGADO. Tarjeta no reconocida.");
    delay(1000);
  }
}