# INTERFAZ-II
##### Introducción a processing y arduino para el desarrollo de una interfaz interactiva 
1. [Hola mundo](#ejercicio-n1-hola-mundo) <br>
2. [Led intermitente](#ejercicio-n2-led-intermitente) <br>
3. [Control por pulsador](#ejercicio-3-control-por-pulsador) <br>
4. [Led con potenciador](#ejercicio-4-led-con-potenciometro) <br>
5. [Semaforo en arduino](#ejercicio-4-led-con-potenciometro) <br>
6. [Arduino processing](#ejercicio-6-arduino-processing) <br>
7. [Arduino+boton+prossecing](#ejercicio-7-arduino--boton--processing) <br>
8. [Arduino+boton+potenciador+processing](#ejercicio-n8-arduino--bot%C3%B3n--potenciometro--processing) <br>
9. [Botonera+audio](#ejercicio-n9-botonera--audio) <br>
10. [Botonera](#ejercicio-n10-botonera) <br>
11. [Sensor Sharp](

#### EJERCICIO n°1: hola mundo!

```js
void setup() {
  Serial.begin(9600); // Inicia la comunicación serie a 9600 bps
  Serial.println("Hola, Mundo!"); // Envía "Hola, Mundo!" al monitor serie
}

void loop() {
  // No es necesario poner nada en el loop para este ejemplo
}
```

### EJERCICIO N°2: LED INTERMITENTE 
 ```js
void setup() {  // Configuración inicial (ej: pines como entrada/salida)
  pinMode(13, OUTPUT);  // Pin 13 como salida
  pinMode(8, OUTPUT);
    
}

void loop() {   // Se repite infinitamente
  digitalWrite(13, HIGH);  // Encender LED
  delay(1000);             // Esperar 1 segundo
  digitalWrite(13, LOW);   // Apagar LED
  delay(1000);             // Esperar 1 segundo

 digitalWrite(8, HIGH);  // Encender LED
  delay(1000);             // Esperar 1 segundo
  digitalWrite(8, LOW);   // Apagar LED
  delay(1000); 
  
}
```
<img src="https://raw.githubusercontent.com/crojasj-prog/INTERFAZ-II/refs/heads/main/img/led%20intermitente.png"/>
<img src="https://raw.githubusercontent.com/crojasj-prog/INTERFAZ-II/refs/heads/main/img/led%20intermitente.jpg">

### EJERCICIO 3: Control por Pulsador
```js
void setup() {
  pinMode(2, INPUT);  // Botón como entrada
  pinMode(13, OUTPUT);
}
void loop() {
  if (digitalRead(2) == HIGH) {  // Si se presiona el botón
    digitalWrite(13, HIGH);
  } else {
    digitalWrite(13, LOW);
  }

}
```
<img src="https://raw.githubusercontent.com/crojasj-prog/INTERFAZ-II/refs/heads/main/img/led%20con%20pulsador.png"/>


### EJERCICIO 4: LED CON POTENCIOMETRO
```js
void setup() {
  pinMode(9, OUTPUT);  // Pin PWM (símbolo ~)
}
void loop() {
  int valor = analogRead(A0);           // Leer potenciómetro (0-1023)
  int brillo = map(valor, 0, 1023, 0, 255);  // Convertir a rango PWM
  analogWrite(9, brillo);               // Ajustar brillo
}
```
<img src="https://github.com/user-attachments/assets/46fd35e7-b9b6-4c64-b279-0dbb6210bfd6"/>

### EJERCICIO 5: SEMAFORO EN ARDUINO
``` js
 // C++ code - Semáforo Autos y Peatones

// Definición de pines
int LED_1 = 6;  // Luz roja autos
int LED_2 = 7;  // Luz amarilla autos
int LED_3 = 8;  // Luz verde autos
int LED_4 = 9;  // Luz verde peatones
int LED_5 = 10; // Luz roja peatones

void setup() {
  // Configuramos todos los pines como salida
  pinMode(LED_1, OUTPUT);
  pinMode(LED_2, OUTPUT);
  pinMode(LED_3, OUTPUT);
  pinMode(LED_4, OUTPUT);
  pinMode(LED_5, OUTPUT);
}

void loop() {
  // 🚦 Fase 1: Autos en verde, peatones en rojo
  digitalWrite(LED_1, LOW);   // Rojo autos apagado
  digitalWrite(LED_2, LOW);   // Amarillo autos apagado
  digitalWrite(LED_3, HIGH);  // Verde autos encendido
  digitalWrite(LED_4, LOW);   // Verde peatones apagado
  digitalWrite(LED_5, HIGH);  // Rojo peatones encendido
  delay(5000); // 5 segundos

  // 🚦 Fase 2: Amarillo autos, peatones siguen en rojo
  digitalWrite(LED_3, LOW);   // Verde autos apagado
  digitalWrite(LED_2, HIGH);  // Amarillo autos encendido
  delay(2000); // 2 segundos
  digitalWrite(LED_2, LOW);   // Amarillo autos apagado

  // 🚦 Fase 3: Rojo autos, verde peatones
  digitalWrite(LED_1, HIGH);  // Rojo autos encendido
  digitalWrite(LED_5, LOW);   // Rojo peatones apagado
  digitalWrite(LED_4, HIGH);  // Verde peatones encendido
  delay(5000); // 5 segundos

  // 🚦 Fase 4: Rojo autos, rojo peatones (tiempo intermedio)
  digitalWrite(LED_4, LOW);   // Verde peatones apagado
  digitalWrite(LED_5, HIGH);  // Rojo peatones encendido
  delay(2000); // 2 segundos
}
```
<img src="https://raw.githubusercontent.com/crojasj-prog/INTERFAZ-II/refs/heads/main/img/semaforo.png"/> 

#### EJERCICIO 6: Arduino processing 
```Js

codigo arduino
```js
unsigned int ADCValue;
void setup(){
    Serial.begin(9600);
}

void loop(){

 int val = analogRead(0);
   val = map(val, 0, 300, 0, 255);
    Serial.println(val);
delay(50);
}
```js

codigo processing
```js

import processing.serial.*;

Serial myPort;  // Crear objeto de la clase Serial
static String val;    // Datos recibidos desde el puerto serial
int sensorVal = 0;

void setup()
{
  background(0); 
  //fullScreen(P3D);
   size(1080, 720);
   noStroke();
  noFill();
  String portName = "COM3";// Cambia el número (en este caso) para que coincida con el puerto correspondiente conectado a tu Arduino. 

  //myPort = new Serial(this, "/dev/cu.usbmodem1101", 9600);
  myPort = new Serial(this, Serial.list()[0], 9600);

}

void draw()
{
  if ( myPort.available() > 0) {  // Si hay datos disponibles,
  val = myPort.readStringUntil('\n'); 
  try {
   sensorVal = Integer.valueOf(val.trim());
  }
  catch(Exception e) {
  ;
  }
  println(sensorVal); // léelos y guárdalos en vals!
  }  
 //background(0);
  // Escala el valor de mouseX de 0 a 640 a un rango entre 0 y 175
  float c = map(sensorVal, 0, width, 0, 400);
  // Escala el valor de mouseX de 0 a 640 a un rango entre 40 y 300
  float d = map(sensorVal, 0, width, 40,500);
  fill(255, c, 0);
  ellipse(width/2, height/2, d, d);   
}

```

#### EJERCICIO 7: Arduino + Boton + Processing 
```Js
  
  CODIGO ARDUINO
```js

  int buttonPin = 2;  // Pin del botón
int buttonState = 0;

void setup() {
  pinMode(buttonPin, INPUT_PULLUP); // Botón con resistencia interna
  Serial.begin(9600);
}

void loop() {
  buttonState = digitalRead(buttonPin);

  if (buttonState == HIGH) {   // Botón presionado
    Serial.println(1);        // Enviar un "1" a Processing
    delay(200);               // Evitar rebotes
  }
}
```js
CODIGO PROCESSING 

```js
import processing.serial.*;

Serial myPort;
ArrayList<PVector> circles; 

void setup() {
  size(1920, 1080);
  background(0);
  
  // Ajusta el nombre del puerto según tu Arduino
  println(Serial.list());
  myPort = new Serial(this, "/dev/cu.usbmodem1101", 9600);
  //myPort = new Serial(this, Serial.list()[0], 9600);
  
  circles = new ArrayList<PVector>();
}

void draw() {
  //background(0);
  
  // Dibujar círculos almacenados
  fill(0, 0, 0);
  //noStroke();
  stroke(255, 0, 0);
  for (PVector c : circles) {
    ellipse(c.x, c.y, 30, 30);
  }
  
  // Revisar si llega algo de Arduino
  if (myPort.available() > 0) {
    String val = myPort.readStringUntil('\n');
    if (val != null) {
      val = trim(val);
      if (val.equals("1")) {
        // Cada vez que se aprieta el botón, agregar un círculo en posición aleatoria
        circles.add(new PVector(random(width), random(height)));
      }
    }
  }
}

```
#### EJERCICIO N°8: Arduino + botón + potenciometro + Processing
```Js
Codigo arduino
```js
int buttonPin = 2;       // Pin del botón
int potPin = A0;         // Pin del potenciómetro
int buttonState = 0;

void setup() {
  pinMode(buttonPin, INPUT_PULLUP); // Botón con resistencia interna
  Serial.begin(9600);
}

void loop() {
  buttonState = digitalRead(buttonPin);

  if (buttonState == HIGH) {   // Botón presionado
    int potValue = analogRead(potPin);   // 0 - 1023
    Serial.print("BTN,");     // etiqueta para Processing
    Serial.println(potValue); // mando el valor junto con el evento
    delay(200);               // debounce simple
  }
}
```js
Codigo processing
```js
import processing.serial.*;

Serial myPort;
ArrayList<CircleData> circles; 

void setup() {
  size(1200, 720);
  background(0);
  
  // Ajusta el puerto según tu Arduino
  println(Serial.list());
  myPort = new Serial(this, "/dev/cu.usbmodem1101", 9600);
  //myPort = new Serial(this, Serial.list()[0], 9600);
  
  circles = new ArrayList<CircleData>();
}

void draw() {
  //background(0);
  
  // Dibujar todos los círculos guardados
  fill(#F77AC3);
  //noStroke();
  fill(#F77AC3);
  stroke(0, 0, 0);
  for (CircleData c : circles) {
    ellipse(c.x, c.y, c.size, c.size);
  }
  
  // Leer datos de Arduino
  if (myPort.available() > 0) {
    String val = myPort.readStringUntil('\n');
    if (val != null) {
      val = trim(val);
      if (val.startsWith("BTN")) {
        // Extraer el valor del potenciómetro
        String[] parts = split(val, ',');
        if (parts.length == 2) {
          float potVal = float(parts[1]);
          float circleSize = map(potVal, 0, 1023, 10, 100); // tamaño 10-100 px
          circles.add(new CircleData(random(width), random(height), circleSize));
        }
      }
    }
  }
}

// Clase para guardar datos de cada círculo
class CircleData {
  float x, y, size;
  CircleData(float x, float y, float size) {
    this.x = x;
    this.y = y;
    this.size = size;
  }
}
```
#### EJERCICIO N°9: BOTONERA + AUDIO 
```js
Codigo Arduino
```js
// --- Configuración de botones ---
const int numButtons = 3;
const int buttonPins[numButtons] = {2, 4, 7};
const int ledButtonPins[numButtons] = {9, 10, 11}; // LEDs botones

// --- Configuración de potenciómetros ---
const int numPots = 2;
const int potPins[numPots] = {A0, A1};
const int ledPotPins[numPots] = {3, 5}; // LEDs PWM

// Variables de estados previos
int lastButtonState[numButtons];
int lastPotValue[numPots];

void setup() {
  Serial.begin(9600);

  // Configurar botones y LEDs
  for (int i = 0; i < numButtons; i++) {
    pinMode(buttonPins[i], INPUT_PULLUP);
    pinMode(ledButtonPins[i], OUTPUT);
    lastButtonState[i] = digitalRead(buttonPins[i]);
  }

  // Configurar LEDs de potenciómetros
  for (int i = 0; i < numPots; i++) {
    pinMode(ledPotPins[i], OUTPUT);
    lastPotValue[i] = analogRead(potPins[i]);
  }
}

void loop() {
  // Leer y enviar botones
  for (int i = 0; i < numButtons; i++) {
    int buttonState = digitalRead(buttonPins[i]);

    // LED se enciende cuando botón está presionado
    digitalWrite(ledButtonPins[i], buttonState == LOW ? HIGH : LOW);

    if (buttonState != lastButtonState[i]) {  // enviar cambios
      Serial.print("B");
      Serial.print(i); 
      Serial.print(":");
      Serial.println(buttonState);
      lastButtonState[i] = buttonState;
    }
  }

  // Leer y enviar potenciómetros
  for (int i = 0; i < numPots; i++) {
    int potValue = analogRead(potPins[i]); // 0–1023
    int pwmValue = potValue / 4;           // 0–255

    // Ajustar LED según valor
    analogWrite(ledPotPins[i], pwmValue);

    if (abs(pwmValue - lastPotValue[i]) > 2) { // evitar ruido
      Serial.print("P");
      Serial.print(i);
      Serial.print(":");
      Serial.println(pwmValue);
      lastPotValue[i] = pwmValue;
    }
  }

  delay(10);
```js
Codigo Processing
```js
// Importamos librería para comunicación serial
import processing.serial.*;
// Importamos librería Minim para manejar audio
import ddf.minim.*;

// Declaramos el objeto serial para comunicarnos con Arduino
Serial myPort;
// Objeto principal de Minim
Minim minim;
// Array de reproductores de audio (3 pistas)
AudioPlayer[] players;
// Variable para guardar el índice de la pista que está sonando
int currentTrack = -1;  // -1 significa que no hay pista activa al inicio

void setup() {
  size(400, 200); // Ventana de 400x200 píxeles
  
  // --- Configuración del puerto serial ---
  printArray(Serial.list()); // Muestra en consola la lista de puertos disponibles
  myPort = new Serial(this, Serial.list()[0], 9600); // Abrimos el primer puerto de la lista a 9600 baudios
  
  // --- Configuración de audio ---
  minim = new Minim(this); // Inicializamos Minim
  players = new AudioPlayer[3]; // Creamos un array de 3 reproductores
  
  // Cargamos los 3 archivos de audio desde la carpeta "data"
  players[0] = minim.loadFile("pipipi.mp3", 2048); 
  players[1] = minim.loadFile("estrellita.mp3", 2048); 
  players[2] = minim.loadFile("boowomp.mp3", 2048); 
}

void draw() {
  background(0); // Fondo negro
  fill(255);     // Color blanco para el texto
  textSize(16);  // Tamaño del texto
  
  // Mostramos en pantalla qué botón está activo
  text("Botón actual: " + (currentTrack == -1 ? "ninguno" : currentTrack), 20, 40);
}

void serialEvent(Serial myPort) {
  // Leemos la cadena que llega desde Arduino hasta el salto de línea
  String inString = trim(myPort.readStringUntil('\n'));
  
  // Si no llega nada, salimos
  if (inString == null) return;

  // --- Si el mensaje recibido empieza con "B" significa que es un botón ---
  if (inString.startsWith("B")) {
    // Quitamos la letra "B" y separamos el mensaje en partes (ejemplo "0:0")
    String[] parts = split(inString.substring(1), ':');
    
    // Si realmente recibimos dos partes (índice y estado)
    if (parts.length == 2) {
      int buttonIndex = int(parts[0]); // Número del botón (0,1,2)
      int state = int(parts[1]);       // Estado del botón (0 = presionado, 1 = suelto)
      
      // Si el botón fue presionado (LOW = 0 en Arduino)
      if (state == 0) { 
        playTrack(buttonIndex); // Llamamos a la función para reproducir la pista correspondiente
      }
    }
  }
}

// --- Función que reproduce una pista según el botón ---
void playTrack(int index) {
  // Si ya había una pista sonando, la pausamos y la rebobinamos al inicio
  if (currentTrack != -1 && players[currentTrack].isPlaying()) {
    players[currentTrack].pause();
    players[currentTrack].rewind();
  }
  
  // Reproducimos en bucle la pista seleccionada
  players[index].loop();
  
  // Actualizamos la variable para saber cuál es la pista activa
  currentTrack = index;
}
```
#### EJERCICIO N°10: BOTONERA 
  ```Js

Código para Arduino.
```js

// --- Configuración de botones ---
const int numButtons = 3;
const int buttonPins[numButtons] = {2, 4, 7};
const int ledButtonPins[numButtons] = {9, 10, 11}; // LEDs botones

// --- Configuración de potenciómetros ---
const int numPots = 2;
const int potPins[numPots] = {A0, A1};
const int ledPotPins[numPots] = {3, 5}; // LEDs PWM

// Variables de estados previos
int lastButtonState[numButtons];
int lastPotValue[numPots];

void setup() {
  Serial.begin(9600);

  // Configurar botones y LEDs
  for (int i = 0; i < numButtons; i++) {
    pinMode(buttonPins[i], INPUT_PULLUP);
    pinMode(ledButtonPins[i], OUTPUT);
    lastButtonState[i] = digitalRead(buttonPins[i]);
  }

  // Configurar LEDs de potenciómetros
  for (int i = 0; i < numPots; i++) {
    pinMode(ledPotPins[i], OUTPUT);
    lastPotValue[i] = analogRead(potPins[i]);
  }
}

void loop() {
  // Leer y enviar botones
  for (int i = 0; i < numButtons; i++) {
    int buttonState = digitalRead(buttonPins[i]);

    // LED se enciende cuando botón está presionado
    digitalWrite(ledButtonPins[i], buttonState == LOW ? HIGH : LOW);

    if (buttonState != lastButtonState[i]) {  // enviar cambios
      Serial.print("B");
      Serial.print(i); 
      Serial.print(":");
      Serial.println(buttonState);
      lastButtonState[i] = buttonState;
    }
  }

  // Leer y enviar potenciómetros
  for (int i = 0; i < numPots; i++) {
    int potValue = analogRead(potPins[i]); // 0–1023
    int pwmValue = potValue / 4;           // 0–255

    // Ajustar LED según valor
    analogWrite(ledPotPins[i], pwmValue);

    if (abs(pwmValue - lastPotValue[i]) > 2) { // evitar ruido
      Serial.print("P");
      Serial.print(i);
      Serial.print(":");
      Serial.println(pwmValue);
      lastPotValue[i] = pwmValue;
    }
  }

  delay(10);
}
```
<img src="https://raw.githubusercontent.com/crojasj-prog/INTERFAZ-II/refs/heads/main/img/botonera.png">

#### CIRCUITO PERSONAL: 3 LUCES LED + POTENCIADOR 
 ``` Js
 void setup() {
  pinMode(7, OUTPUT); 
  pinMode(9, OUTPUT);
  pinMode(10, OUTPUT);   // Pin PWM (símbolo ~)
}

void loop() {
  int valor = analogRead(A0);           // Leer potenciómetro (0-1023)
  int brillo = map(valor, 0, 1023, 0, 255);  // Convertir a rango PWM
 analogWrite(7, brillo);    
 analogWrite(9, brillo);   
 analogWrite(10, brillo);          // Ajustar brillo
}
```
<img src="https://raw.githubusercontent.com/crojasj-prog/INTERFAZ-II/refs/heads/main/img/3%20led%20%2B%20potenciador.png">

<img src="https://raw.githubusercontent.com/crojasj-prog/INTERFAZ-II/refs/heads/main/img/registro%20circuito%201.jpg">

<img src="https://raw.githubusercontent.com/crojasj-prog/INTERFAZ-II/refs/heads/main/img/registro%20circuito%202.jpg">

##### EJERCICIO 10: SENSOR SHARP
##### Código arduino 
```js
// Definir el pin del sensor Sharp
int sharpPin = A0;

void setup() {
  Serial.begin(9600); // Iniciar comunicación serial
}

void loop() {
  int sensorValue = analogRead(sharpPin); // Leer valor del sensor
  Serial.println(sensorValue); // Enviar valor a Processing
  delay(100); // Esperar un momento
}
```
 ##### Código processing 
 ```js
import processing.serial.*;

Serial myPort;  // Create object from Serial class
static String val;    // Data received from the serial port
int sensorVal = 0;

void setup()
{
  background(0); 
  //fullScreen(P3D);
   size(1080, 720);
   noStroke();
  noFill();
  String portName = "COM5";// Change the number (in this case ) to match the corresponding port number connected to your Arduino. 

  myPort = new Serial(this, Serial.list()^[0], 9600);
}

void draw()
{
  if ( myPort.available() > 0) {  // If data is available,
  val = myPort.readStringUntil('\n'); 
  try {
   sensorVal = Integer.valueOf(val.trim());
  }
  catch(Exception e) {
  ;
  }
  println(sensorVal); // read it and store it in vals!
  }  
 //background(0);
  // Scale the mouseX value from 0 to 640 to a range between 0 and 175
  float c = map(sensorVal, 0, width, 0, 400);
  // Scale the mouseX value from 0 to 640 to a range between 40 and 300
  float d = map(sensorVal, 0, width, 40,500);
  fill(255, c, 0);
  ellipse(width/2, height/2, d, d);   

}
```
##### EJERCICIO 11: Sensor de humedad (DFRobot)
##### Código arduino 
```js
void setup()
{
  Serial.begin(9600);// abre el puerto serial y Establece la velocidad en baudios a 9600 bps
}
void loop()
{
  int sensorValue;
  sensorValue = analogRead(0);   //conectar el sensor de humedad al pin analogo 0
  Serial.println(sensorValue); //imprime el valor a serial.
  delay(200);
}
```

#### Ejercicio nota 2: 
#### codigo arduino
```js
void setup() {
  Serial.begin(9600);
}

void loop() {
  int potValue = analogRead(A0);
  Serial.println(potValue);
  delay(20);
}

```
#### codigo processesing:
```js
// --- Librerías ---
import processing.serial.*;
import java.io.File;

// --- Variables globales ---
Serial myPort;
float potValue = 0;

// Suavizado del sensor
float smoothedValue = 0;
float smoothFactor = 0.02;

// Imágenes
PImage[] imgs;
PImage avgImg;

// Rango dinámico del sensor con inicialización segura
float minSensor = 0;
float maxSensor = 1023;

// --- Setup inicial ---
void setup() {
  size(745, 1024);

  // Cargar imágenes
  imgs = loadImagesFromFolder("imagenes");
  println("Imágenes cargadas: " + imgs.length);

  // Redimensionar imágenes
  for (int i = 0; i < imgs.length; i++) {
    imgs[i].resize(width, height);
  }

  // Imagen promedio
  avgImg = createImage(width, height, RGB);

  // Configurar puerto serial
  printArray(Serial.list()); // Muestra los puertos disponibles
  myPort = new Serial(this, Serial.list()[0], 9600); // usa el primer puerto
}

// --- Draw principal ---
void draw() {
  background(0);
  readSerial();

  if (imgs == null || imgs.length < 2) return;

  // Suavizado del sensor
  smoothedValue = lerp(smoothedValue, potValue, smoothFactor);

  // Actualizar rango dinámico solo si el valor nuevo supera los límites actuales
  if (smoothedValue < minSensor) minSensor = smoothedValue;
  if (smoothedValue > maxSensor) maxSensor = smoothedValue;

  // Evitar divisor cero
  float range = maxSensor - minSensor;
  if (range < 1) range = 1;

  // Mapear valor suavizado al rango de imágenes
  float mixValue = map(smoothedValue, minSensor, maxSensor, 0, imgs.length - 1);
  mixValue = constrain(mixValue, 0, imgs.length - 1);

  // Calcular imagen promedio
  avgImagesWeighted(mixValue);

  // Mostrar imagen
  image(avgImg, 0, 0);

  // Mostrar info en pantalla
  fill(255);
  text("Valor sensor: " + nf(smoothedValue, 1, 1), 10, height - 30);
  text("MIN: " + nf(minSensor,1,1) + " MAX: " + nf(maxSensor,1,1), 10, height - 10);
}

// --- Función de promedio ponderado entre imágenes ---
void avgImagesWeighted(float mix) {
  avgImg.loadPixels();

  mix = constrain(mix, 0, imgs.length - 1);

  int i1 = floor(mix);
  int i2 = min(i1 + 1, imgs.length - 1);
  float t = mix - i1;

  imgs[i1].loadPixels();
  imgs[i2].loadPixels();

  for (int i = 0; i < avgImg.pixels.length; i++) {
    color c1 = imgs[i1].pixels[i];
    color c2 = imgs[i2].pixels[i];

    float r = lerp(red(c1), red(c2), t);
    float g = lerp(green(c1), green(c2), t);
    float b = lerp(blue(c1), blue(c2), t);

    avgImg.pixels[i] = color(r, g, b);
  }

  avgImg.updatePixels();
}

// --- Leer datos del sensor ---
void readSerial() {
  while (myPort != null && myPort.available() > 0) {
    String val = myPort.readStringUntil('\n');
    if (val != null) {
      val = trim(val);
      if (val.length() > 0) {
        potValue = float(val);
      }
    }
  }
}

// --- Cargar todas las imágenes desde la carpeta ---
PImage[] loadImagesFromFolder(String folderName) {
  String path = sketchPath("data/" + folderName);
  File folder = new File(path);
  File[] files = folder.listFiles();

  if (files == null) {
    println("Carpeta no encontrada: " + path);
    return null;
  }

  ArrayList<PImage> loaded = new ArrayList<PImage>();
  for (File f : files) {
    String fname = f.getName().toLowerCase();
    if (fname.endsWith(".jpg") || fname.endsWith(".png")) {
      PImage img = loadImage(folderName + "/" + f.getName());
      if (img != null) loaded.add(img);
    }
  }

  return loaded.toArray(new PImage[loaded.size()]);
}
```
<img src="https://raw.githubusercontent.com/crojasj-prog/INTERFAZ-II/refs/heads/main/img/entrega%202.jpg"> 



