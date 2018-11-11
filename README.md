Gateway LoRa Monocanal
==============================
Este repositorio contiene un ejemplo de un Gateway LoRa Mono-Canal de 915MHz para mi proyecto de Telecomunicaciones

Ha sido probado en la plataforma Raspberry Pi, con el chip SX1276 (HopeRF RFM95W).

Parte de este código ha sido copiado del Semtech Packet Forwarder (con el debido permiso).

Primero fue bifurcado de @jlesech https://github.com/tftelkamp/single_chan_pkt_fwd to add json configuration file    
Y luego bifurcado por  @hallard https://github.com/hallard/single_chan_pkt_fwd
Para luego ser bifurcado por @martinvolantin https://github.com/martinvolantin/

El mapa de pines es el siguiente y se encuentra en el archivo `global_conf.json`. Las asignaciones siguientes son de la biblioteca Wiringpi.

```
root@pi04 # gpio readall
+-----+-----+---------+--B Plus--+---------+-----+-----+
| BCM | wPi |   Name  | Physical | Name    | wPi | BCM |
+-----+-----+---------+----++----+---------+-----+-----+
|     |     |    3.3v |  1 || 2  | 5v      |     |     |
|   2 |   8 |   SDA.1 |  3 || 4  | 5V      |     |     |
|   3 |   9 |   SCL.1 |  5 || 6  | 0v      |     |     |
|   4 |   7 | GPIO. 7 |  7 || 8  | TxD     | 15  | 14  |
|     |     |      0v |  9 || 10 | RxD     | 16  | 15  |
|  17 |   0 | GPIO. 0 | 11 || 12 | GPIO. 1 | 1   | 18  |
|  27 |   2 | GPIO. 2 | 13 || 14 | 0v      |     |     |
|  22 |   3 | GPIO. 3 | 15 || 16 | GPIO. 4 | 4   | 23  |
|     |     |    3.3v | 17 || 18 | GPIO. 5 | 5   | 24  |
|  10 |  12 |    MOSI | 19 || 20 | 0v      |     |     |
|   9 |  13 |    MISO | 21 || 22 | GPIO. 6 | 6   | 25  |
|  11 |  14 |    SCLK | 23 || 24 | CE0     | 10  | 8   |
|     |     |      0v | 25 || 26 | CE1     | 11  | 7   |
|   0 |  30 |   SDA.0 | 27 || 28 | SCL.0   | 31  | 1   |
|   5 |  21 | GPIO.21 | 29 || 30 | 0v      |     |     |
|   6 |  22 | GPIO.22 | 31 || 32 | GPIO.26 | 26  | 12  |
|  13 |  23 | GPIO.23 | 33 || 34 | 0v      |     |     |
|  19 |  24 | GPIO.24 | 35 || 36 | GPIO.27 | 27  | 16  |
|  26 |  25 | GPIO.25 | 37 || 38 | GPIO.28 | 28  | 20  |
|     |     |      0v | 39 || 40 | GPIO.29 | 29  | 21  |
+-----+-----+---------+----++----+---------+-----+-----+
| BCM | wPi |   Name  | Physical | Name    | wPi | BCM |
+-----+-----+---------+--B Plus--+---------+-----+-----+
```
* Para configurarlo con la PCB personalizada se deben ajustar en el archivo los siguientes parámetros
```
  "pin_nss": 6,
  "pin_dio0": 7,
  "pin_rst": 0
```
Instalación
------------

Las dependencias se instalan de la siguiente manera.

```shell
cd /home/pi
git clone https://github.com/hallard/single_chan_pkt_fwd
make
sudo make install
````

Para manipular el programa como un servicio mediante systemctl (aunque ya en el paso anterior debe quedar funcionando) se usa el comando
```shell
systemctl start single_chan_pkt_fwd
systemctl stop single_chan_pkt_fwd
systemctl status single_chan_pkt_fwd
````

Para ver la información de debug del gateway en tiempo real se usa el comando
```shell
journalctl -f -u single_chan_pkt_fwd
````

Características
--------
- Escucha en una frecuencia configurable y un factor de separacion ajustable
- Desde SF7 hasta SF12
- Actualizaciones de estado
- Puede enviar datos a dos servidores

Dependencias
------------
- Se debe habilitar SPI en la Raspberry Pi (con raspi-config)
- WiringPi: Una biblioteca escrita en C para controlar el chip BCM2835
  utilizado en la Raspberry Pi
  sudo apt-get install wiringpi
  Ver http://wiringpi.com
- Ejecutar como Root

Connections
-----------
| SX127x | Raspberry PI         |
|--------|----------------------|
| 3.3V   | 3.3V (header pin #1) |
| GND    | GND (pin #6)         |
| MISO   | MISO (pin #21)       |
| MOSI   | MOSI (pin #19)       |
| SCK    | CLK (pin #23)        |
| NSS    | GPIO6 (pin #22)      |
| DIO0   | GPIO7 (pin #7)       |
| RST    | GPIO0 (pin #11)      |

Configuración
-------------

Por defecto:

- LoRa:   SF7 en 903.9 Mhz
- Servidor: router.eu.staging.thethings.network, puerto 1700

Licencia
-------
Los archivos listados en este repositorio se adecúan a la licencia Eclipse Public License v1.0, excepto:
- La implementación de base64, que fue copiada de Semtech Packet Forwarder;
- RapidJSON, licenciado bajo la MIT License.


[1]: https://github.com/hallard/LoRasPI
[2]: http://wiki.dragino.com/index.php?title=Lora/GPS_HAT
[3]: https://github.com/hallard/RPI-Lora-Gateway
 

