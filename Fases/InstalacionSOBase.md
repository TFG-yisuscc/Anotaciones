
 Haciendo uso de la herramienta RP imager v1.8.5-2 instalada desde el repositorio oficial de fedora ,ejecutandose en un slimbook prox15 corriendo fedora workstation 41, instalo la versión de 64 bit de RpiOS Lite lanzada el 2024-11-19 [link](https://downloads.raspberrypi.com/raspios_lite_arm64/images/raspios_lite_arm64-2024-11-19/) con la siguiente configuración:
 - Dispositivo: Rpi 5.
 - Sistema operativo: Rpi Lite
 - Lector de tarjetas interno.
- nombre de anfitrión: raspberrypi1.local
- usuario y contraseña: user1
- zona horaria: madrid
- teclado: es 
- ssh: por contraseñas
- telemetria desactivada
- LAN inalambrica: configurada(españa) para mi movil (intención de usar cableada).
mientras se intalaba el soft en la tarjeta micro sd , he ido colocando las patas de goma en la carcasa (NO he colocado el disipador pero se ha conectado el ventilador en la RPi por ahora) y montado la rpi en la carcasa (sin atornillar).
Siguiendo los preparativos de la guía del [AI KIT](https://www.raspberrypi.com/documentation/accessories/ai-kit.html#ai-module-features):

```bash
sudo apt update && sudo apt full-upgrade #actualiza el software a la última versión 
sudo rpi-eeprom-update # comprobamos el firmware
# firmware con fecha superior a 6 DE DICIEMBRE DE 2023 POR LO QUE NO ES NECESARIO ACTUALIZAR    
# (Mon 23 Sep 13:02:56 UTC 2024 (1727096576)
#¿Actualizo de tods modos? -> no por ahora 
```
Shigiendo la guía procedo a instalar el modulo ai kit ()divergo en las instalacíon en que coloco tambíen la parte inferío de la carcasa para evitar daños en la manipulación, pero no coloco la parte superior de la carcasa(se ha intentado quitando el ventilador de la carcasa pero al ver que el cable pce ocupaba mucho y era frágil, he desistido).
procedo a hacer la instalcación software del módulo ai kit siguiendo la siguiente [guia](https://www.raspberrypi.com/documentation/computers/ai.html)
```bash

sudo apt install hailo-all # trada mucho y me he equivocado de teclado varias veces espero no haber fastidadado la instalación
sudo reboot 
hailortcli fw-control identify
```
Sin embargo tras ejecutarl los siguientes comandos 
el terminal muestra las siguientes respuestas: 
 
``` bash
user1@raspberrypi1:~ $ hailortcli fw-control identify
[HailoRT] [error] Can't find hailort driver class. Can happen if the driver is not installed, if the kernel was updated or on some driver failure (then read driver dmesg log)
[HailoRT] [error] CHECK_SUCCESS failed with status=HAILO_DRIVER_NOT_INSTALLED(64) - Failed listing hailo devices
[HailoRT] [error] CHECK_SUCCESS failed with status=HAILO_DRIVER_NOT_INSTALLED(64)
[HailoRT] [error] CHECK_SUCCESS failed with status=HAILO_DRIVER_NOT_INSTALLED(64)
[HailoRT] [error] CHECK_SUCCESS failed with status=HAILO_DRIVER_NOT_INSTALLED(64)
[HailoRT] [error] CHECK_SUCCESS failed with status=HAILO_DRIVER_NOT_INSTALLED(64)
[HailoRT CLI] 

``` 
Procedo a volcar el resultado del siguinete comando: 
```bash 
user1@raspberrypi1:~ $ sudo apt install hailo-all
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
hailo-all is already the newest version (4.20.0).
The following package was automatically installed and is no longer required:
  libcamera0.3
Use 'sudo apt autoremove' to remove it.
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
#al esntar instalado  "correctamente" voy a ver los logs 
user1@raspberrypi1:~ $ dmesg | grep -i hailo
# no devuelve nada 
```
Procedo a seguir los siguientes pasos: 
1. Desisntalo completamente. hailo-all 
2. Activo el [pcle 3.0](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#pcie-gen-3-0) (aunque la rpi no está certificada)
3. Reinstalo hailo-all 
4. vuelvo a comprobar
```bash
# purgamos hailo -all 
sudo apt purge hailo-all 
#reiniciamos  
sudo reboot 
#compruebo  usando  hailortcli fw-control identify
 hailortcli fw-control identify
#sigue dando errores
#procedo a activar el pcie3 

#dtparam=pciex1_gen=3 #no sirve 
sudo nano /boot/firmware/config.txt
sudo reboot 
#reinstalo hailo-all 
sudo apt autoremove 
sudo reboot 
sudo apt install hailo-all
#reintento mediante sudo raspi-config
# reinicio
sudo reboot
```
No funcion, procedoa hacer otra instalacíon limpia y reintentarlo 

