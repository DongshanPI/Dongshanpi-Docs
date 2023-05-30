[ [中文](https://dongshanpi.com/DongshanPi-PicoW/07-IKAYAKISDK_DevelopmentGuide/) | [[Español]](https://dongshanpi.com/DongshanPi-PicoW/07-IKAYAKISDK_DevelopmentGuide.ES/) ]

# Compilación del sistema utilizando IKAYAKI-SDK

## 1. Introducción a la arquitectura de IKAYAKI-SDK

### 1. Descripción de los módulos

| **Abreviatura** | **Nombre completo** | **Responsabilidad** |
| :-------------- | :------------------ | :------------------- |
| SYS             | System              | Implementación de la inicialización del sistema MI, la gestión del búfer de memoria y la gestión del flujo de datos entre los diferentes módulos |
| VDEC            | Video Decoder       | Decodificador de vídeo H264/H265/JPEG |
| DIVP            | Deinterlace&Video Post Process Engine | El motor DIVP tiene dos funciones principales: ① convertir el formato de los datos decodificados ② redimensionar los datos decodificados |
| VDISP           | Virtual Display     | Montaje de software |
| DISP            | Display Engine      | El motor DISP realiza el montaje de hardware de la imagen de salida de las unidades de procesamiento VDEC/DIVP y la señal de audio de salida AO, y la codifica en una señal de salida HDMI/VGA/CVBS |
| VENC            | Video Encoder       | Codificador de vídeo H264/H265/MotionJpeg |
| AI              | Audio Input Interface | Unidad de captura de entrada de audio I2S |
| AO              | Audio Output Interface | Salida de audio |
| GFX             | Graphics Engine     | El motor gráfico proporciona soporte básico de aceleración de hardware para el dibujo en 2D, reduciendo la carga de la CPU |
| FB              | FrameBuffer         | Pantalla de UI |
| HDMI            | High Definition Multimedia Interface | Salida estándar HDMI/VGA |

### 2. Arquitectura de software

![](https://jsd.cdn.zzko.cn/gh/DongshanPI/Docs-Photos@master/DongshanPI-PicoW/IKAYAKI-SDK_sw_arch.png)

- Las funciones de implementación se dividen de arriba hacia abajo en la capa de API de MI, la capa de implementación de MI, la capa de abstracción de hardware Hal, la capa de controlador Driver y la capa de hardware del chip.
- El código de funcionalidad del SDK se implementa en la capa del kernel para reducir la eficiencia de implementación de la función lógica al evitar la programación del kernel a modo de usuario.
- Para los clientes de la capa superior, se proporciona una interfaz de usuario de la API de MI. Las aplicaciones de la capa de usuario pueden llamar directamente a la interfaz de MI para acceder a la funcionalidad correspondiente de MI.



### 3. Estructura de directorios del SDK

| Directorio | Nombre del módulo | Función |
| :------ | :------------- | :--------------------------------------------------- |
| project | board          | Ruta de almacenamiento de información de PCB         |
| project | configs        | Ruta de almacenamiento de archivos de configuración predefinidos |
| project | image          | Biblioteca de materiales y lugar de almacenamiento de archivos de imagen para generar archivos de imagen |
| project | kbuild         | Entorno de compilación de kernel                       |
| project | release        | Piscina objetivo que almacena archivos de encabezado, archivos de biblioteca, módulos del kernel y bibliotecas de terceros para uso externo |
| SDK     | Verify/feature | Carpeta de verificación que almacena archivos de prueba de unidad y características de módulos |
| SDK     | Verify/demo    | Demostración de prueba de funcionalidad general |

### 4. Gestión de memoria

Consulte [Introducción a la distribución de memoria](../reference/memory_layout.html) para obtener más detalles.

- Memoria reservada a través de mmap.ini
- Soporta gestión de memoria mma en el módulo, con cada módulo asignando un tamaño reservado en mma.

![](https://jsd.cdn.zzko.cn/gh/DongshanPI/Docs-Photos@master/DongshanPI-PicoW/IKAYAKI-SDK_memory_layout.png)


### 5. Conceptos básicos

- Flujo de datos: cada módulo MI se puede considerar una unidad de procesamiento de datos puros, y el flujo de datos de entrada y salida se coordina internamente en MI SYS. El flujo de datos de entrada representa los datos de entrada de la unidad de datos, y el flujo de datos de salida representa los datos procesados por la unidad.
- Flujo de control: el proceso de control de los parámetros de procesamiento de datos de cada módulo MI por parte de la aplicación, como configurar los parámetros de decodificación de MI_VDEC, iniciar y detener el canal de MI_VDEC, y establecer la resolución y el formato de salida del puerto de MI_VDEC, etc.
- Canal: para los módulos MI que requieren procesar o producir un flujo, un canal representa el contexto de multiplexación por división de tiempo de un flujo en un módulo MI y los ajustes de flujo de control relacionados. Para módulos reutilizables como MI_VDEC, MI_DIVP y MI_DISP, pueden admitir varios canales.
- Puerto: el puerto se divide en dos tipos, puerto de entrada y puerto de salida. El puerto de entrada es la posición del flujo de entrada del canal, mientras que el puerto de salida es la posición del flujo de salida del canal. Un canal puede tener múltiples puertos de entrada y múltiples puertos de salida.

![](https://jsd.cdn.zzko.cn/gh/DongshanPI/Docs-Photos@master/DongshanPI-PicoW/IKAYAKI-SDK_mi_flow.png)



## 2. Obtener el proyecto SDK

### Obtener el código

Abra el enlace https://dongshanpi.cowtransfer.com/s/5c3c05deef7247 con un navegador y descargue la carpeta completa IKAYAKI_SDK. Después de descargar, verá que contiene los siguientes 6 archivos de paquetes comprimidos:

```bash
arm-sigmastar-linux-uclibcgnueabihf-9.1.0.tar.xz
gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf.tar.xz
boot-IKAYAKI_DLM00V015.tar.gz    
project-IKAYAKI_DLM00V015.tar.gz
kernel-IKAYAKI_DLM00V015.tar.gz  
sdk-IKAYAKI_DLM00V015.tar.gz
```
Utilice Filezila, Samba o arrastre los archivos para copiarlos a la carpeta de inicio del sistema Ubuntu.

Antes de copiar, puede crear un directorio para la placa usando el siguiente comando de ejemplo después de mkdir DongshanPI-PicoW, y luego cree otro directorio llamado IKAYAKI_SDK dentro de él.

```shell
book@100ask.org:~$ mkdir DongshanPI-PicoW
book@100ask.org:~$ cd DongshanPI-PicoW/
book@100ask.org:~/DongshanPI-PicoW$ mkdir IKAYAKI_SDK
book@100ask.org:~/DongshanPI-PicoW$ cd IKAYAKI_SDK/
book@100ask.org:~/DongshanPI-PicoW/IKAYAKI_SDK$
```

Por último, copie los 6 archivos comprimidos descargados en la carpeta IKAYAKI_SDK que acabamos de crear. Una vez que se haya completado la copia, se mostrará como se muestra a continuación:

```shell
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$ ls -lh
total 1.3G
-rwxr--r-- 1 book book  40M Sep  9  2022 arm-sigmastar-linux-uclibcgnueabihf-9.1.0.tar.xz
-rwxr--r-- 1 book book  16M Sep  9  2022 boot-IKAYAKI_DLM00V015.tar.gz
-rwxr--r-- 1 book book 341M Sep  9  2022 gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf.tar.xz
-rwxr--r-- 1 book book 158M Sep  9  2022 kernel-IKAYAKI_DLM00V015.tar.gz
-rwxr--r-- 1 book book 452M Sep  9  2022 project-IKAYAKI_DLM00V015.tar.gz
-rwxr--r-- 1 book book 310M Sep  9  2022 sdk-IKAYAKI_DLM00V015.tar.gz
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$
```

### Descomprimir el código fuente

A continuación, necesitamos utilizar el comando tar para descomprimir en orden.

* Descomprimir la cadena de herramientas glibc

```shell
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$ tar -xvf  gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf.tar.xz
```

* Descomprimir el cargador de arranque
```shell
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$ tar -xvf  boot-IKAYAKI_DLM00V015.tar.gz
```


* Descomprimir el kernel
```shell
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$ tar -xvf  kernel-IKAYAKI_DLM00V015.tar.gz
```

* Descomprimir el kit de desarrollo de software
```shell
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$ tar -xvf  project-IKAYAKI_DLM00V015.tar.gz
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$ tar -xvf  sdk-IKAYAKI_DLM00V015.tar.gz
```

Después de descomprimir, se encontrará que los directorios de boot, kernel, project y sdk no tienen la marca **-IKAYAKI_DLM00V015** en su nombre. Por favor, preste atención a esto.

```bash
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$ ls -lh
total 1.3G
dr-xr-xr-x  8 book book 4.0K Sep 16  2020 arm-sigmastar-linux-uclibcgnueabihf-9.1.0
-rwxr--r--  1 book book  40M Sep  9  2022 arm-sigmastar-linux-uclibcgnueabihf-9.1.0.tar.xz
drwxr-xr-x 21 book book 4.0K Dec 20  2021 boot
-rwxr--r--  1 book book  16M Sep  9  2022 boot-IKAYAKI_DLM00V015.tar.gz
drwxr-xr-x  8 book book 4.0K Jul 24  2020 gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf
-rwxr--r--  1 book book 341M Sep  9  2022 gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf.tar.xz
drwxr-xr-x 25 book book 4.0K Dec 20  2021 kernel
-rwxr--r--  1 book book 158M Sep  9  2022 kernel-IKAYAKI_DLM00V015.tar.gz
drwxr-xr-x 10 book book 4.0K Dec 20  2021 project
-rwxr--r--  1 book book 452M Sep  9  2022 project-IKAYAKI_DLM00V015.tar.gz
drwxr-xr-x  4 book book 4.0K Dec 20  2021 sdk
-rwxr--r--  1 book book 310M Sep  9  2022 sdk-IKAYAKI_DLM00V015.tar.gz
```



### Configuración de la herramienta de cadena

En el paso anterior descomprimimos la herramienta de cadena cruzada de compilación glibc, ahora necesitamos configurar las variables de entorno. Primero, ingresamos a la ruta de la herramienta de cadena de compilación cruzada `gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf`, luego ingresamos a la ruta bin y usamos el comando pwd para mostrar la ruta completa actual, `/home/book/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin`. Luego, configuramos los parámetros de la variable de entorno en el archivo ~/.bashrc.

```shell
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$ cd gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf$ cd bin/
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin$ ls
arm-linux-gnueabihf-addr2line      arm-linux-gnueabihf-ld.bfd                      arm-linux-gnueabihf-sigmastar-9.1.0-gcov-dump
arm-linux-gnueabihf-ar             arm-linux-gnueabihf-ld.gold                     arm-linux-gnueabihf-sigmastar-9.1.0-gcov-tool
arm-linux-gnueabihf-as             arm-linux-gnueabihf-nm                          arm-linux-gnueabihf-sigmastar-9.1.0-gdb
arm-linux-gnueabihf-c++            arm-linux-gnueabihf-objcopy                     arm-linux-gnueabihf-sigmastar-9.1.0-gdb-add-index
arm-linux-gnueabihf-c++filt        arm-linux-gnueabihf-objdump                     arm-linux-gnueabihf-sigmastar-9.1.0-gfortran
arm-linux-gnueabihf-cpp            arm-linux-gnueabihf-ranlib                      arm-linux-gnueabihf-sigmastar-9.1.0-gprof
arm-linux-gnueabihf-dwp            arm-linux-gnueabihf-readelf                     arm-linux-gnueabihf-sigmastar-9.1.0-ld
arm-linux-gnueabihf-elfedit        arm-linux-gnueabihf-sigmastar-9.1.0-addr2line   arm-linux-gnueabihf-sigmastar-9.1.0-ld.bfd
arm-linux-gnueabihf-g++            arm-linux-gnueabihf-sigmastar-9.1.0-ar          arm-linux-gnueabihf-sigmastar-9.1.0-ld.gold
arm-linux-gnueabihf-gcc            arm-linux-gnueabihf-sigmastar-9.1.0-as          arm-linux-gnueabihf-sigmastar-9.1.0-nm
arm-linux-gnueabihf-gcc-9.1.0      arm-linux-gnueabihf-sigmastar-9.1.0-c++         arm-linux-gnueabihf-sigmastar-9.1.0-objcopy
arm-linux-gnueabihf-gcc-ar         arm-linux-gnueabihf-sigmastar-9.1.0-c++filt     arm-linux-gnueabihf-sigmastar-9.1.0-objdump
arm-linux-gnueabihf-gcc-nm         arm-linux-gnueabihf-sigmastar-9.1.0-cpp         arm-linux-gnueabihf-sigmastar-9.1.0-ranlib
arm-linux-gnueabihf-gcc-ranlib     arm-linux-gnueabihf-sigmastar-9.1.0-dwp         arm-linux-gnueabihf-sigmastar-9.1.0-readelf
arm-linux-gnueabihf-gcov           arm-linux-gnueabihf-sigmastar-9.1.0-elfedit     arm-linux-gnueabihf-sigmastar-9.1.0-size
arm-linux-gnueabihf-gcov-dump      arm-linux-gnueabihf-sigmastar-9.1.0-g++         arm-linux-gnueabihf-sigmastar-9.1.0-strings
arm-linux-gnueabihf-gcov-tool      arm-linux-gnueabihf-sigmastar-9.1.0-gcc         arm-linux-gnueabihf-sigmastar-9.1.0-strip
arm-linux-gnueabihf-gdb            arm-linux-gnueabihf-sigmastar-9.1.0-gcc-9.1.0   arm-linux-gnueabihf-size
arm-linux-gnueabihf-gdb-add-index  arm-linux-gnueabihf-sigmastar-9.1.0-gcc-ar      arm-linux-gnueabihf-strings
arm-linux-gnueabihf-gfortran       arm-linux-gnueabihf-sigmastar-9.1.0-gcc-nm      arm-linux-gnueabihf-strip
arm-linux-gnueabihf-gprof          arm-linux-gnueabihf-sigmastar-9.1.0-gcc-ranlib
arm-linux-gnueabihf-ld             arm-linux-gnueabihf-sigmastar-9.1.0-gcov
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin$ pwd
/home/book/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin$
```

* Configuración de variables de entorno temporales

  Este método solo es válido en la terminal actual. Si se abre una nueva terminal, es necesario volver a configurar y ejecutar estas configuraciones.

```bahs
export ARCH=arm
export CROSS_COMPILE=arm-linux-gnueabihf-
export PATH=$PATH:/home/book/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin
```

```bash
book@virtual-machine:~$ export ARCH=arm
book@virtual-machine:~$ export CROSS_COMPILE=arm-linux-gnueabihf-
book@virtual-machine:~$ export PATH=$PATH:/home/book/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin
```



* Configuración de variables de entorno permanente

Abra el archivo `~/.bashrc` con un editor de texto como `vim`, `gedit`, `nano`, etc., y agregue las siguientes tres líneas de configuración de entorno al final del archivo para que se apliquen de manera permanente.

```bash
export ARCH=arm
export CROSS_COMPILE=arm-linux-gnueabihf-
export PATH=$PATH:/home/book/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin
```



### Modificar el shell predeterminado

Si el shell predeterminado es sh, es necesario cambiarlo a **bash**.

```
book@virtual-machine:~$ sudo rm /bin/sh
book@virtual-machine:~$ sudo ln –s /bin/bash /bin/sh
```

## 3. Pasos para compilar el paquete de desarrollo del SDK

A continuación se mencionan los pasos para compilar "boot", "kernel" y "SDK". El "boot" ya se ha compilado y se encuentra en la ruta del proyecto de forma predeterminada. Por lo tanto, al comprobar la validez de los pasos, se puede omitir "compilar boot" y "compilar kernel" y empezar a compilar todo el "image" desde "compilar SDK" (la compilación del kernel ya está integrada en la compilación del SDK y se compilará automáticamente). Entonces se puede quemar y verificar.

Nota: Los fabricantes de chips diferentes tienen diferentes marcos y formas de desarrollo. Por favor, no se enfoquen en este problema, sólo sigan las instrucciones del documento.

Nota: Los fabricantes de chips diferentes tienen diferentes marcos y formas de desarrollo. Por favor, no se enfoquen en este problema, sólo sigan las instrucciones del documento.

Nota: Los fabricantes de chips diferentes tienen diferentes marcos y formas de desarrollo. Por favor, no se enfoquen en este problema, sólo sigan las instrucciones del documento.

### Compilación del cargador de arranque

1. Verificar si las variables de entorno son correctas

En la terminal, introduzca "arm-linux-gnueabihf-gcc -v" para comprobar si está correcto. Si se muestra la información de la versión de GCC, está correcto. Si no es así, vuelva al paso anterior para configurarlo.

```bash
book@virtual-machine:~$ arm-linux-gnueabihf-gcc -v
Using built-in specs.
COLLECT_GCC=arm-linux-gnueabihf-gcc
COLLECT_LTO_WRAPPER=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin/../libexec/gcc/arm-linux-gnueabihf/9.1.0/lto-wrapper
Target: arm-linux-gnueabihf
Configured with: '/home/meng/toolchain/build/snapshots/gcc.git~gcc-9_1_0-release/configure' SHELL=/bin/bash --with-mpc=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu --with-mpfr=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu --with-gmp=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu --with-gnu-as --with-gnu-ld --disable-libmudflap --enable-lto --enable-shared --without-included-gettext --enable-nls --with-system-zlib --disable-sjlj-exceptions --enable-gnu-unique-object --enable-linker-build-id --disable-libstdcxx-pch --enable-c99 --enable-clocale=gnu --enable-libstdcxx-debug --enable-long-long --with-cloog=no --with-ppl=no --with-isl=no --disable-multilib --with-float=hard --with-fpu=vfpv3-d16 --with-mode=thumb --with-arch=armv7-a --enable-threads=posix --enable-multiarch --enable-libstdcxx-time=yes --enable-gnu-indirect-function --with-build-sysroot=/home/meng/toolchain/build/sysroots/arm-linux-gnueabihf --with-sysroot=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu/arm-linux-gnueabihf/libc --enable-checking=yes --disable-bootstrap --enable-languages=c,c++,fortran,lto --build=x86_64-unknown-linux-gnu --host=x86_64-unknown-linux-gnu --target=arm-linux-gnueabihf --prefix=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu
Thread model: posix
gcc version 9.1.0 (GCC)
```

Después de eso, configure nuevamente las dos variables de entorno ARCH y CROSS_COMPILE en la terminal, y una vez completada la configuración, puede continuar con las siguientes operaciones de desarrollo.

```bash
export ARCH=arm
export CROSS_COMPILE=arm-linux-gnueabihf-
```



2. Entrar en el directorio bootloader

A continuación, ingresa al directorio `boot`, que es el directorio de código fuente del bootloader. Una vez dentro, podrás compilar la imagen del bootloader aquí.

```bash
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$ cd boot/
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/boot$ ls
api     config.mk      doc       fs       lib             MAKEALL           mz           README                tatus
arch    configs        drivers   include  Licenses        Makefile          net          release_to_alkaid.sh  test
board   create_img.sh  dts       Kbuild   list_config.sh  mkimage           pad_file.py  scripts               tools
common  disk           examples  Kconfig  MAINTAINERS     ms_gen_mvxv_h.py  post         snapshot.commit
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/boot$
```



3. Compilar bootloader

Antes de comenzar a compilar, debemos asegurarnos de que el nombre del archivo de configuración sea el correcto, como se muestra a continuación. El archivo de configuración de imagen predeterminado para DongshanPI-PicoW se llama pioneer3_spinand_defconfig.

| Tipo de Flash | Otros                         | Defconfig                          |
| :----------- | :---------------------------- | :--------------------------------- |
| SPI-NAND      | Básico                        | pioneer3_spinand_defconfig         |

A continuación, use "make pioneer3_spinand_defconfig" para especificar el archivo de configuración y comenzar la compilación. Después de completar la configuración, ingrese el comando "make" para comenzar la compilación. Los siguientes son los pasos de operación, tenga en cuenta que solo después del símbolo "$" es el comando.

```bash
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/boot$ make  pioneer3_spinand_defconfig
  HOSTCC  scripts/basic/fixdep
  HOSTCC  scripts/kconfig/conf.o
  SHIPPED scripts/kconfig/zconf.tab.c
  SHIPPED scripts/kconfig/zconf.lex.c
  SHIPPED scripts/kconfig/zconf.hash.c
  HOSTCC  scripts/kconfig/zconf.tab.o
  HOSTLD  scripts/kconfig/conf
#
# configuration written to .config
#
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/boot$ make -j8
......

{standard input}: Assembler messages:
{standard input}:1404: Warning: setting incorrect section attributes for .rodata
  CC      net/net.o
  CC      fs/fat/fat_write.o
  CC      fs/firmwarefs/fwfs_util.o
  CC      fs/firmwarefs/fwfs.o
  LD      common/built-in.o
  CC      net/ping.o
  CC      lib/xz/xz_dec_lzma2.o
  CC      fs/fat/file.o
  CC      fs/firmwarefs/fwfs_tftl.o
  CC      fs/firmwarefs/firmwarefs.o
  CC      net/tftp.o
  CC      lib/zlib/zlib.o
  CC      lib/crc7.o
  CC      lib/crc8.o
  CC      lib/crc16.o
  CC      lib/gunzip.o
  LD      net/built-in.o
  CC      lib/initcall.o
  CC      lib/lmb.o
  CC      lib/ldiv.o
  CC      lib/net_utils.o
  CC      lib/qsort.o
  CC      lib/strmhz.o
  CC      lib/rbtree.o
  CC      lib/list_sort.o
  CC      lib/hashtable.o
  LD      lib/xz/built-in.o
  CC      lib/errno.o
  CC      lib/display_options.o
  CC      lib/crc32.o
  CC      lib/ctype.o
  CC      lib/div64.o
  CC      lib/hang.o
  CC      lib/linux_compat.o
  CC      lib/linux_string.o
  CC      lib/string.o
  CC      lib/time.o
  CC      lib/vsprintf.o
  CC      lib/uminiz.o
  LD      fs/fat/built-in.o
  LD      lib/zlib/built-in.o
  LD      lib/built-in.o
  LD      fs/firmwarefs/built-in.o
  LD      fs/built-in.o
  CC      examples/standalone/stubs.o
  CC      examples/standalone/hello_world.o
  LD      examples/standalone/libstubs.o
  LD      examples/standalone/hello_world
  OBJCOPY examples/standalone/hello_world.srec
  OBJCOPY examples/standalone/hello_world.bin
  LD      u-boot
  OBJCOPY u-boot.srec
  OBJCOPY u-boot.bin
Mode: c, Level: 4
Input File: "u-boot.bin"
Output File: "u-boot.bin.mz"
Input file size: 731100
Total input bytes: 731100
Total output bytes: 343142
Success.
xz: u-boot.bin.xz: File already has `.xz' suffix, skipping

u-boot_spinand.mz.img.bin
Image Name:   MVX4##P3##g#######CM_UBT1501#XVM
Created:      Wed Apr 12 09:47:31 2023
Image Type:   ARM U-Boot Kernel Image (mz compressed)
Data Size:    343142 Bytes = 335.10 kB = 0.33 MB
Load Address: 23d00000
Entry Point:  23e00000


u-boot_spinand.xz.img.bin
Image Name:   MVX4##P3##g#######CM_UBT1501#XVM
Created:      Wed Apr 12 09:47:31 2023
Image Type:   ARM U-Boot Kernel Image (lzma compressed)
Data Size:    255572 Bytes = 249.58 kB = 0.24 MB
Load Address: 23d00000
Entry Point:  23e00000


u-boot_spinand.img.bin
Image Name:   MVX4##P3##g#######CM_UBT1501#XVM
Created:      Wed Apr 12 09:47:31 2023
Image Type:   ARM U-Boot Kernel Image (uncompressed)
Data Size:    731100 Bytes = 713.96 kB = 0.70 MB
Load Address: 23d00000
Entry Point:  23e00000

book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/boot$
```

La imagen compilada es `u-boot_spinand.xz.img.bin`, que es el archivo de imagen final que se puede utilizar para la grabación.

En general, no se puede escribir solo la imagen de U-boot, por lo que es mejor seguir los siguientes pasos para copiarla al directorio del proyecto, crear una imagen completa y usar una herramienta de grabación.

4. Copia y empaquetado

Reemplace `u-boot_spinand.xz.img.bin` compilado en `project/board/p3/boot/spinand/uboot/u-boot_spinand.xz.img.bin`. Luego, vuelva a compilar el proyecto para generar una imagen del sistema completa.

```bash
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/boot$ cp u-boot_spinand.xz.img.bin ../project/board/p3/boot/spinand/uboot/
```



### Compilación del kernel

El SDK compilará el kernel por defecto, a diferencia de SSD201/SSD202. Por lo tanto, se recomienda que después de modificar el kernel, se compile directamente en el directorio del proyecto. De esta manera, se compilará el contenido del kernel por defecto y no será necesario realizar una liberación manual en la ruta del proyecto.

1. Verifique si las variables de entorno están configuradas correctamente

En la terminal, ingrese "arm-linux-gnueabihf-gcc -v" para verificar si están configuradas correctamente. Si está todo bien, se imprimirá la información de la versión de gcc, como se muestra a continuación. Si no es así, regrese al paso anterior para realizar la configuración.

```bash
book@virtual-machine:~$ arm-linux-gnueabihf-gcc -v
Using built-in specs.
COLLECT_GCC=arm-linux-gnueabihf-gcc
COLLECT_LTO_WRAPPER=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin/../libexec/gcc/arm-linux-gnueabihf/9.1.0/lto-wrapper
Target: arm-linux-gnueabihf
Configured with: '/home/meng/toolchain/build/snapshots/gcc.git~gcc-9_1_0-release/configure' SHELL=/bin/bash --with-mpc=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu --with-mpfr=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu --with-gmp=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu --with-gnu-as --with-gnu-ld --disable-libmudflap --enable-lto --enable-shared --without-included-gettext --enable-nls --with-system-zlib --disable-sjlj-exceptions --enable-gnu-unique-object --enable-linker-build-id --disable-libstdcxx-pch --enable-c99 --enable-clocale=gnu --enable-libstdcxx-debug --enable-long-long --with-cloog=no --with-ppl=no --with-isl=no --disable-multilib --with-float=hard --with-fpu=vfpv3-d16 --with-mode=thumb --with-arch=armv7-a --enable-threads=posix --enable-multiarch --enable-libstdcxx-time=yes --enable-gnu-indirect-function --with-build-sysroot=/home/meng/toolchain/build/sysroots/arm-linux-gnueabihf --with-sysroot=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu/arm-linux-gnueabihf/libc --enable-checking=yes --disable-bootstrap --enable-languages=c,c++,fortran,lto --build=x86_64-unknown-linux-gnu --host=x86_64-unknown-linux-gnu --target=arm-linux-gnueabihf --prefix=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu
Thread model: posix
gcc version 9.1.0 (GCC)
```

Luego, configure nuevamente las variables de entorno ARCH y CROSS_COMPILE en la terminal. Después de configurarlas, puede continuar con las siguientes operaciones de desarrollo.

```bash
export ARCH=arm
export CROSS_COMPILE=arm-linux-gnueabihf-
```



2. Ingrese al directorio del kernel de Linux

A continuación, ingrese al directorio "boot", que es el directorio del código fuente del bootloader. Después de ingresar al directorio, puede compilar la imagen del bootloader aquí.

```bash
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK$ cd kernel/
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/kernel$ ls
arch     CREDITS        firmware  ipc      lib             Makefile        ms_pack_modules.sh           REPORTING-BUGS  sound
block    crypto         fs        Kbuild   list_config.sh  mm              net                          samples         tools
certs    Documentation  include   Kconfig  MAINTAINERS     modules         README                       scripts         usr
COPYING  drivers        init      kernel   makefile        Module.symvers  release_kernel_to_alkaid.sh  security        virt
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/kernel$

```



3. Compile el kernel de Linux

Antes de comenzar a compilar, debemos confirmar el nombre del archivo de configuración. Para el DongshanPI-PicoW, el nombre del archivo de configuración de imagen predeterminado es "pioneer3_ssc021a_s01a_spinand_demo_defconfig", como se muestra a continuación.

| **Chip** | **Packaging** | **Memory** | **Flash Type** | **Toolchain** | **Other**  | **Defconfig**                                     |
| :------- | :------------ | :--------- | :------------- | :------------ | :--------- | :------------------------------------------------ |
| SSD210   | QFN68         | 64M        | SPI-NAND       | glibc         | Sin sensor | make pioneer3_ssc021a_s01a_spinand_demo_defconfig |


A continuación, use "make pioneer3_ssc021a_s01a_spinand_demo_defconfig" para especificar el archivo de configuración y comenzar a compilar. Después de completar la configuración, ingrese el comando "make" para comenzar a compilar. Los pasos operativos son los siguientes, preste atención a que el comando debe seguir el símbolo "$".

```bash
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/kernel$ make pioneer3_ssc021a_s01a_spinand_demo_defconfig
Extract CHIP NAME (pioneer3) to '.sstar_chip.txt'
make[1]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/kernel'
  HOSTCC  scripts/basic/fixdep
  HOSTCC  scripts/kconfig/conf.o
  HOSTCC  scripts/kconfig/zconf.tab.o
  HOSTLD  scripts/kconfig/conf
#
# configuration written to .config
#
make[1]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/kernel'
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/kernel$ make -j8
make[1]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/kernel'
.....
changelist g
  BRANCHID   g
  CHK     include/generated/uapi/linux/version.h
  MS_PLATFORM_ID: P3
  CHK     include/generated/utsrelease.h
  CHK     include/generated/timeconst.h
  CHK     include/generated/bounds.h
  CHK     include/generated/asm-offsets.h
  CALL    scripts/checksyscalls.sh
  CHK     include/generated/compile.h
  CC      arch/arm/mach-sstar/ms_chip.o
  LD      arch/arm/mach-sstar/built-in.o
  LD      vmlinux.o
  MODPOST vmlinux.o
WARNING: modpost: Found 5 section mismatch(es).
To see full details build your kernel with:
'make CONFIG_DEBUG_SECTION_MISMATCH=y'
  GEN     .version
  CHK     include/generated/compile.h
  UPD     include/generated/compile.h
  CC      init/version.o
  LD      init/built-in.o
  KSYM    .tmp_kallsyms1.o
  KSYM    .tmp_kallsyms2.o
  LD      vmlinux
  SORTEX  vmlinux
  SYSMAP  System.map
  OBJCOPY arch/arm/boot/Image
  Building modules, stage 2.
  Kernel: arch/arm/boot/Image is ready
  MODPOST 35 modules
  DTC     arch/arm/boot/dts/pioneer3-ssc021a-s01a-demo.dtb
  DTC     arch/arm/boot/dts/pioneer3-fpga.dtb
  XZKERN  arch/arm/boot/compressed/piggy_data


  Packing modules to '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/kernel/modules'


  AS      arch/arm/boot/compressed/piggy.o
  LD      arch/arm/boot/compressed/vmlinux
  OBJCOPY arch/arm/boot/zImage
#update builtin DTB
  IMAGE   arch/arm/boot/Image
  BNDTB arch/arm/boot/dts/pioneer3-ssc021a-s01a-demo.dtb
offset:0x004103B0
  size:0x00009545

#update Image-fpga DTB
  IMAGE   arch/arm/boot/Image-fpga
  BNDTB   pioneer3-fpga.dtb
offset:0x004103B0
  size:0x00008904

  The configuration of GPIO & PADMUX is correct!
#build uImage
Image Name:   MVX4##P3##g#######KL_LX409##[BR:
Created:      Wed Apr 12 10:07:07 2023
Image Type:   ARM Linux Kernel Image (uncompressed)
Data Size:    4444160 Bytes = 4340.00 kB = 4.24 MB
Load Address: 20008000
Entry Point:  20008000

Compress Kernel Image
Image Name:   MVX4##P3##g#######KL_LX409##[BR:
Created:      Wed Apr 12 10:07:10 2023
Image Type:   ARM Linux Kernel Image (lzma compressed)
Data Size:    2120676 Bytes = 2070.97 kB = 2.02 MB
Load Address: 20008000
Entry Point:  20008000
Mode: c, Level: 4
Input File: "arch/arm/boot/Image"
Output File: "arch/arm/boot/Image.mz"
Input file size: 4444160
Total input bytes: 4444160
Total output bytes: 2577043
Success.

Image Name:   MVX4##P3##g#######KL_LX409##[BR:
Created:      Wed Apr 12 10:07:10 2023
Image Type:   ARM Linux Kernel Image (mz compressed)
Data Size:    2577043 Bytes = 2516.64 kB = 2.46 MB
Load Address: 20008000
Entry Point:  20008000


  Kernel: arch/arm/boot/zImage is ready
make[1]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/kernel'
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/kernel$

```

El imagen compilado se encuentra en `arch/arm/boot/uImage.xz`, este es el imagen de kernel final que se puede utilizar para arrancar. El archivo de dispositivo es `arch/arm/boot/dts/pioneer3-ssc021a-s01a-demo.dtb`.

Si hay nuevos módulos de kernel añadidos, se deben agregar los módulos correspondientes a `kernel_mod_list/kernel_mod_list_late` (los ko en kernel_mod_list_late se cargan después del módulo mi). Ruta de modificación: `project/kbuild/customize/$(KERNEL_VERSION)/$(CHIP)/$(PRODUCT)/kernel_mod_list`.

Nota: Si se produce un error de empaquetamiento dtb durante la compilación del kernel, consulte https://github.com/DongshanPI/Sigmastar-Linux/commit/dfbfa30b5dd564a05812d5ecd967a53e51749304 para modificar el código fuente correspondiente. Este problema se debe a problemas de incompatibilidad del SDK.

### Compilando SDK

Por defecto, al compilar el SDK, se compila primero el kernel. La primera vez se compila todo con el comando `make clean;make image -j8`, posteriormente, si no es necesario compilar el kernel, se puede utilizar `make image-fast` en lugar de `make image`.

1. Verifique si las variables de entorno son correctas

En la terminal, escriba `arm-linux-gnueabihf-gcc -v` para verificar si es correcto. Si es correcto, se imprimirá información de versión de gcc, etc., como se muestra a continuación. Si no, vuelva al paso anterior de configuración.

```bash
book@virtual-machine:~$ arm-linux-gnueabihf-gcc -v
Using built-in specs.
COLLECT_GCC=arm-linux-gnueabihf-gcc
COLLECT_LTO_WRAPPER=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/gcc-sigmastar-9.1.0-2020.07-x86_64_arm-linux-gnueabihf/bin/../libexec/gcc/arm-linux-gnueabihf/9.1.0/lto-wrapper
Target: arm-linux-gnueabihf
Configured with: '/home/meng/toolchain/build/snapshots/gcc.git~gcc-9_1_0-release/configure' SHELL=/bin/bash --with-mpc=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu --with-mpfr=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu --with-gmp=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu --with-gnu-as --with-gnu-ld --disable-libmudflap --enable-lto --enable-shared --without-included-gettext --enable-nls --with-system-zlib --disable-sjlj-exceptions --enable-gnu-unique-object --enable-linker-build-id --disable-libstdcxx-pch --enable-c99 --enable-clocale=gnu --enable-libstdcxx-debug --enable-long-long --with-cloog=no --with-ppl=no --with-isl=no --disable-multilib --with-float=hard --with-fpu=vfpv3-d16 --with-mode=thumb --with-arch=armv7-a --enable-threads=posix --enable-multiarch --enable-libstdcxx-time=yes --enable-gnu-indirect-function --with-build-sysroot=/home/meng/toolchain/build/sysroots/arm-linux-gnueabihf --with-sysroot=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu/arm-linux-gnueabihf/libc --enable-checking=yes --disable-bootstrap --enable-languages=c,c++,fortran,lto --build=x86_64-unknown-linux-gnu --host=x86_64-unknown-linux-gnu --target=arm-linux-gnueabihf --prefix=/home/meng/toolchain/build/builds/destdir/x86_64-unknown-linux-gnu
Thread model: posix
gcc version 9.1.0 (GCC)
```

Después, configure nuevamente las variables de entorno ARCH y CROSS_COMPILE en la terminal. Una vez configuradas, se puede continuar con las operaciones de desarrollo posteriores.

```bash
export ARCH=arm
export CROSS_COMPILE=arm-linux-gnueabihf-
```



2. Compilación con archivo de configuración especificado

Ingrese al directorio del proyecto:
```
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/project$ ls
board                                                      include                         make_sd_upgrade_sigmastar.sh   scripts
change_config_into_defconfig.sh                            kbuild                          make_usb_factory_sigmastar.sh  setup_config.sh
configs                                                    Kconfig                         make_usb_upgrade_sigmastar.sh  setup_defconfig.sh
dispcam_p3_nor.glibc-9.1.0-ramfs.s01a.64.qfn128_defconfig  make_emmc_upgrade_sigmastar.sh  parser_IPL.sh                  split_partion.sh
image                                                      makefile                        release                        tools
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/project$
```
Ejecute el comando `make dispcam_p3_spinand.glibc-9.1.0-s01a.64.qfn68.demo_defconfig` para comenzar la configuración.

| **Chip** | **Empaquetado** | **Memoria** | **Tipo de flash** | **Herramienta de compilación** | **Otros**  | **Archivo de configuración**                                                |
| -------- | ------------- | ---------- | -------------- | ------------- | ---------- | ------------------------------------------------------------ |
| SSD210   | QFN68         | 64M        | SPI-NAND       | glibc         | Sin sensor | make dispcam_p3_spinand.glibc-9.1.0-s01a.64.qfn68.demo_defconfig |

Todo el proceso de configuración es bastante rápido.
```bash
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/project$ make dispcam_p3_spinand.glibc-9.1.0-s01a.64.qfn68.demo_defconfig
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
make -f /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/scripts/Makefile.build obj=scripts/basic
make[1]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project'
  gcc -Wp,-MD,scripts/basic/.fixdep.d -Wall -Wmissing-prototypes -Wstrict-prototypes -O2 -fomit-frame-pointer -std=gnu89     -o scripts/basic/fixdep scripts/basic/fixdep.c
make[1]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project'
make -f /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/scripts/Makefile.build obj=scripts/kconfig dispcam_p3_spinand.glibc-9.1.0-s01a.64.qfn68.demo_defconfig
make[1]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project'
  gcc -Wp,-MD,scripts/kconfig/.conf.o.d -Wall -Wmissing-prototypes -Wstrict-prototypes -O2 -fomit-frame-pointer -std=gnu89   -D_GNU_SOURCE -D_DEFAULT_SOURCE -I/usr/include/ncursesw -DCURSES_LOC="<ncurses.h>" -DNCURSES_WIDECHAR=1 -DLOCALE   -c -o scripts/kconfig/conf.o scripts/kconfig/conf.c
  gcc -Wp,-MD,scripts/kconfig/.zconf.tab.o.d -Wall -Wmissing-prototypes -Wstrict-prototypes -O2 -fomit-frame-pointer -std=gnu89   -D_GNU_SOURCE -D_DEFAULT_SOURCE -I/usr/include/ncursesw -DCURSES_LOC="<ncurses.h>" -DNCURSES_WIDECHAR=1 -DLOCALE  -Iscripts/kconfig -c -o scripts/kconfig/zconf.tab.o scripts/kconfig/zconf.tab.c
  gcc  -o scripts/kconfig/conf scripts/kconfig/conf.o scripts/kconfig/zconf.tab.o
#scripts/kconfig/conf  --defconfig=arch//configs/dispcam_p3_spinand.glibc-9.1.0-s01a.64.qfn68.demo_defconfig Kconfig
scripts/kconfig/conf  --defconfig=configs/defconfigs/dispcam_p3_spinand.glibc-9.1.0-s01a.64.qfn68.demo_defconfig Kconfig
#
# configuration written to .config
#
make[1]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project'
make silentoldconfig
make[1]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project'
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
cat: /home/causer/swnas/workspace/ALL--ALKAID--ReleaseBuild/alkaid/release/release_1220/sourcecode/project/../kernel/Makefile: No such file or directory
make -f /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/scripts/Makefile.build obj=scripts/kconfig silentoldconfig
make[2]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project'
mkdir -p include/config include/generated
test -e include/generated/autoksyms.h || \
    touch   include/generated/autoksyms.h
scripts/kconfig/conf  --silentoldconfig Kconfig
make[2]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project'
make[1]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project'
PROJ_ROOT = /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project
CONFIG_NAME = config_module_list.mk
KBUILD_MK = kbuild/kbuild.mk
SOURCE_MK = ../sdk/sdk.mk
KERNEL_MEMADR = $(shell /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/makefiletools/bin/mmapparser /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/$(CHIP)/mmap/$(MMAP) $(CHIP) E_LX_MEM phyaddr)
KERNEL_MEMLEN = $(shell /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/makefiletools/bin/mmapparser /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/$(CHIP)/mmap/$(MMAP) $(CHIP) E_LX_MEM size)
KERNEL_MEMADR2 = $(shell /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/makefiletools/bin/mmapparser /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/$(CHIP)/mmap/$(MMAP) $(CHIP) E_LX_MEM2 phyaddr)
KERNEL_MEMLEN2 = $(shell /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/makefiletools/bin/mmapparser /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/$(CHIP)/mmap/$(MMAP) $(CHIP) E_LX_MEM2 size)
KERNEL_MEMADR3 = $(shell /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/makefiletools/bin/mmapparser /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/$(CHIP)/mmap/$(MMAP) $(CHIP) E_LX_MEM3 phyaddr)
KERNEL_MEMLEN3 = $(shell /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/makefiletools/bin/mmapparser /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/$(CHIP)/mmap/$(MMAP) $(CHIP) E_LX_MEM3 size)
LOGO_ADDR = Not support chip
P3 = y
CHIP = p3
DISP = y
PRODUCT = disp
NO_WIFI = y
SIGMA_WIFI = no_wifi
BOARD = 020A
BOARD_NAME = SSC021A-S01B
TOOLCHAIN_GLIBC = y
TOOLCHAIN = glibc
TOOLCHAIN_VERSION_9.1.0 = y
TOOLCHAIN_VERSION = 9.1.0
TOOLCHAIN_REL_ARM-LINUX-GNUEABIHF-SIGMASTAR-9.1.0 = y
TOOLCHAIN_REL = arm-linux-gnueabihf-sigmastar-9.1.0
BUSYBOX = busybox-1.20.2-arm-linux-gnueabihf-glibc-sigmastar-9.1.0-dynamic
KERNEL_VERSION_4.9.84 = y
KERNEL_VERSION = 4.9.84
KERNEL_CONFIG = pioneer3_ssc021a_s01a_spinand_demo_defconfig
KERNEL_BOOT_ENV = LX_MEM=0x3FE0000 mma_heap=mma_heap_name0,miu=0,sz=0x1E00000 cma=2M highres=off $(KERNEL_RESERVED_ENV)
EXBOOTARGS =
IMAGE_CONFIG = spinand.ubifs.partition.dualenv.dispcam.config
FLASH_SIZE_NONE = y
FLASH_SIZE =
MMAP = MMAP_P3_64.h
IQ0 = gc1054/gc1054_iqfile.bin
IQ1 = gc1054/gc1054_iqfile.bin
IQ2 = gc1054/gc1054_iqfile.bin
IQ3 = gc1054/gc1054_iqfile.bin
IQ_API0 = gc1054/gc1054_api.bin
IQ_API1 = gc1054/gc1054_api.bin
SENSOR_LIST =
SENSOR0 =
SENSOR0_OPT =
SENSOR1 =
SENSOR1_OPT =
SENSOR2 =
SENSOR2_OPT =
mi_dbg = y
MI_DBG = 1
SUPPORT_SMART_DISPLAY_VDEC = 0
support_msos_mpool_add_pa2varange = y
SUPPORT_MsOS_MPool_Add_PA2VARange = 1
support_disp_align_up_offset32 = y
SUPPORT_DISP_ALIGN_UP_OFFSET32 = 1
SUPPORT_HDMI_VGA_DIRECT_MODE = 0
SUPPORT_DIVP_USE_GE_SCALING_UP = 0
SUPPORT_VIF_USE_GE_FILL_BUF = 0
support_stub_device_for_test_sys = y
SUPPORT_STUB_DEVICE_FOR_TEST_SYS = 1
SUPPORT_VDEC_MULTI_RES = 0
SUPPORT_DIVP_P2_MODE = 0
FB_VIDEO = 0
CONFIG_MI_ENABLE_VB_POOL = 0
config_mi_enable_meta_pool = y
CONFIG_MI_ENABLE_META_POOL = 1
config_mi_enable_sys_cfg = y
CONFIG_MI_ENABLE_SYS_CFG = 1
CONFIG_MI_ENABLE_SHRINKABLE_POOL = 0
config_mi_enable_ringheap_pool = y
CONFIG_MI_ENABLE_RINGHEAP_POOL = 1
is_demo_board = y
IS_DEMO_BOARD = 3
config_mi_venc_enable_jpeg = y
CONFIG_MI_VENC_ENABLE_JPEG = 1
config_mi_venc_enable_h26x = y
CONFIG_MI_VENC_ENABLE_H26X = 1
FPGA = 0
BENCH = no
MHAL =
MERGE_BOOT =
BOOTLOGO_FILE =
LOGO_ADDR =
BOOTLOGO_BUFSIZE =
UPGRADE_FILE =
DISP_OUT_NAME = SAT070CP50
FBDEV =
WIFI_MODULE =
SPEECH_VENDOR =
ALINK =
DUAL_OS = off
FAST_DEMO = off
SENSOR_TYPE0 =
SENSOR_TYPE1 =
LINUX_MEM_SIZE =
RTOS_BIN =
RTOS_LOAD_ADDR =
MMA_BASE =
MMA_SIZE =
verify_ai = disable
verify_ao = disable
verify_cipher = disable
verify_disp = disable
verify_divp = disable
verify_dla = disable
verify_fb = disable
verify_gfx = disable
verify_hdmi = disable
verify_ipu = disable
verify_jpd = disable
verify_shadow = disable
verify_sys = disable
verify_uvc_uac = disable
verify_uac = disable
verify_vdec = disable
verify_vdisp = disable
verify_venc = disable
verify_vif = disable
verify_vpe = disable
verify_warp = disable
verify_wlan = disable
verify_mdb = disable
verify_mi_demo = disable
verify_mixer = disable
verify_py_ipu = disable
verify_zk_bootup = disable
verify_zk_mini = disable
verify_zk_full = disable
verify_zk_mini_nosensor = disable
verify_zk_mini_fastboot = disable
verify_zk_full_fastboot = disable
verify_zk_mini_nosensor_fastboot = disable
verify_qfn68_sensor_panel = disable
verify_barcode = disable
verify_disp_pic_fastboot = disable
verify_usbcamera_fastboot = disable
verify_usbcamera = disable
verify_usbcam_fastboot = disable
verify_barcodeYuyan = disable
verify_jpeg2disp = disable
INTERFACE_AI = y
interface_ai = enable
interface_alsa = disable
INTERFACE_AO = y
interface_ao = enable
interface_bar = disable
interface_cipher = disable
INTERFACE_COMMON = y
interface_common = enable
interface_cus3a = disable
INTERFACE_DISP = y
interface_disp = enable
INTERFACE_DIVP = y
interface_divp = enable
INTERFACE_PSPI = y
interface_pspi = enable
interface_foo = disable
INTERFACE_GFX = y
interface_gfx = enable
interface_gyro = disable
interface_hdmi = disable
interface_ipu = disable
interface_iqserver = disable
interface_isp = disable
interface_ispalgo = disable
interface_jpd = disable
interface_ldc = disable
interface_mipitx = disable
INTERFACE_PANEL = y
interface_panel = enable
INTERFACE_RGN = y
interface_rgn = enable
interface_sed = disable
interface_sensor = disable
interface_shadow = disable
INTERFACE_SYS = y
interface_sys = enable
interface_vdec = disable
interface_vdf = disable
interface_vdisp = disable
INTERFACE_VENC = y
interface_venc = enable
interface_vif = disable
interface_vpe = disable
interface_warp = disable
INTERFACE_WLAN = y
interface_wlan = enable
INTERFACE_FB = y
interface_fb = enable
misc_fbdev = disable
MISC_CONFIG_TOOL = y
misc_config_tool = enable
MHAL_AIO = y
mhal_aio = enable
MHAL_CMDQ_SERVICE = y
mhal_cmdq_service = enable
mhal_csi = disable
MHAL_DISP = y
mhal_disp = enable
MHAL_DIVP = y
mhal_divp = enable
mhal_earlyinit = disable
MHAL_GE = y
mhal_ge = enable
mhal_hdmitx = disable
MHAL_IMI_HEAP = y
mhal_imi_heap = enable
MHAL_INIT = y
mhal_init = enable
mhal_ipu = disable
mhal_isp = disable
mhal_ispalgo = disable
mhal_ispmid = disable
mhal_ispscl = disable
mhal_jpd = disable
mhal_ldc = disable
mhal_mipitx = disable
mhal_mload = disable
MHAL_NEON = y
mhal_neon = enable
MHAL_PANEL = y
mhal_panel = enable
MHAL_RGN = y
mhal_rgn = enable
mhal_sensorif = disable
MHAL_VCODEC = y
mhal_vcodec = enable
mhal_vif = disable
mhal_vpe = disable
ARCH=arm
CROSS_COMPILE=arm-linux-gnueabihf-sigmastar-9.1.0-
CHIP_ALIAS = ikayaki
CHIP_FULL_NAME = pioneer3
PREFIX =$(TOOLCHAIN_REL)-
AS = $(PREFIX)as
CC = $(PREFIX)gcc
CXX = $(PREFIX)g++
CPP = $(PREFIX)cpp
AR = $(PREFIX)ar
LD = $(PREFIX)ld
STRIP = $(PREFIX)strip
export ARCH CROSS_COMPILE
KERNEL_RESERVED_ENV = mmap_reserved=fb,miu=0,sz=0x300000,max_start_off=0x3C00000,max_end_off=0x3F00000
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/project$

```

3. Compilación de la imagen final

Después de configurar con éxito, puede comenzar a compilar ejecutando el comando `make image` en el directorio del proyecto. Espere a que la compilación finalice y se generará una imagen completa para grabar.

```bash
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/project$ make image
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/../kernel/modules /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/../kernel/arch /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/../kernel/drivers /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/../kernel/include /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/../kernel/scripts /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/../kernel/Makefile /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/../kernel/Module.symvers /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/../kernel/.sstar_chip.txt /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/../kernel/.config /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/tools/usr /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/
ln -sf /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/customize/4.9.84/p3/disp /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/kbuild/4.9.84/customize
make headfile_link
make[1]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project'
'/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/release/include/mi_isp_general.h' -> '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/release/include/isp/mi_isp_general.h'
......
Brief USAGE:
   /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/makefiletools/scri
                                        pt/../../build/mkfwfs  {-c
                                        <pack_dir>|-u <dest_dir>|-l} [-d
                                        <0-5>] [-a] [-m <number>] [-C
                                        <number>] [-D <number>] [-P
                                        <number>] [-e <number>] [-y
                                        <number>] [-X <number>] [-x
                                        <number>] [-w <number>] [-r
                                        <number>] [-B <number>] [-b
                                        <number>] [-p <number>] [-s
                                        <number>] [--] [--version] [-h]
                                        <image_file>

For complete USAGE and HELP type:
   /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/makefiletools/script/../../build/mkfwfs --help

image size 605253
'/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/p3/boot/spinand/partition/flash.sni' -> '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/flash.sni'
/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/makefiletools/bin/pnigenerator -c 1024 -s 0x20000 -a "0x140000(CIS),0x60000(IPL),0x60000(IPL_CUST),0xe0000(UBOOT)" -b "0x60000(IPL),0x60000(IPL_CUST),0xe0000(UBOOT)" -t "0x40000(ENV),0x40000(ENV1),0x20000(KEY_CUST),0x500000(KERNEL),0x500000(RECOVERY),0x600000(rootfs),0xa0000(MISC),-(UBI)" -o /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/partinfo.pni      \
                 > /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/partition_layout.txt
[[boot_nofsimage]]
make boot_images
make[4]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
cat /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/partition_layout.txt
BOOT0: 0x140000(CIS),0x60000(IPL),0x60000(IPL_CUST),0xe0000(UBOOT)
BOOT1: 0x60000(IPL),0x60000(IPL_CUST),0xe0000(UBOOT)
SYS: 0x40000(ENV),0x40000(ENV1),0x20000(KEY_CUST),0x500000(KERNEL),0x500000(RECOVERY),0x600000(rootfs),0xa0000(MISC),-(UBI)
FLASH HAS USED 0x8000000KB
ChkSum       : 8081
Magic        : SSTARSEMICIS0001
Checksum ok!!
IDX:         StartBlk:           BlkCnt:     Trunk/BkTrunk:     Active:     BBM:      Name:
  0:    0,(0000000000)   10,(0X00140000)                0/1           1      OFF        CIS
  1:   10,(0X00140000)    3,(0X00060000)                0/1           1      OFF        IPL
  2:   13,(0X001A0000)    3,(0X00060000)                0/1           1      OFF   IPL_CUST
  3:   16,(0X00200000)    7,(0X000E0000)                0/1           1      OFF      UBOOT
  4:   23,(0X002E0000)    3,(0X00060000)                1/0           0      OFF        IPL
  5:   26,(0X00340000)    3,(0X00060000)                1/0           0      OFF   IPL_CUST
  6:   29,(0X003A0000)    7,(0X000E0000)                1/0           0      OFF      UBOOT
  7:   36,(0X00480000)    2,(0X00040000)                0/0           1      OFF        ENV
  8:   38,(0X004C0000)    2,(0X00040000)                0/0           1      OFF       ENV1
  9:   40,(0X00500000)    1,(0X00020000)                0/0           1      OFF   KEY_CUST
 10:   41,(0X00520000)   40,(0X00500000)                0/0           1      OFF     KERNEL
 11:   81,(0X00A20000)   40,(0X00500000)                0/0           1      OFF   RECOVERY
 12:  121,(0X00F20000)   48,(0X00600000)                0/0           1      OFF     rootfs
 13:  169,(0X01520000)    5,(0X000A0000)                0/0           1      OFF       MISC
 14:  174,(0X015C0000)  850,(0X06A40000)                0/0           1      OFF        UBI
if [ "spinand" = "spinand" ]; then      \
                flash_page_offset=$[0x23]; flash_page_size=8; printf "%x: %02x" ${flash_page_offset} ${flash_page_size} | xxd -r - /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/flash.sni;     \
        loop=$[$(stat -c "%s" /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/flash.sni)/512-1]; for i in `seq 0 ${loop}`;do blk_pb0_off=$[${i}*512+0x2F]; blk_pb1_off=$[${i}*512+0x30]; printf "%x: %02x" ${blk_pb0_off} 10 | xxd -r - /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/flash.sni; printf "%x: %02x" ${blk_pb1_off} 23 | xxd -r - /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/flash.sni; done;    \
        loop=$[$(stat -c "%s" /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/flash_list.sni)/512-1]; for i in `seq 0 ${loop}`;do blk_pb0_off=$[${i}*512+0x2F]; blk_pb1_off=$[${i}*512+0x30]; printf "%x: %02x" ${blk_pb0_off} 10 | xxd -r - /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/flash_list.sni; printf "%x: %02x" ${blk_pb1_off} 23 | xxd -r - /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/flash_list.sni; done;     \
fi;
[================================================================/                                                                    ] 24/49  48%make ipl_mkboot ipl_cust_mkboot uboot_mkboot
make[5]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
[====================================================================================================================================/] 49/49 100%

Exportable Squashfs 4.0 filesystem, xz compressed, data block size 131072
        compressed data, compressed metadata, compressed fragments, compressed xattrs
        duplicates are removed
Filesystem size 1800.93 Kbytes (1.76 Mbytes)
        47.58% of uncompressed filesystem size (3784.79 Kbytes)
Inode table size 1492 bytes (1.46 Kbytes)
        9.64% of uncompressed inode table size (15477 bytes)
Directory table size 3458 bytes (3.38 Kbytes)
        57.20% of uncompressed directory table size (6045 bytes)
Number of duplicate files found 0
Number of inodes 414
Number of files 25
Number of fragments 5
Number of symbolic links  366
Number of device nodes 0
Number of fifo nodes 0
Number of socket nodes 0
Number of directories 23
Number of ids (unique uids + gids) 1
Number of uids 1
        root (0)
Number of gids 1
        root (0)
dd if=/dev/zero bs=131072 count=1 | tr '\000' '\377' > /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_mkboot;
dd if=/dev/zero bs=131072 count=1 | tr '\000' '\377' > /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_cust_mkboot;
dd if=/dev/zero bs=393216 count=1 | tr '\000' '\377' > /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/uboot_mkboot;
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.000785679 s, 167 MB/s
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.00157433 s, 83.3 MB/s
dd if=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/p3/boot/spinand/ipl/IPL_CUST.bin of=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_cust_mkboot bs=131072 count=1 conv=notrunc seek=0;
dd if=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/p3/boot/spinand/ipl/IPL.bin of=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_mkboot bs=131072 count=1 conv=notrunc seek=0;
1+0 records in
1+0 records out
393216 bytes (393 kB, 384 KiB) copied, 0.00284586 s, 138 MB/s
dd if=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/p3/boot/spinand/uboot/u-boot_dualenv_spinand.xz.img.bin of=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/uboot_mkboot bs=393216 count=1 conv=notrunc seek=0;
0+1 records in
0+1 records out
31760 bytes (32 kB, 31 KiB) copied, 0.000403474 s, 78.7 MB/s
if [ "3" != "" ]; then \
        for((Row=1;Row<3;Row++));do \
                dd if=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_cust_mkboot of=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_cust_mkboot bs=131072 count=1 conv=notrunc seek=${Row}; \
        done; \
fi;
0+1 records in
0+1 records out
29200 bytes (29 kB, 29 KiB) copied, 0.000393671 s, 74.2 MB/s
if [ "3" != "" ]; then \
        for((Row=1;Row<3;Row++));do \
                dd if=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_mkboot of=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_mkboot bs=131072 count=1 conv=notrunc seek=${Row}; \
        done; \
fi;
0+1 records in
0+1 records out
271312 bytes (271 kB, 265 KiB) copied, 0.000833338 s, 326 MB/s
if [ "1" != "" ]; then \
        for((Row=1;Row<1;Row++));do \
                dd if=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/uboot_mkboot of=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/uboot_mkboot bs=393216 count=1 conv=notrunc seek=${Row}; \
        done; \
fi;
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.000951708 s, 138 MB/s
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.000823161 s, 159 MB/s
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.000792032 s, 165 MB/s
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.000828601 s, 158 MB/s
make[5]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
cat /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_mkboot /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_cust_mkboot /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/uboot_mkboot > /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot.bin
rm -rfv /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_mkboot /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_cust_mkboot /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/uboot_mkboot
removed '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_mkboot'
removed '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/ipl_cust_mkboot'
removed '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/uboot_mkboot'
make[4]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
dd if=/dev/zero bs=2048 count=2 | tr '\000' '\377' > /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/cis.bin
2+0 records in
2+0 records out
4096 bytes (4.1 kB, 4.0 KiB) copied, 7.7572e-05 s, 52.8 MB/s
dd if=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/flash.sni of=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/cis.bin bs=2048 count=1 conv=notrunc seek=0
1+0 records in
1+0 records out
2048 bytes (2.0 kB, 2.0 KiB) copied, 0.000287638 s, 7.1 MB/s
dd if=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/partinfo.pni of=/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/cis.bin bs=2048 count=1 conv=notrunc seek=1
0+1 records in
0+1 records out
2008 bytes (2.0 kB, 2.0 KiB) copied, 0.000260181 s, 7.7 MB/s
cat /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/boot/flash_list.sni >> /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/cis.bin
make[3]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
make scripts
make[3]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
mkdir -p /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/scripts
make set_partition
make[4]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
make[4]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
make cis_spinand__script boot_spinand__script kernel_spinand__script rootfs_spinand_squashfs_script misc_spinand_fwfs_script miservice_spinand_ubifs_script customer_spinand_ubifs_script appconfigs_spinand_ubifs_script spinand_config_script
make[4]: Entering directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
#@echo -e ubi create rootfs 0x600000\\n ubi create misc 0xa0000\\n ubi create miservice 0xC00000\\n ubi create customer 0x5000000\\n ubi create appconfigs 0x4C0000\\n >> /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/scripts/[[ipl.es
#@echo -e ubi create rootfs 0x600000\\n ubi create misc 0xa0000\\n ubi create miservice 0xC00000\\n ubi create customer 0x5000000\\n ubi create appconfigs 0x4C0000\\n >> /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/scripts/[[ipl.es
#@echo ubi create appconfigs 0x4C0000 >> /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/scripts/[[appconfigs.es
#@echo ubi create miservice 0xC00000 >> /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/scripts/[[miservice.es
#@echo -e ubi create rootfs 0x600000\\n ubi create misc 0xa0000\\n ubi create miservice 0xC00000\\n ubi create customer 0x5000000\\n ubi create appconfigs 0x4C0000\\n >> /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/scripts/[[ipl.es
#@echo ubi create customer 0x5000000 >> /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/scripts/[[customer.es
if [ -a ../parser_IPL.sh ] ; \
then \
        sh ../parser_IPL.sh /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/board/p3/boot/spinand/ipl/IPL.bin /home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image/output/images/scripts ;\
fi;
kernel-image done!!!
../parser_IPL.sh: line 1: warning: command substitution: ignored null byte in input
make[4]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
make[3]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
make[2]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project/image'
./split_partion.sh
split customer image
customer.es is not over size,do nothing!!
make[1]: Leaving directory '/home/book/DongshanPI-PicoW/IKAYAKI_SDK/project'
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/project$

```

Solución a errores de empaquetado:
```
vim image/makefiletools/script/fwfs_pack.py
Modificar línea 120, reemplazando `str(image_size)` por `str(int(image_size))`
```

4. Copiar y grabar la imagen
Después de la compilación, la imagen se encuentra en el directorio `project/image/output/images`. Copie todos los archivos de imagen a la carpeta de imagen del sistema predeterminada y reemplace los archivos anteriores. Luego, reinicie la placa de desarrollo en modo de grabación para actualizar el sistema con la imagen más reciente que acaba de compilar.
```bash
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/project$ ls image/output/images/
appconfigs.ubifs     auto_update.txt  boot.bin  customer.ubifs  misc.fwfs        partition_layout.txt  scripts      usb_updater_boot.bin
auto_update_bin.txt  boot             cis.bin   kernel          miservice.ubifs  rootfs.sqfs           scripts_bin  usb_updater_ipl.bin
book@virtual-machine:~/DongshanPI-PicoW/IKAYAKI_SDK/project$
```

