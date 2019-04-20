# Activitats per practicar

## Enunciats

1. Crea un projecte anomenat exercici01. Inicialitza'l amb el box **alpine/alpine64** en mode mínim i aixeca la màquina. Obre l'arxiu *Vagrantfile* i fixa't a l'estructura de la configuració. Connecta't ara via ssh i comprova amb quin usuari es fa la connexió. Comprova l'existència de la carpeta compartida per defecte. Tanca la connexió ssh i a continuació, mira l'estat (*status*) de la instància Vagrant. Ara, indica a Vagrant que aturi la màquina, observa l'error que surt i comprova si la màquina s'ha aturat. Força l'aturada de la màquina. [Solució](solucions/exercici01.md)

2. Obre la màquina Alpine o crea una de nou mitjançant el box **alpine/alpine64**. Connectat via ssh i instal·la *apache2*, podeu seguir la següent guia <https://wiki.alpinelinux.org/wiki/Apache>. Configura l'arxiu *Vagrantfile* per a poder connectar-se aquest servidor utilitzant el navegador del teu ordinador. Comprova, que hi funciona. [Solució](solucions/exercici02.md)

3. Crea un projecte anomenat exercici03 usant la inicialització mínima de **generic/ubuntu1804**. Editeu l'arxiu Vagranfile de per tal que tingui les següents configuracions:

    * Mapeja a la màquina virtual a la carpeta /home/vagrant una carpeta a *Documentos* que anomenaràs *exercici03*.
    
    * Rediregeix el port 443 de la màquina cap el port 4300 de la màquina física.

    * Desactivar l'actualització automàtica del box.

[Solució](solucions/exercici03.md)
