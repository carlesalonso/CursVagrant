# Instal·lació

## Pas previ

És necessari que tingueu instal·lat VirtualBox al vostre ordinador, preferentment actualitzat. Tot i que Vagrant està preparat per funcionar amb altres virtualitzadors com hyperV, sí que hi ha una sèrie de limitacions pel propi enfocament de hyperV que fan que sigui molt més senzill treballar amb VirtualBox.

## Instal·lació

Instal·lar Vagrant és molt senzill, únicament heu d'anar a la pàgina (<https://www.vagrantup.com/downloads.html)> i descarregar la versió adient pel vostre sistema (compte els usuaris de Windows, assegureu-vos de baixar la versió de 64 bits).

Un cop finalitzat el procés d'instal·lació i haver reiniciat la màquina, comproveu que Vagrant funciona correctament, per fer això, obriu un terminal: cmd, PowerShell o terminal de mac i mirem quina versió de Vagrant s'ha instal·lat:

Si tot ha funcionat correctament, ja tenim Vagrant disponible al nostre equip. Ara crearem l'espai on treballar, per això creeu una carpeta que anomenareu vagrant a l'arrel de la vostra carpeta personal. Com a consell, mai creeu aquest tipus de carpetes a l'escriptori de Windows.

Com a primer pas, baixarem un box, però fent-ho d'una forma una mica diferent de com es va fer a classe a la presentació i de com ho farem habitualment (al moment d'aixecar la instància amb el vagrant up). En aquesta ocasió anem a baixar el box directament per deixar-ho emmagatzemat a l'ordinador fins que aprovisionament la instància a classe.

El procediment serà el següent:

```bash
    vagrant box add generic/ubuntu1804
```

Abans de baixar, caldrà que trieu el proveïdor (provider) que vulgueu utilitzar (VirtualBox o Parallels). Un cop fet això, el box es descarregarà. Si us pregunteu on es guarda aquest box, la resposta, és que s'ha creat una carpeta .Vagrant en el vostre sistema.

Podem comprovar que la descàrrega s'ha realitzat exitosament llistant els box descarregats a la vostra màquina:

```bash
    vagrant box list
```

Amb aquesta comanda, se'ns mostren els boxes que tenim descarregats al nostre sistema. Més endavant, veurem com podem gestionar aquests boxes.

[<< Tornar a índex AA2](../README.md)

[>> AA2. Comandes](../T2)