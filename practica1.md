# Práctica 1 - Blink con ESP32

## Objetivo

El objetivo de esta práctica es implementar un programa básico de parpadeo de un LED utilizando la placa ESP32-S3-DevKitC-1. Además, se utiliza el puerto serie para depurar el funcionamiento del programa mostrando mensajes de estado.

---

# Material utilizado

- ESP32-S3-DevKitC-1
- Visual Studio Code
- PlatformIO
- Framework Arduino
- LED conectado al GPIO21

---

# Configuración del proyecto

## Archivo `platformio.ini`

```ini
[env:esp32-s3-devkitc-1]
platform = espressif32
board = esp32-s3-devkitc-1
framework = arduino
monitor_speed = 115200
```

---

# Código implementado

## Archivo `main.cpp`

```cpp
#include <Arduino.h>

// Pin utilizado para el LED
const int pinLed = 21;

void setup() {

    // Configuración del pin como salida
    pinMode(pinLed, OUTPUT);

    // Inicialización del puerto serie
    Serial.begin(115200);
}

void loop() {

    // Encender LED
    digitalWrite(pinLed, HIGH);

    // Mensaje por puerto serie
    Serial.println("ON");

    // Espera de 500 ms
    delay(500);

    // Apagar LED
    digitalWrite(pinLed, LOW);

    // Mensaje por puerto serie
    Serial.println("OFF");

    // Espera de 500 ms
    delay(500);
}
```

---

# Funcionamiento del programa

El programa ejecuta un bucle infinito donde el LED conectado al GPIO21 se enciende y apaga periódicamente cada 500 milisegundos.

Durante el funcionamiento, también se envían mensajes por el puerto serie indicando el estado actual del LED:

- `"ON"` cuando el LED se enciende.
- `"OFF"` cuando el LED se apaga.

El periodo completo de funcionamiento es de 1 segundo.

---

# Diagrama de flujo

```text
        ┌──────────────┐
        │    Inicio    │
        └──────┬───────┘
               │
               ▼
    ┌────────────────────┐
    │ Configurar GPIO21  │
    │ como salida        │
    └─────────┬──────────┘
              │
              ▼
    ┌────────────────────┐
    │ Inicializar puerto │
    │ serie              │
    └─────────┬──────────┘
              │
              ▼
      ┌────────────────┐
      │ Encender LED   │
      └──────┬─────────┘
             │
             ▼
      ┌────────────────┐
      │ Mostrar "ON"   │
      └──────┬─────────┘
             │
             ▼
      ┌────────────────┐
      │ Esperar 500 ms │
      └──────┬─────────┘
             │
             ▼
      ┌────────────────┐
      │ Apagar LED     │
      └──────┬─────────┘
             │
             ▼
      ┌────────────────┐
      │ Mostrar "OFF"  │
      └──────┬─────────┘
             │
             ▼
      ┌────────────────┐
      │ Esperar 500 ms │
      └──────┬─────────┘
             │
             └───────► Repetir
```

---

# Diagrama temporal

```text
LED

ON  ────────────────        ────────────────
                    │      │
OFF                 └──────┘
     0 ms         500 ms   1000 ms

Puerto serie

ON ------------------------ OFF ------------------------
```

---

# Tiempo libre del procesador

Durante las instrucciones `delay(500)` el procesador permanece esperando durante 500 milisegundos.

En cada iteración del programa existen dos retardos de 500 ms:

- 500 ms con el LED encendido.
- 500 ms con el LED apagado.

Por tanto, el procesador permanece prácticamente libre durante casi todo el tiempo de ejecución, ya que las instrucciones de encendido, apagado y escritura serie tardan muy poco comparadas con los retardos.

El tiempo libre aproximado del procesador es cercano al 100% del ciclo de ejecución.

---

# Conclusiones

En esta práctica se ha aprendido a:

- Configurar un GPIO como salida.
- Utilizar el puerto serie para depuración.
- Implementar retardos temporales.
- Programar un bucle infinito en ESP32.

Además, se ha comprobado el correcto funcionamiento del entorno PlatformIO y la comunicación con la placa ESP32-S3.