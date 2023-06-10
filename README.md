# Practica ESP32 con sensor ultrasónico de distancia y DHT22
En este repositorio se realizó una práctica utilizando la página Wokwi. En esta simulación se adicionaron un sensor de temperatura y humedad **DHT22**, un sensor **ultrasónico de distancia** y una **LCD** para la muestra de datos.

## Introducción

### Descripción

En esta simulación se muestran en una **LCD** los datos recopilados por los sensores **DHT** y de **Temperatura y humedad**. En esta práctica se usará nuevamente el simulador [WOKWI](https://wokwi.com/).


## Material Necesario

Para realizar esta práctica se ocuparon las siguientes herramientas y componentes:

- [SIMULADOR WOKWI](https://wokwi.com/)
- Tarjeta ESP32
- Sensor ultrasónico de distancia
- LCD 16x2 (I2C)
- Sensor **DHT22**



## Instrucciones

### Requisitos previos

Para poder usar este repositorio necesitas entrar a la plataforma [WOKWI](https://wokwi.com/).


### Instrucciones de preparación de entorno 

1. Al ingresar a la página de [WOKWI](https://wokwi.com/) se selecciona la tarjeta ESP32, se agrega el sensor ultrasónico de distancia, una LCD de 16x2 y un sensor **DHT22**.

2. Una vez seleccionado la tarjeta ```Esp32``` junto a los componentes, en la parte izquierda se encuentra la pestaña de código donde se agrega lo siguiente:

```
#include <LiquidCrystal_I2C.h>
#include "DHTesp.h"

#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4

const int Trigger = 13;   //Pin digital 2 para el Trigger del sensor
const int Echo = 12;   //Pin digital 3 para el Echo del sensor
const int DHT_PIN = 15;

DHTesp dhtSensor;
LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

void setup() {
  Serial.begin(9600);//iniciailzamos la comunicación
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0
  lcd.init();
  lcd.backlight();
}

void loop()
{
  TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 1) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("---");
  
  lcd.setCursor(0,0);
  lcd.print("Diplomado AIyM");
  lcd.setCursor(0,1);
  lcd.print("Michelle C. G.");
  delay(1500);

  lcd.setCursor(0, 0);
  lcd.print("  Temp: " + String(data.temperature, 1) + "\xDF"+"C  ");
  lcd.setCursor(0, 1);
  lcd.print(" Humidity: " + String(data.humidity, 1) + "% ");
  lcd.print("Wokwi Online IoT");
  delay(3000);

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
  delay(1000);          //Hacemos una pausa de 100ms

  lcd.setCursor(0,0);
  lcd.print("Distancia:      ");
  lcd.setCursor(0,1);
  lcd.print(String(d) + "  cm         ");
  delay(3000);

}

``` 
3.En la pestaña de *Library Manager*, instalar la librería de **DHT sensor library for ESPx** y **LiquidCrystal I2C** como se muestra en la siguente imagen.

![](https://github.com/Michellecg/DHT22_y_sensor_ultrasonico/blob/main/Lib_Ult_Temp.PNG)

4. Hacer la conexión del **sensor ultrasónico de distancia**, **DHT22** y **LCD de 16x2** a la tarjeta **ESP32** como se muestra en la siguente imagen.

![](https://github.com/Michellecg/DHT22_y_sensor_ultrasonico/blob/main/Conex_sens.PNG)

### Instrucciónes de operación

1. Iniciar simulador.
2. Visualizar los datos en el monitor serial.
3. Modificar la distancia dando *doble click* al sensor **ultrasónico de distancia**
4. Modificar la temperatura y humedad dando *doble click* al sensor **DHT22**


## Resultados

Una vez compilado el programa y que no se hayan presentado errores, se podrán visualizar los datos que recopilan cada sensor en la LCD.

![](https://github.com/Michellecg/DHT22_y_sensor_ultrasonico/blob/main/Sen_Tem.PNG)

![](https://github.com/Michellecg/DHT22_y_sensor_ultrasonico/blob/main/Sen_Dist.PNG)


# Créditos

Desarrollado por Michelle Cuatlapantzi González

- [GitHub](https://github.com/Michellecg/)