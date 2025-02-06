
 Haciendo uso de la herramienta RP imager v1.8.5-2 instalada desde el repositorio oficial de fedora ,ejecutandose en un slimbook prox15 corriendo fedora workstation 41, instalo la versión de 64 bit de RpiOS Lite lanzada el 2024-11-19 [link](https://downloads.raspberrypi.com/raspios_lite_arm64/images/raspios_lite_arm64-2024-11-19/) con la siguiente configuración:
 - Dispositivo: Rpi 5.
 - Sistema operativo: Rpi Lite
 - Lector de tarjetas interno.
- nombre de anfitrión: raspberrypi1.local
- usuario y contraseña: user1
-  zona horaria: madrid
- teclado: es 
- ssh: por contraseñas
- telemetria desactivada
-lan inalambrica: configurada(españa) para mi movil ()intnción de usar cableada).


mientras se intalaba el soft en la tarjeta micro sd , he ido colocando las patas de goma en la carcasa (NO he colocado el disipador pero se ha conectado el ventilador en la RPi por ahora) y montado la rpi en la carcasa (sin atornillar).

 Uso wave terminal para conectarme mediante ssh a la rpi5 (nota en mi fedora tuve que usar este [fix](https://github.com/wavetermdev/waveterm/issues/654#issuecomment-2113017898) para usar wave)
usando la siguiente configuración



