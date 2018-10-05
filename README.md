# Fuente XL4015 DC-DC de 5A como Driver

## DESCRIPCIÓN:
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

Con este Convertidor DC-DC XL4015 Step-Down podrás reducir una caída de potencial de Corriente Directa con alta eficiencia, baja ondulación y excelente línea de regulación de carga. En una frecuencia fija de 180 KHz PWM el Buck (reductor) Requiere una mínima cantidad de componentes externos, el regulador es simple de usar e incluye un oscilador de frecuencia fija.

El circuito de control PWM puede ajustar el relación de trabajo linealmente de 0 a 100%. Un exceso la función de protección actual está construida en el interior. Cuando ocurre una función de protección corta, la frecuencia de operación se reducirá desde 18 0 kHz a 48 kHz. Una compensación interna bloque está integrado para minimizar componentes.
Este dispositivo tiene una gran cantidad de aplicaciones, principalmente para monitores LCD y TV, ya que deben tener una alimentación adecuada, así como en dispositivos de radiofrecuencia que también son más sensibles a las variaciones de voltaje. Fuentes de alimentación y equipos de telecomunicaciones.

En este caso quise ir un poco mas allá por la necesidad de tener un driver y poder variarle la velocidad de un motor de 24v 120W a través de Arduino y un encoder de tipo rotativo. Aunque de encoder no vamos a hablar ahora. pero si deseas te remito a esta pagina que tiene un exelente tutorial. https://www.luisllamas.es/arduino-encoder-rotativo/
Dicha fuente es la mentada XL4015 de 5 amperios. 

En esta fuente note que inyectandole pwm con dicha modificación. alimentando la entrada con 24V. la salida de pwm daba el rango entre 90 y 255  siendo 255 para el mortor apagado y 90 el voltaje maximo. esto en arduino se puede mapear.

 ## Convertidor DC-DC XL4015 Step-Down:
<img src="https://raw.githubusercontent.com/uagaviria/Fuente-DC-DC-de-5A-como-Driver/master/img/fuente-dc-dc-regulador-5a.jpg" />
 
 ## Reforma para el funcionamiento por pwm desde el arduino: 
<img src="https://raw.githubusercontent.com/uagaviria/Fuente-DC-DC-de-5A-como-Driver/master/img/driver.png" /> 

# Codigo:

```cpp
const int channelPinA = 2;
const int channelPinB = 10;

int Pin_PWM = 5;
 
const int timeThreshold = 5;
long timeCounter = 0;
 
const int maxSteps = 255;
volatile int ISRCounter = 0;
int counter = 0;
 
bool IsCW = true;
 
void setup()
{
   pinMode(channelPinA, INPUT_PULLUP);
   Serial.begin(9600);
   attachInterrupt(digitalPinToInterrupt(channelPinA), doEncode, CHANGE);
}
 
void loop()
{
   if (counter != ISRCounter)
   {
      counter = ISRCounter;
      Serial.println(counter);
      analogWrite(Pin_PWM, counter);
   }
   delay(100);
}
 
void doEncode()
{
   if (millis() > timeCounter + timeThreshold)
   {
      if (digitalRead(channelPinA) == digitalRead(channelPinB))
      {
         IsCW = true;
         if (ISRCounter + 1 <= maxSteps) ISRCounter++;
      }
      else
      {
         IsCW = false;
         if (ISRCounter - 1 > 0) ISRCounter--;
      }
      timeCounter = millis();
   }
}

```

# Coeccion del encoder:

<img src="https://raw.githubusercontent.com/uagaviria/Fuente-DC-DC-de-5A-como-Driver/master/img/encoder.png" /> 

