# Introducció a Vagrant

Vagrant és una eina pensada per simplificar la feina de crear entorns virtuals de desenvolupament. Quan es programa, és molt important desenvolupar en un entorn amb les mateixes característiques que el de producció, per exemple, quan es creen aplicacions web en PHP, és necessari utilitzar la mateixa versió tant en desenvolupament com en producció, per evitar problemes de incompatibilitats, instruccions que canvien, etc.

![Works in my machine](../pics/WorkInMyMachine.jpg)

El problema és que si treballem amb diferents entorns de producció, caldria anar modificant les configuracions del nostre ordinador per adaptar-lo als diferents casos. La solució per evitar això, és la virtualització. Usant màquines virtuals puc recrear en el meu equip un sistema que tingui les mateixes característiques: sistema operatiu, framework, etc. que la màquina on finalment s'executarà l'aplicació.

Configurar entorns virtuals, pot ser una tasca complexa i repetitiva, de manera que voldríem disposar d'alguna eina que ens permeti desplegar aquests entorns virtuals d'una forma senzilla i automatitzable.

Vagrant és una eina que ens permetrà realitzar aquesta automatització d'entorns virtuals, situant-se en una capa superior als virtualitzadors. 