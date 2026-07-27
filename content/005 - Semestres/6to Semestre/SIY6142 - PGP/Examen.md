> [!WARNING] Peligro
> Este ramo tuvo un Profesor Horrible, en el cual no aprendi nada sobre Prototipar proyectos y solucionar problematicas globales, fue mirar un Arduino UNO por un semestre, solamente durante las clases...
# Info
Nuestro problema fue ...

Nuestra solucion fue "Regular las luces Automaticamente"

Nuestro Hardware Usado fue:
- [Arduino UNO](https://docs.arduino.cc/hardware/uno-rev3/): Es el cerebro del proyecto
- Protoboard: Una base para conectar componentes sin soldar
- Led: Simular red de luces
- Resistencias: Para que las cosas no exploten
- Cables Dupont: Conexiones entre Arduino o Protoboard y componentes
- Display LCD 16x2 (Modo 4 Pines): Mostrar estado del sistema
- Sensores
	- LDR (Light Depent Resistor): Sensor para detectar luz ambiental
- Rele: Controlar las luces
- Potenciometro

Software Usado
- [Arduino IDE](https://www.arduino.cc/en/software): Controlar el codigo enviado a Arduino UNO

Flujo del proyecto

La energia llega al Arduino UNO (5V) mediante una entrada de USB, la cual le entrega energia al circuito en la protoboard, el fotoresistor va midiendo la intensidad de la luz ambiente, dependiendo de la hora o la intensidad se activara o desactivara la energia segun el rele

## Conexiones
- Arduino
	- A0 -> LDR A0
	- D2 -> LCD D7
	- D3 -> LCD D6
	- D4 -> LCD D5
	- D5 -> LCD D4
	- D9 -> Rele IN
	- D11 -> LCD E
	- D12 -> LCD RS
	- GND -> Protoboard GND
	- 5V -> Protoboard Alimentacion (5V)
- Protoboard
	- (5V) Alimentacion -> Resistencia 220ohms -> LCD A
	- (5V) Alimentacion -> Resistencia 1K o 220ohms -> LDR VDD
	- (5V) Alimentacion -> Rele VCC
	- (5V) Alimentacion -> Potenciometro Pin 1
	- (5V) Alimentacion -> Resistencia 220ohms -> Anodo LED o Motor
	- GND -> LCD VSS
	- GND -> LDR GND
	- GND -> Rele GND
- Display LCD 16x2 (Modo 4 Pines)
	- VSS -> Protoboard GND
	- VDD -> Protoboard Alimentacion 5V
	- V0 -> Potenciometro Pin 2
	- RS -> Arduino D12
	- RW -> Protoboard GND
	- E -> Arduino D11
	- D4 -> Arduino D5
	- D5 -> Arduino D4
	- D6 -> Arduino D3
	- D7 -> Arduino D2
	- A -> Resistencia 220ohm -> Protoboard Alimentacion (5V)
	- K -> Protoboard GND
- Potenciometro
	- Pin 1 (Izquierdo) -> Protoboard Alimentacion (5V)
	- Pin 2 (Centro) -> LCD V0
	- Pin 3 (Derecha) -> Protoboard GND
- LDR (Sensor de Luz)
	- LDR VCC -> Protoboard Alimentacion (5V)
	- LDR GND -> Protoboard GND
	- LDR A0 -> Resistencia 1K -> Pin A0
- Led
	- Anodo (Pata Larga) -> Arduino D9
	- Catodo (Pata Corta) -> Resistencia 1K -> Protoboard GND
- Rele
	- VCC -> Protoboard Alimentacion (5V)
	- GND -> Protoboard GND
	- IN -> Arduino D9
	- COM -> Motor terminal negativo o Fuente de alimentacion del motor
	- NO -> Motor terminal positivo
- Motor
	- Positivo -> Rele NO
	- Negativo -> Rele COM o Protoboard GND


## Codigo LDR, Led y Display
```arduino
const int RS = 12, E = 11, D4 = 5, D5 = 4, D6 = 3, D7 = 2; // Pines del LCD
const int RELAY_PIN = 9, LDR_PIN = A0;  // Pin del relé y LDR
const int threshold = 500; // Umbral para activar/desactivar el motor

void setup() {
  pinMode(RS, OUTPUT); pinMode(E, OUTPUT); 
  pinMode(D4, OUTPUT); pinMode(D5, OUTPUT); 
  pinMode(D6, OUTPUT); pinMode(D7, OUTPUT);
  pinMode(RELAY_PIN, OUTPUT); // Pin para controlar el relé

  lcdInit();
  Serial.begin(9600);
  lcdPrint("Luz Ambiente:");
}

void loop() {
  int ldrValue = analogRead(LDR_PIN);  // Leer sensor LDR
  int lumenValue = map(ldrValue, 0, 1023, 1000, 0); // Aproximación de lúmenes
  
  // Estado del motor y del sensor LDR
  String motorEstado = (ldrValue > threshold) ? "on" : "off";
  String sensorEstado = (ldrValue > threshold) ? "on" : "off";
  
  // Mostrar la información en el monitor serial
  Serial.print("Luz Ambiental: ");
  Serial.print(lumenValue);
  Serial.print(" lumenes | umbral: ");
  Serial.print(threshold);
  Serial.println(" lumenes");

  Serial.print("Motor: ");
  Serial.println(motorEstado);

  Serial.print("Sensor LDR: ");
  Serial.println(sensorEstado);

  // Control del relé según el valor del LDR y el umbral
  if (ldrValue > threshold) {
    digitalWrite(RELAY_PIN, LOW);  // Enciende el relé (motor ON)
  } else {
    digitalWrite(RELAY_PIN, HIGH);   // Apaga el relé (motor OFF)
  }

  // Mostrar lúmenes y umbral en el LCD
  lcdSetCursor(0, 1);  
  lcdPrintLumen(lumenValue, threshold);
  
  delay(1500);  // Pequeña pausa
}

// Funciones para controlar el LCD
void lcdInit() {
  lcdCommand(0x33); lcdCommand(0x32); lcdCommand(0x28); 
  lcdCommand(0x0C); lcdCommand(0x06); lcdClear();
}

void lcdClear() { lcdCommand(0x01); delay(5); }

void lcdSetCursor(int col, int row) {
  int row_offsets[] = { 0x00, 0x40 };
  lcdCommand(0x80 | (col + row_offsets[row]));
}

void lcdPrint(const char* str) { 
  while (*str) lcdData(*str++); 
}

void lcdPrintLumen(int lumenValue, int threshold) { 
  char buffer[32]; 
  sprintf(buffer, "%d lm | %d lm", lumenValue, threshold); 
  lcdPrint(buffer); 
}

void lcdCommand(byte cmd) {
  digitalWrite(RS, LOW); lcdWrite(cmd);
}

void lcdData(byte data) {
  digitalWrite(RS, HIGH); lcdWrite(data);
}

void lcdWrite(byte value) {
  digitalWrite(D4, (value >> 4) & 0x01);
  digitalWrite(D5, (value >> 5) & 0x01);
  digitalWrite(D6, (value >> 6) & 0x01);
  digitalWrite(D7, (value >> 7) & 0x01);
  lcdEnablePulse();

  digitalWrite(D4, value & 0x01);
  digitalWrite(D5, (value >> 1) & 0x01);
  digitalWrite(D6, (value >> 2) & 0x01);
  digitalWrite(D7, (value >> 3) & 0x01);
  lcdEnablePulse();
}

void lcdEnablePulse() {
  digitalWrite(E, LOW); delayMicroseconds(1);
  digitalWrite(E, HIGH); delayMicroseconds(1);
  digitalWrite(E, LOW); delayMicroseconds(100);
}
```

## Conectar Wifi a Arduino UNO
Info Placa Wifi
- Sitio: [Wemos](https://www.wemos.cc/en/latest/index.html)
- Wrapper: LOL1n
- Modelo Extra: new NodeMcu V3
- Basado en: [LOL1n D32](https://www.wemos.cc/en/latest/d32/d32.html)

Info Chip Modulo Wifi
- Marca: Doiting
- Modelo: [ESP-12E](https://www.espressif.com/en/producttype/esp32-wroom-32)
- ISM: 2.4Ghz (802.11 b/g/m)
- PA: +25dBm

Instrucciones
- Install [CH3408 Driver](https://www.wemos.cc/en/latest/ch340_driver.html) | [CH340 Driver](https://sparks.gogo.co.nz/ch340.html)
- Use 9600bps baud rate
- Connect to Wifi

Ayuda
- [Xtronical - Setting up a NodeMCU V3 ESP-12E](https://www.xtronical.com/basics/systems/esp8266mcu-et-al/setting-nodemcu-v3-esp-12e-esp8266-arduino-ide/)
- [Github - esp8266 wifi](https://github.com/esp8266/Arduino)
- [Github - PubSubClient Library MQTT](https://github.com/knolleary/pubsubclient)
- [The Engineering Projects - Introduction to NodeMCU V3](https://www.theengineeringprojects.com/2018/10/introduction-to-nodemcu-v3.html) | [Pinout](https://images.theengineeringprojects.com/image/webp/2018/10/Introduction-to-NodeMCU-V3.png.webp?ssl=1)
- [Luis Llamas - Enviar y Recibir Mensajes MQTT arduino y pubsubclient](https://www.luisllamas.es/enviar-y-recibir-mensajes-por-mqtt-con-arduino-y-la-libreria-pubsubclient/)
- [Github Luis Llamas - Proyectos ESP8266](https://github.com/luisllamasbinaburo/ESP8266-Examples)
- [ElectroJoan - Tutorial ESP8266](https://electrojoan.com/tutorial-sobre-el-nodemcu/)

## Conexiones
- PC USB-A -> LOL1n Micro-USB-B

## Funcionamiento
1. Instala Arduino IDE, junto con sus dependencias hasta que termine la instalacion
2. Ve a "Archivo" -> "Preferencias" y en "URLs adicionales de gestor de placa" agregamos la siguiente URL:
	- `http://arduino.esp8266.com/stable/package_esp8266com_index.json`
3. Le das en "Aceptar"
4. Abre el menu "Herramientas" -> "Placa" -> "Gestor de Placa" e instalas "Arduino AVR Boards" y "esp8266"
5. Selecciona desde el menu "Herramientas" -> "Placa" -> "esp8266" -> NodeMCU 1.0 (ESP 12E Module)
6. Abre el menu "Herramientas" -> "Gestionar Librerias" y busca la libreria "PubSubClient"
7. Selecciona "USB COM 3 (USB)" desde el selector de placas junto a NodeMCU 1.0 (ESP 12E Module)

## Prueba Rapida de Funcionamiento "Blink"
```arduino
void setup()
{
pinMode(D0, OUTPUT);
}
void loop()
{
digitalWrite(D0, HIGH);
delay(500);
digitalWrite(D0, LOW);
delay(500);
}
```

## Codigo para encender el modulo wifi de forma remota
> [!IMPORTANT] Importante
> Necesitas crear un HostSpot con la configuracion "SSID" -> "Weno", "Password" -> "123456890"

```arduino
#include <ESP8266WiFi.h>
#include <WiFiServer.h>

const char* ssid = "Weno";
const char* password = "1234567890";
int ledPin = 2; 
WiFiServer server(80);

void setup() {
  Serial.begin(9600);
  delay(10);

  // Configuración del pin LED
  pinMode(ledPin, OUTPUT);
  digitalWrite(ledPin, LOW); // Asegura que el LED esté apagado al inicio

  // Conexión a la red WiFi
  Serial.println();
  Serial.println("Conectando a WiFi...");
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println();
  Serial.println("Conexión WiFi establecida");

  // Iniciar el servidor
  server.begin();
  Serial.println("Servidor iniciado");
  Serial.print("Usa esta URL para conectarte: ");
  Serial.print("http://");
  Serial.println(WiFi.localIP());
  Serial.println("/");
}

void loop() {
  // Verificar si hay un cliente conectado
  WiFiClient client = server.available();
  if (!client) {
    return;
  }

  // Esperar a que el cliente envíe una solicitud
  Serial.println("Nuevo cliente conectado");
  while (!client.available()) {
    delay(1);
  }

  // Leer la primera línea de la solicitud HTTP
  String request = client.readStringUntil('\r');
  Serial.println(request);
  client.flush();

  // Controlar el LED según la solicitud
  if (request.indexOf("/LED=ON") != -1) {
    digitalWrite(ledPin, HIGH); // Encender el LED
  } 
  if (request.indexOf("/LED=OFF") != -1) {
    digitalWrite(ledPin, LOW);  // Apagar el LED
  }

  // Enviar la respuesta al cliente
  client.println("HTTP/1.1 200 OK");
  client.println("Content-Type: text/html");
  client.println(""); // Separador necesario entre los encabezados y el cuerpo

  // Página HTML
  client.println("<!DOCTYPE HTML>");
  client.println("<html>");
  client.print("El LED esta ahora: ");
  if (digitalRead(ledPin) == HIGH) {
    client.println("Apagado");
  } else {
    client.println("Encendido");
  }

  // Botones para controlar el LED
  client.println("<br><br>");
  client.println("<a href=\"/LED=ON\"><button>Apagar</button></a>");
  client.println("<a href=\"/LED=OFF\"><button>Encender</button></a>");
  client.println("</html>");

  // Cerrar la conexión con el cliente
  delay(1);
  Serial.println("Cliente desconectado");
  Serial.println("");
}
```