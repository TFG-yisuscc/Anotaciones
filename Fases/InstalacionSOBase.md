# Instalación del SO y drivers
## Parte común
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
mientras se intalaba el soft en la tarjeta micro sd , he ido colocando las patas de goma en la carcasa (NO he colocado el disipador y se ha desconectado el ventilador en la RPi por ahora) y montado la rpi en la carcasa (sin atornillar) para protegerla .
#primer intento
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
No funciona, procedo a  hacer otra instalacíon limpia y reintentarlo.
# Segundo intento

 Reinstalo en la sd el  mismo SO siguiendo los mismos pasos,  esta vez con el AI kit  ya instalado (verificando que tuviera los conectores)  me conector por ssh (misma configuración) y procedo a seguir los pasos de la [guía de hailo](https://github.com/hailo-ai/hailo-rpi5-examples/blob/main/doc/install-raspberry-pi5.md) previamente habiendo actualizado la clave ssh en mi dispositivo 
 ejecuto los siguientes comandos
 ```bash 
sudo apt update && sudo apt full-upgrade
#en teoria por reproucibilidad creo que deberiamos evitar el full-upgrade
# ya que actualiza la distro al completo 

# compruebo el firmware del bootloader 
sudo rpi-eeprom-update
# no es necesario actualizar
sudo apt install hailo-all
# he tenido un apagon mientras se instalaba
#ejecuto sdo dpkg --configure - a
sudo dpkg --configure - a
sudo apt install hailo-all
#activamos el pcie3 y reiniciamos
sudo raspi-config
# comprobamos 
hailortcli fw-control identify
 ```
 Esto devuelve: 
> Executing on device: 0000:01:00.0
> Identifying board
> Control Protocol Version: 2
> Firmware Version: 4.20.0 (release,app,extended context switch buffer)
> Logger Version: 0
> Board Name: Hailo-8
> Device Architecture: HAILO8L
> Serial Number: HLDDLBB243500705
> Part Number: HM21LB1C2LAE
> Product Name: HAILO-8L AI ACC M.2 B+M KEY MODULE EXT TMP
Se considera la instalación existosa y se pasa al siguente punto del proyecto 
instalar los componentes de la IA(mientras que creo un script con la instalación del software y los drivers).
Aparentemente funciona todos los componentes-> desconecto la hailo board y procedo  aha hacer la instalación de ollama 




