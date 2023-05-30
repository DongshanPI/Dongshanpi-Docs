[ [中文](https://dongshanpi.com/DongshanPi-PicoW/01-BoardIntroduction/) | [[Español]](https://dongshanpi.com/DongshanPi-PicoW/01-BoardIntroduction.ES) ]

* El precio de la placa de desarrollo se acerca al costo de producción y solo se proporcionan materiales y documentos de desarrollo de SDK originales de la fábrica, ¡sin ningún soporte técnico adicional!
* Cualquier problema con esta placa de desarrollo se puede discutir y compartir en nuestro foro en https://forums.100ask.net/c/10-category/picow

# Introducción de la placa de desarrollo DongshanPI-PicoW

La placa de desarrollo DongshanPI-PicoW utiliza la combinación de tecnología de StarChip SSD210 y SSW101B usb WiFi, con un diseño de pines de 2,0 mm y agujeros de estampado, y solo necesita una entrada de alimentación de 5V para arrancar el sistema de la placa de desarrollo.

## Introducción a la configuración de parámetros mínimos de la placa

* SigMastar SSD210
  * ARM Cortex-A7 Dual Core 1 GHz con Neon y FPU
  * Máximo de dos interfaces MIPI con 2 o 1 carril de datos y 2 carriles de reloj
  * Salida TTL hasta 1280x800 60fps
  * Entradas MIC y DMIC, I2S TDM de línea de salida de 8 canales, RX de 2/4/8 canales, TX de 2 canales
  * SDIO 2.0 x1
  * USB 2.0 x1
  * 64MB DDR2
  * Ethernet x1
  * Motores de seguridad
  * SPI x2, I2C x2, UART x4, PWM x4
* SSW101B
  * WiFi USB 1T1R de 2,4 GHz
  * IEEE 802.11b/g/n
  * CPU RISC de 32 bits a 150 MHz
  * Admite ancho de banda HT20/40 MHz
* Memoria Flash NAND SPI Winbond 128MB (W25Q128)
* Conmutador USB 2.0 Onseemi (480MBbps) (FSUSB30)

----

Diagrama de la apariencia de la placa de desarrollo
 

![](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanPI-PicoW/DongshanPI-PicoW-TOP.png)



* Esquema de circuito complementario: https://forums.100ask.net/uploads/short-url/jM5L1WocV3O5xZRiRpLYRlEBVNg.pdf
* Imagen del sistema predeterminado: https://dongshanpi.cowtransfer.com/s/639100d687674c



## Tablero inferior de 2,0 mm a 2,54 mm de acompañamiento

Como se muestra en la figura siguiente, suelde la placa principal a la placa de adaptador correspondiente de los pines y suéldela a la placa inferior. Esta placa inferior se puede conectar a una placa perforada y se puede desarrollar a través de 7 líneas + cabeza micro usb + herramienta de conversión usb a serie.


![](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanPI-PicoW/DongshanPI-PicoW-FlashUsb.png)



## Tablero de desarrollo de funciones básicas de acompañamiento

¡Se espera que se lance la próxima semana!

