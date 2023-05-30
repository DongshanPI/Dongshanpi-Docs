[ [中文](https://dongshanpi.com/DongshanPi-PicoW/03-1_FlashSystem/) | [[Español]](https://dongshanpi.com/DongshanPi-PicoW/03-1_FlashSystem.ES/) ]

# Actualización de la imagen del sistema

## Grabación mediante USB

DongshanPI-PicoW es una placa de desarrollo extremadamente pequeña que puede funcionar directamente con una entrada de voltaje de 5V. Puede hacer referencia a las cuatro flechas en la imagen de la derecha a continuación y conectar un módulo Micro USB a:

* `P36 USB_DM` conectado a la señal `D-` del módulo MicroUSB.
* `P35 USB_DP` conectado a la señal `D+` del módulo MicroUSB.
* `P34 GND` conectado a la señal `GND` del módulo MicroUSB.
* `P31 5V` conectado a la señal `VBUS` del módulo MicroUSB.

**Nota: Si anteriormente estaba utilizando una alimentación de 5V a través de un puerto serie, debe desconectar primero la alimentación de 5V del puerto serie antes de conectar el pin 5V del MicroUSB en esta sección.**

**Nota: Si anteriormente estaba utilizando una alimentación de 5V a través de un puerto serie, debe desconectar primero la alimentación de 5V del puerto serie antes de conectar el pin 5V del MicroUSB en esta sección.**

**Nota: Si anteriormente estaba utilizando una alimentación de 5V a través de un puerto serie, debe desconectar primero la alimentación de 5V del puerto serie antes de conectar el pin 5V del MicroUSB en esta sección.**

**Nota: Asegúrese de conectar correctamente la placa de desarrollo con los pines VBUS y GND del módulo MicroUSB en los pines P31 y P34 de la placa de desarrollo respectivamente antes de encenderla, de lo contrario, se puede dañar el hardware, lo cual no es responsabilidad nuestra.**



![](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanPI-PicoW/DongshanPI-PicoW-TOPUSBLine.png)



### 1. Conexión del MicroUSB a la placa de desarrollo

La siguiente imagen muestra cómo se conectan en la realidad las cuatro líneas Dupont en la esquina inferior izquierda con el módulo MicroUSB verde. Asegúrese de conectar correctamente la placa de desarrollo con los pines VBUS y GND del módulo MicroUSB en los pines P31 y P34 de la placa de desarrollo respectivamente antes de encenderla, de lo contrario, se puede dañar el hardware, lo cual no es responsabilidad nuestra.

![](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanPI-PicoW/DongshanPI-PicoW-FlashUsb.png)



### 2. Ejecución de la herramienta de grabación

Después de asegurarse de que la placa de desarrollo y el módulo Micro USB estén conectados correctamente como se indicó en los pasos anteriores, obtenga la herramienta de grabación y la imagen correspondiente.


1. Descargue la imagen predeterminada y la herramienta de grabación USB desde el siguiente enlace: https://dongshanpi.cowtransfer.com/s/639100d687674c. Descargue el archivo DongshanPI-PicoW_DefaultImage.zip y descomprímalo después de la descarga. Después de la descompresión, encontrará los siguientes archivos en la carpeta, donde **USBDownloadTool.exe** es la herramienta de grabación y los demás archivos son archivos de imagen del sistema requeridos, etc.

``` bash
 AitUVCExtApi.dll
 USBDownloadTool.exe
 appconfigs.ubifs
 auto_update.txt
 auto_update_bin.txt
 boot/
 boot.bin
 cis.bin
 customer.ubifs
 kernel
 misc.fwfs
 miservice.ubifs
 partition_layout.txt
 rootfs.sqfs
 scripts/
 scripts_bin/
 u-boot.bin
 usb_updater.bin
 usb_updater_boot.bin
 usb_updater_ipl.bin
```

2. Haga doble clic en el software **USBDownloadTool.exe** para abrirlo. Se abrirá un cuadro de diálogo como se muestra en la siguiente imagen. A continuación, siga los pasos posteriores para entrar en el modo de grabación USB mediante el cortocircuito de la placa `GND M4` para comenzar la operación de grabación.

![](https://jsd.cdn.zzko.cn/gh/DongshanPI/Docs-Photos@master/DongshanPI-PicoW/DongshanPI-PicoW-UsbDownTools.png)

### 3. Acceder al modo de grabación de la placa de desarrollo

Siga los siguientes pasos como se muestra en la imagen:

1. Use una pinza para insertar en el agujero `GND M4` indicado por la flecha en la placa y luego apriételo sin soltarlo.

2. Con la otra mano, presione el botón blanco en el centro de la placa.

3. En este momento, la herramienta de grabación mostrará automáticamente "MSDC Connected", lo que indica que el dispositivo está conectado.

4. Finalmente, haga clic en **Upgrade Firmware** para grabar el firmware.

![](H:\DongshanPI-PicoW\2023-04-10_在线资料整理\在线文档\DongshanPI-D1s\03-1_FlashSystem.assets\DongshanPI-PicoW-UsbDown2.png)

Consulte la ilustración a continuación para ver la operación.

![](https://jsd.cdn.zzko.cn/gh/DongshanPI/Docs-Photos@master/DongshanPI-PicoW/DongshanPI-PicoW-UsbDown3.png)

### 4. Inicio de la grabación

Haga clic en **Upgrade Firmware** en el software de grabación USBDonwloadTool para comenzar la grabación. Espere a que se complete la grabación. Una vez completada la grabación, aparecerá un cuadro de diálogo de confirmación de RESET. Presione el botón blanco central de la placa de desarrollo para reiniciar la placa.

![Imagen del software de grabación](https://jsd.cdn.zzko.cn/gh/DongshanPI/Docs-Photos@master/DongshanPI-PicoW/DongshanPI-PicoW-UsbDown4.png)


### 5. Configuración después de la grabación

Después de la grabación, si no se puede iniciar, es necesario agregar los siguientes parámetros env en la línea de comando uboot. Actualmente, se cree que es un error en la herramienta de grabación. La razón exacta es desconocida. Mantenga presionada la tecla Enter al iniciar para ingresar al modo de línea de comando uboot, ingrese la siguiente configuración y reinicie la placa.

```bash
setenv mtdids nand0=nand0
setenv mtdparts ' mtdparts=nand0:0x140000(CIS),0x1a0000(BOOT0),0x1a0000(BOOT1),0x40000(ENV),0x40000(ENV1),0x20000(KEY_CUST),0x500000(KERNEL),0x500000(RECOVERY),0x600000(rootfs),0xa0000(MISC),-(UBI)
setenv bootargs ubi.mtd=UBI,0x800 root=/dev/mtdblock8 rootfstype=squashfs ro init=/linuxrc LX_MEM=0x3FE0000 mma_heap=mma_heap_name0,miu=0,sz=0x1E00000 cma=2M highres=off mmap_reserved=fb,miu=0,sz=0x300000,max_start_off=0x3C00000,max_end_off=0x3F00000 ${mtdparts}
setenv bootcmd ' nand read.e 0x22000000 KERNEL ${kernel_file_size}; dcache on ; bootlogo 0 0 0 0; bootm 0x22000000;nand read.e 0x22000000 RECOVERY ${recovery_file_size}; dcache on ; bootm 0x22000000
setenv autoestart 0
setenv sstar_bbm off
setenv ipl_version "##p3##gdf99011IPL_##########setenv ipl_version "DUALENV=1 SILENT_CONSOLE=1 CFG_SDMMC_DISABLE=n ALK=1 SPINAND=1 CHIP=pioneer3""
saveenv
```