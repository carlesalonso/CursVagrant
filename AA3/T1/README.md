# Xarxes

Aquí veurem les diferents opcions a nivell de xarxa que ofereix Vagrant. Quan usem VirtualBox o VMware com virtualitzadors, el mode de xarxa per defecte és NAT, que significa que la instància virtual pot sortir a Internet a través de la màquina física, però sense que es puguin comunicar entre elles. És el mode més senzill perquè el virtualitzador proporciona configuració de xarxa automàtica als equips client.

## Reenviament de ports

Com hem dit, en el mode per defecte la instància virtual és invisible tant a l'equip hoste, com òbviament a la resta de la xarxa. Si volem connectar-nos a la màquina virtual perquè estem implementant un servidor web, ldap o el que sigui, necessitem fer visibles els ports concrets de la màquina Vagrant. Per fer això, usarem el reenviament de ports. El procediment és força senzill, editarem l'arxiu Vangrantfile i afegirem la seüent línia a la configuració:

```ruby
    config.vm.network "forwarded_port", guest: 80, host: 8000
```

Aquí estem dient que redirigim el port 80 de la màquina Vagrant cap el port 8000 de la màquina física, de manera que si volem veure el servei web de la instància virtual podrem utilitzar el navegador de la màquina física fent [*http://localhost:8000*](xarxes.md).

Un problema que es pot donar si treballem amb diversos entorns Vagrant a l'hora és que intentin utilitzar el mateix port de la màquina física, si volem evitar col·lisions, es pot afegir el paràmetre *auto_correct* que comprovarà en el moment d'aixecar la màquina els conflictes i ens avisarà del nou port assignat.

```ruby
    config.vm.network "forwarded_port", guest: 80, host: 8000, auto_correct: true
```

## Xarxes privades

El model de xarxa privada es correpon amb el "host-only" de VirtualBox. Les xarxes privades serveixen per poder definir una xarxa interna que permetrà que dues o més màquines Vagrant es puguin comunicar, per exemple, podríem tenir una fent el rol de servidor web i l'altra fent de servidor de BD.

Quan definim una xarxa privada, tindrem dues interfaces a la màquina Vagrant, la corresponent a la configuració NAT, que és la que permet comunicar-se amb Internet i una xarxa privada. Aquesta xarxa privada, la podrem definir amb servidor DHCP, d'aquesta manera no ens haurem de preocupar de donar adreces o veurem que podem fer assignacions estàtiques.

Aquí veiem un exemple d'arxiu Vagrantfile per definir una xarxa privada:

```ruby
    Vagrant.configure("2") do |config|
    config.vm.box = "generic/ubuntu1804"
    config.vm.network "private_network", type: "dhcp"
    end
```

Un cop iniciada la màquina, si ens connectem per ssh i mirem la configuració de xarxa, veurem quelcom similar a això:

![private network](../pics/privateNetwork1.png)

on eth0 correspon a la interfície configurada amb NAT i eth1 a la xarxa privada.

Amb xarxa privada, no cal redirigir ports, ja que la màquina Vagrant i la màquina física comparteixen xarxa, de manera, que si es volgués connectar al servei web de la màquina Vagrant, simplement podríem fer [*http://172.28.128.3*](xarxes.md)

De la mateixa manera, podem definir la configuració de forma estàtica:

```ruby
    Vagrant.configure("2") do |config|
    config.vm.box = "generic/ubuntu1804"
    config.vm.network "private_network", ip: "10.10.10.10"
    end
```

## Xarxes públiques

És el model que correspon a *l'adaptador pont* de VirtualBox, això vol dir que la màquina Vagrant tindrà una adreça IP de la xarxa local on estiguem. Aquesta configuració és útil si necessitem que siguin visibles des de fora de la màquina física. De la mateixa manera que a la configuració anterior, podrem tenir dues opcions, utilitzar DHCP (sempre que la xarxa disposi d'un servidor de configuració de xarxa) o configuració estàtica.

Si tenim més d'un adaptador de xarxa, en el moment de l'arrancada ens preguntarà, quin dels disponibles es vol assignar o també es pot fer per codi:

```ruby
    Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/bionic64"
    config.vm.network "public_network", type: "dhcp", “bridge: "en0: Wi-Fi (AirPort)”
```

També es pot configurar en estàtic, però hi ha paràmetres que cal aprovisionar a la configuració de la màquina.

[<< Tornar a índex AA3](../README.md)

[>> AA3. Aprovisionament per scripts](../T2)