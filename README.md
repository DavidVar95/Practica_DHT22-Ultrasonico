# Practica DHT22 Y ULTRASONICO
Este repositorio muestra la lectura de un dht22 y un ultrasonico y mostrarlo en un LCD.

## Introducción

### Descripción

Conectamos el DHT22 y el sensor ultrasonico HC-SR04 a la ```Esp32``` la utilizamos en un entorno de adquision de datos, obtendremos los datos de humedad temperatura y distancia, una vedz otenida la informacion la enviamos a un LCD para mostrar los datos cada determinado tiempo .


## Material Necesario

Para realizar esta practica necesitas lo siguiente

- [WOKWI](https://https://wokwi.com/)
- Tarjeta ESP 32
- Sensor DHT11
- Sensor ultrasonico HC-SR04 
- LCD 16 X 2 I2C




## Instrucciones

### Requisitos previos

Para poder usar este repositorio necesitas entrar a la plataforma [WOKWI](https://https://wokwi.com/).


### Instrucciones de preparación de entorno 

1. Abrir la terminal de programación y colocar la siguente programación:

```
#include "DHTesp.h"
#include <LiquidCrystal_I2C.h>
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4
const int DHT_PIN = 19;
const int Trigger = 4;   //Pin digital 2 para el Trigger del sensor
const int Echo = 15;   //Pin digital 3 para el Echo del sensor

DHTesp dhtSensor;

LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

void setup() {
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0
  lcd.init();
  lcd.backlight();
  Serial.begin(9600);//iniciailzamos la comunicación
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
 
}

void loop() {
  long t; //timepo que demora en llegar el eco
  long d; //distancia en centimetros

  digitalWrite(Trigger, HIGH);
  delayMicroseconds(10);          //Enviamos un pulso de 10us
  digitalWrite(Trigger, LOW);
  
  t = pulseIn(Echo, HIGH); //obtenemos el ancho del pulso
  d = t/59;             //escalamos el tiempo a una distancia en cm
  
  Serial.print("Distancia: ");
  Serial.print(d);      //Enviamos serialmente el valor de la distancia
  Serial.print("cm");
  Serial.println();
  delay(3000);          //Hacemos una pausa de 100ms

  lcd.setCursor(0, 0);
  lcd.print(" Dist: " + String(d)+"cm ");
  lcd.setCursor(0, 1);
  lcd.print(" Ing.Vargas   ");
delay(1000);

  TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 1) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("---");
  
  lcd.setCursor(0, 0);
  lcd.print("  Temp: " + String(data.temperature, 1) + "\xDF"+"C  ");
  lcd.setCursor(0, 1);
  lcd.print(" Humidity: " + String(data.humidity, 1) + "% ");
  lcd.print("Wokwi Online IoT");

  delay(1000);
  lcd.setCursor(0,0);
  lcd.print("    DAIyM_2023  ");
  delay(1000);
  lcd.setCursor(0,1);
  lcd.print("    VARGAS_95    ");
  delay(1000);

}

```
2. Instalar la libreria de **DHT sensor library for ESPx** y  **LiquidCrystal I2C** como se muestra en la siguente imagen.

![](https://github.com/DavidVar95/Practica_DHT22-Ultrasonico/blob/main/Captura%20de%20pantalla%202023-06-10%2013.30.36.png?raw=true)

3. Hacer la conexion de **DHT11**, **HC-SR04** Y **LCD**con la **ESP32** como se muestra en la siguente imagen.

![](https://github.com/DavidVar95/Practica_DHT22-Ultrasonico/blob/main/Captura%20de%20pantalla%202023-06-10%2013.35.46.png?raw=true)

### Instrucciónes de operación

1. Iniciar simulador.
2. Visualizar los datos en el monitor serial.
3. Colocar la temperatura y humedad dando *doble click* al sensor **DHT11** 
4. Selecionar la distancia en el ultrasonico

## Resultados

la pantalla lcd mostrara los datos de temperatura, humedad y distancia despues de determinado tiempo.

- Distancia

![](https://github.com/DavidVar95/Practica_DHT22-Ultrasonico/blob/main/Captura%20de%20pantalla%202023-06-10%2013.40.10.png?raw=true)

- Temperatura y humedad

![](https://github.com/DavidVar95/Practica_DHT22-Ultrasonico/blob/main/Captura%20de%20pantalla%202023-06-10%2013.41.23.png?raw=true)



# Créditos

Desarrollado por Ing. David Vargas Gonzalez
