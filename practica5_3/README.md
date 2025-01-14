# Práctica 5 parte 3

## Código

```c++
#include <Arduino.h>
#include <Wire.h>
#include <AHT10.h>
#include <LiquidCrystal_I2C.h>
#include <Adafruit_Sensor.h>

AHT10 aht10;
LiquidCrystal_I2C lcd(0x27, 16, 2); // Dirección I2C y dimensiones del display LCD

void setup() {
  Serial.begin(115200);
  Wire.begin(); // Inicializa la comunicación I2C
  lcd.init();   // Inicializa el display LCD
  lcd.backlight();
  
  if (!aht10.begin()) {
    Serial.println("Error al inicializar el sensor AHT10");
    while (1);
  }
}

void loop() {
  delay(2000); // Espera 2 segundos entre lecturas
  
  float temp = aht10.readTemperature(); // Lee la temperatura en Celsius
  float hum = aht10.readHumidity();     // Lee la humedad relativa
  
  // Imprime los datos en el monitor serial
  Serial.print("Temperatura: ");
  Serial.print(temp);
  Serial.print(" °C\t");
  Serial.print("Humedad: ");
  Serial.print(hum);
  Serial.println("%");

  // Imprime los datos en el display LCD
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Temp: ");
  lcd.print(temp);
  lcd.print(" C");

  lcd.setCursor(0, 1);
  lcd.print("Humedad: ");
  lcd.print(hum);
  lcd.print("%");
}

```
# Descripción

Este código usa un sensor de temperatura y humedad AHT10 junto con una pantalla LCD para medir y mostrar la temperatura y la humedad ambiente.

Para llevar esto a cabo hemos incluido librerias adicionales, la AHT10.h para el sensor AHT10, l.h para el control de la pantalla LCD mediente I2C y también Adafruit_Sensor que puede ser necesaria por AHT10.h.

Primero hemos declarado una variable para poder manejar el AHT10 y luego hemos creado otra para manejar la pantalla LCD dónde hemos especificado la dirección del I2C y sus dimensiones.

En la función **setup()**, hemos iniciado la velocidad, la comunicación I2C y el display LCD. Después se comprueba que el sensor AHT10 se haya inicializado correctamente, en caso de que no lo haya hecho se imprime un mensaje de error y entra en un bucle infinito.

En la función **loop()** se realiza la lectura de temperatura y humedad cada 2 segundos. Se lee la temperatura y la humedad del AHT10 y se imprimen el monitor serial y luego se limpia la pantalla LCD y se imprimern los datos por ella también.

### Imagenes del montaje y de la pantalla LCD:

<img src="fotomontaje.jpg" style="width: 30%; margin-right: 10px;">
<img src="cables.jpg"  style="width: 30%; margin-right: 10px;">
<img src="LCD.jpg"  style="width: 30%;">

### Diagrama de flujos:

```mermaid
graph TD;
    A[Inicio] --> B[Inicializar];
    B --> C[¿Error al inicializar el sensor AHT10?];
    C -->|Sí| D[Imprimir mensaje de error y detener];
    C -->|No| E[Leer temperatura y humedad];
    E --> F[Imprimir datos en el monitor serial];
    E --> G[Imprimir datos en el display LCD];
    G --> H[Esperar 2 segundos];
    H --> E;
```

## Conclusión

En conclusión, se ha creado un codigo para  ue lea la temperatura y la humedad ambiental utilizando el sensor AHT10 y luego muestre los datos en una pantalla LCD.
