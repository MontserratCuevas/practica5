# Práctica 5 parte 1

## Código:

```c++

    #include <Arduino.h>

    #include <Wire.h>

    void setup()
    {
    Wire.begin();
    Serial.begin(115200);
    while (!Serial);
    // Leonardo: wait for serial monitor
    Serial.println("\nI2C Scanner");
    }

    void loop()
    {
    byte error, address;

    int nDevices;
    Serial.println("Scanning...");
    nDevices = 0;
    for(address = 1; address < 127; address++ )
    {
    // The i2c_scanner uses the return value of
    // the Write.endTransmisstion to see if
    // a device did acknowledge to the address.
    Wire.beginTransmission(address);
    error = Wire.endTransmission();
    if (error == 0)
    {
    Serial.print("I2C device found at address 0x");
    if (address<16)
    Serial.print("0");
    Serial.print(address,HEX);
    Serial.println(" !");
    nDevices++;
    }
    else if (error==4)
    {
    Serial.print("Unknown error at address 0x");
    if (address<16)
    Serial.print("0");
    Serial.println(address,HEX);
    }
    }
    if (nDevices == 0)
    Serial.println("No I2C devices found\n");
    else
    Serial.println("done\n");
    delay(5000);
    // wait 5 seconds for next scan
    }

 ```


## Descripción:

Con este código pretendemos hacer un escaner I2C para dispositivos conectados a un bus I2C.

Para llevar a cabo su propósito se ha incluido la libreria Wire.h y se han realizado los siguientes pasos:

Inicializa la comunicación serial y el bus I2C en el **setup()**.

En el **loop()** escanea todas las direcciones posibles del bus y verifica si hay dispositivos conectados. Muestra un mensaje conforme está escanenando.


Al final del escaneo si se han encontrado dispositivos imprime un mensaje que dice que se ha encontrado un dispositivo y la dirección que le corresponde en hexadecimal.

Si por lo contrario no ha encontrado ningún dispositivo se imprimirá un mensaje diciendo que no se ha encontrado ningun dispositivo.

Si se han encontrado dispositivos con éxito imprime un mensaje confrome está hecho y espera 5 segundos antes de iniciar el siguiente escaneo.

En nuestro caso por el puerto serie nos ha salido el siguiente mensaje cada 5 segundos:

    Scanning...
    I2C device found at address 0x27 !
    done



## Conclusión

Este codigo nos permite identificar los dispositivos conectados a un bus I2C y nos puede ser útil si queremos saber los dispositivos que hay presentres en la red I2C.