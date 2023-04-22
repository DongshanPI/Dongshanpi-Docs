## Instalación y configuración del entorno de desarrollo

### Obtener el sistema de la máquina virtual

#### Descarga de la herramienta de máquina virtual VMware

Abre el navegador y visita el sitio web https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html. Haz clic en "DESCARGAR AHORA" como se muestra en la imagen a continuación, para descargar e instalar la versión de Windows de VMware Workstation.

![vmwareworkstation_download_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/vmwareworkstation_download_001.png)

Una vez que se complete la descarga, simplemente sigue los pasos de la instalación usando la configuración predeterminada.

#### Obtener la imagen del sistema Ubuntu

Visita https://www.linuxvmimages.com/images/ubuntu-1804/ usando el navegador y haz clic en "VMware Image" como se muestra en la imagen a continuación.

![linuxvmimage_downlaod_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/linuxvmimage_downlaod_001.png)

La descarga puede tardar entre 10 y 30 minutos, dependiendo de la velocidad de la conexión a Internet.

## Ejecutar el sistema de la máquina virtual

1. Descomprime el archivo comprimido de la imagen del sistema de la máquina virtual. Después de la descompresión, verás dos archivos. Usaremos el archivo de configuración con la extensión ".vmx".

![ConfigHost_003](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/ConfigHost_003.png)

2. Abre el software VMware Workstation instalado y haz clic en "Archivo" en la esquina superior izquierda y luego en "Abrir". Busca el archivo "Ubuntu_18.04.6_VM_LinuxVMImages.COM.vmx" y se abrirá una nueva ventana de diálogo de la máquina virtual.

3. La imagen de abajo es la interfaz de configuración de la máquina virtual. Haz clic en el recuadro rojo 2 para editar la configuración de la máquina virtual y ajusta el tamaño de la memoria y el número de procesadores. Se recomienda tener al menos 4GB de memoria y 4 procesadores. Una vez que hayas terminado de ajustar la configuración, haz clic en "Ejecutar esta máquina virtual" para ejecutarla.

![ConfigHost_005](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/ConfigHost_005.png)

La primera vez que abras la máquina virtual, aparecerá un cuadro de diálogo que pregunta si quieres copiar la máquina virtual. Haz clic en "He copiado" para continuar iniciando el sistema de la máquina virtual.

Después de unos segundos, el sistema se iniciará automáticamente. Haz clic en "Ubuntu" para acceder a la pantalla de inicio de sesión. Ingresa la contraseña "ubuntu" para iniciar sesión en el sistema Ubuntu.

Nota: 

El nombre de usuario y la contraseña predeterminados de Ubuntu son "ubuntu ubuntu".

El nombre de usuario y la contraseña predeterminados de Ubuntu son "ubuntu ubuntu".

El nombre de usuario y la contraseña predeterminados de Ubuntu son "ubuntu ubuntu".

**Nota:** _Si tu ordenador con Windows ya tiene acceso a Internet, la red se compartirá automáticamente con Ubuntu después de iniciarlo, permitiendo la conexión a Internet._



### Configurar el entorno de desarrollo

* Instalar los paquetes de software necesarios. Haga clic en la interfaz de Ubuntu con el mouse y presione **ctrl + alt + t** al mismo tiempo en el teclado para invocar rápidamente la interfaz de terminal. Después de que aparezca la ventana de terminal, ejecute el siguiente comando para instalar las dependencias necesarias:

```bash
sudo apt-get install -y sed make binutils build-essential gcc g++ bash patch gzip bzip2 perl tar cpio unzip rsync file bc wget python cvs git mercurial rsync subversion android-tools-mkbootimg vim libssl-dev android-tools-fastboot
```

Si encuentra que su máquina virtual de Ubuntu no puede copiar y pegar comandos desde Windows en la primera vez que inicia, es necesario ingresar el siguiente comando para instalar una herramienta que comparte el portapapeles entre la máquina virtual y Windows:

```bash
sudo apt install open-vm-tools-desktop 
```

![ConfigHost_007](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/ConfigHost_007.png)

Después de la instalación, haga clic en el botón de energía en la esquina superior derecha para reiniciar el sistema Ubuntu o ingrese el comando sudo reboot para reiniciar directamente.

Ahora puede pegar archivos desde Windows en Ubuntu o copiar archivos desde Ubuntu a Windows.

Después de completar este paso, puede continuar y obtener el código fuente para comenzar el desarrollo de la placa de desarrollo RISC-V Dongshan Nezha STU.