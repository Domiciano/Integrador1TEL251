# Getting started con arduino ESP32

## Arduino IDE
Siga esta guía para instalar ESP32 en su arduino
```
https://randomnerdtutorials.com/installing-the-esp32-board-in-arduino-ide-windows-instructions/
```

1. Abra el programa de Arduino IDE

2. Vaya a File > Preferences

3. En la pestaña de settings ubique la sección 'Additional Boards Manager URLs' copie y pegue la siguiente dirección:

```
https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
```

4. Luego vaya a Board > Boards Manager

5. Busque ESP32 y le debe salir lo siguiente

```
esp32 by Espressif Systems
```

6. Seleccionelo y de en Aceptar y espere a la instalación

Después de eso, ya puede seleccionar su ESP32 en el menú de boards. <br><br>

Tools > Boards > esp32 > ESP32-WROOM-DA Module

## PlatformIO
Estos pasos son para usar PlatformIO sobre Visal Studio Code
<ol>
  <li>En visual studio vaya a extensiones y busque PlatformIO</li>
  <li>Escoja la extensión con ícono de insecto naranja, de platformio.org</li>
  <li>Al instalarla, en el menú de la izquierda aparecerá un insecto, de click allí</li>
  <li>En la sección Quick Access, escoja la opción Open</li>
  <li>De click en la opción de + New Project</li>
  <li>Escoja un nombre cualquiera</li>
  <li>Escoja la board Espressif ESP32 Dev Module</li>
  <li>Escoja Arduino como Framework</li>
  <li>En location, escoga un directorio de archivo que no tenga ni espacios, ni caractéres NO ASCII. Por ejemplo C:/Users/Andrés Londoño/Projects no es un directorio válido</li>
  <li>Espere mientras PlatformIO carga los módulos, la primera vez se demora</li>
</ol>

## Verifique puertos de su board
Entre en esta página y verifique dónde se encuentra sus elementos<br>
https://lastminuteengineers.com/esp32-pinout-reference/
<ol>
  <li>GPIO. General purpose</li>
  <li>ADC. Analog/Digital converter</li>
</ol>

## Plantilla
En el archivo main.cpp puede usar este código de referencia
```c++
#include <Arduino.h>

void setup() {
  Serial.begin(115200);
}

void loop() {
  
}

void serialEvent() {
  if (Serial.available() > 0) {
    String data = Serial.readStringUntil('\n');
    Serial.println(data);
  }
}
```
## Configuración inicial
Debe definir la tasa de baudios de la transmisión en el archivo `platformio.ini`
```ini
...
monitor_speed = 115200
...
```

## Si quiere modifcar el puerto para subir su programa, puede usar
```ini
upload_port=/dev/cu.usbserial-0001
```

```ini
upload_port=COM5
```

# Referencias
<ol>
  <li>https://docs.espressif.com/projects/esp-idf/en/v4.2/esp32/api-reference/peripherals/adc.html#:~:text=The%20ESP32%20integrates%20two%2012,channels%20(analog%20enabled%20pins).</li>
</ol>
