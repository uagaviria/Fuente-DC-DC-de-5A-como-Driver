# Fuente XL4015 DC-DC de 5A como Driver

## DESCRIPCIÓN:

Con este Convertidor DC-DC XL4015 Step-Down podrás reducir una caída de potencial de Corriente Directa con alta eficiencia, baja ondulación y excelente línea de regulación de carga. En una frecuencia fija de 180 KHz PWM el Buck (reductor) Requiere una mínima cantidad de componentes externos, el regulador es simple de usar e incluye un oscilador de frecuencia fija.

El circuito de control PWM puede ajustar el relación de trabajo linealmente de 0 a 100%. Un exceso la función de protección actual está construida en el interior. Cuando ocurre una función de protección corta, la frecuencia de operación se reducirá desde 18 0 kHz a 48 kHz. Una compensación interna bloque está integrado para minimizar conteo de componentes.
Este dispositivo tiene una gran cantidad de aplicaciones, principalmente para monitores LCD y TV, ya que deben tener una alimentación adecuada, así como en dispositivos de radiofrecuencia que también son más sensibles a las variaciones de voltaje. Fuentes de alimentación y equipos de telecomunicaciones.
 

 ## Convertidor DC-DC XL4015 Step-Down:

- Destacados del módulo: alta potencia 5A, alta eficiencia y bajo rizado con el indicador de energía
- Voltaje de entrada: 8-36V (entrada por favor trate de no exceder de 38 V)
- Voltaje de salida: 1.25-32V continuamente ajustable
- Corriente de salida: 0-5A
- Potencia de salida: 75W
- Temperatura de funcionamiento: -40 a +85 grados
- Frecuencia de trabajo: 180KHz
- Eficiencia de conversión: hasta el 96%
- Regulación de la carga: S (I) ≤0.8% (entrada de 24V, 12V de salida, la carga varía de 4,5 a 1 ~ medido)
- Regulación de voltaje: S (u) ≤0.8% (salida de 12V, cuando 4A, cuando 18 ~ 32V cambio de tensión de la medida)
- Indicador de encendido: Sí
- Protección contra cortocircuitos: Sí (límite actual 8A)
- protección de sobrecalentamiento: (apagar automáticamente la salida después de sobretemperatura)
- Entrada de protección de polaridad inversa: No
- Instalación: dos tornillos de 3 mm
- Conexión: Soldadura, V-IN es la entrada, V-OUT es la salida
- Dimensiones del módulo: 54 x 23 x 15 mm (L x W x H)

Las aplicaciones de módulos buck CC / CC de la tensión de entrada es mayor que la tensión de salida del buck campo, tales como la batería, transformador de potencia, fuente de alimentación regulada ajustable de bricolaje, 24V Vehículo fuente de alimentación portátil, equipos industriales buck.12V a 3.3V , 12V a 5V, 24V a 5V, 24V a 12V, 36V a 24V etc.



# ESP8266 – SPIFFS + JSON
Todos los ESP8266 tienen un “disco rígido” interno, llamado SPIFFS (SPI Flash File System), en el cual podemos leer y grabar archivos desde el código de Arduino o de forma manual. Justamente en este tutorial de IOT (internet de las cosas) vamos a ver cómo acceder al SPIFFS parar leer y guardar datos en un archivo Json (JavaScript Object Notation).
First attempt at a library. Lots more changes and fixes to do. Contributions are welcome.

## Video
ver este video: https://youtu.be/qRiVBgZENwY


### Codigo de ejemplo
- sketch
```cpp
// Ejemplo: almacenamiento del archivo de configuración JSON en el sistema de archivos flash
//
// Utiliza la biblioteca ArduinoJson de Benoit Blanchon.
// https://github.com/bblanchon/ArduinoJson
//
// Este código de ejemplo está en el dominio público.

#include <ArduinoJson.h>
#include "FS.h"

bool loadConfig() {
  File configFile = SPIFFS.open("/config.json", "r");
  if (!configFile) {
    Serial.println("Failed to open config file");
    return false;
  }

  size_t size = configFile.size();
  if (size > 1024) {
    Serial.println("Config file size is too large");
    return false;
  }

  // Asigne un buffer para almacenar el contenido del archivo.
  std::unique_ptr<char[]> buf(new char[size]);

   // No usamos String aquí porque la biblioteca ArduinoJson requiere la entrada
   // buffer para ser mutable Si no usas ArduinoJson, también puedes
   // use configFile.readString en su lugar.
  configFile.readBytes(buf.get(), size);

  StaticJsonBuffer<200> jsonBuffer;
  JsonObject& json = jsonBuffer.parseObject(buf.get());

  if (!json.success()) {
    Serial.println("Error al analizar el archivo de configuración");
    return false;
  }

  const char* serverName = json["serverName"];
  const char* accessToken = json["accessToken"];

   // La aplicación del mundo real almacenaría estos valores en algunas variables para
   // uso posterior.

  Serial.print("Loaded serverName: ");
  Serial.println(serverName);
  Serial.print("Loaded accessToken: ");
  Serial.println(accessToken);
  return true;
}

bool saveConfig() {
  StaticJsonBuffer<200> jsonBuffer;
  JsonObject& json = jsonBuffer.createObject();
  json["serverName"] = "api.example.com";
  json["accessToken"] = "128du9as8du12eoue8da98h123ueh9h98";

  File configFile = SPIFFS.open("/config.json", "w");
  if (!configFile) {
    Serial.println("Error al abrir el archivo de configuración para escribir");
    return false;
  }

  json.printTo(configFile);
  return true;
}

void setup() {
  Serial.begin(115200);
  Serial.println("");
  delay(1000);
  Serial.println("Montaje FS...");

  if (!SPIFFS.begin()) {
    Serial.println("Error al montar el sistema de archivos");
    return;
  }

  // envia la configuracion a la memoria flash. descomentar si es necesario.
  //if (!saveConfig()) {
  //  Serial.println("Error al guardar la configuración");
  //} else {
  //  Serial.println("Configuracion guardada");
  //}

  if (!loadConfig()) {
    Serial.println("Error al cargar la configuración");
  } else {
    Serial.println("Configuración cargada");
  }
}

void loop() {
}
```

- links para descarga de archivos
```cpp
https://github.com/esp8266/arduino-esp8266fs-plugin

http://esp8266.github.io/Arduino/versions/2.0.0/doc/filesystem.html
```
