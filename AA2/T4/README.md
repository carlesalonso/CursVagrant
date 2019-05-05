# Configurant l'arxiu Vagrantfile

En aquest punt ens centrarem a l'arxiu Vagrantfile, la seva estructura i  la sintaxi correcta per la seva configuració.

L'arxiu Vagrantfile es crea en el moment d'inicialitzar el projecte amb *vagrant init*, en funció de si hem triat l'opció mínima o no, aquest arxiu es crearà sense o amb els comentaris.

Vagrant s'ha desenvolupat utilitzant Ruby, de manera que l'arxiu Vagrantfile no és res més que un arxiu en llenguatge Ruby. La configuració es troba dins del bloc *configure* que finalitza amb un *end* i que indica la versió de Vagrant per la que s'ha definit l'arxiu. Dins d'aquest bloc de configuració és on definirem tots els aspectes de la màquina: box, xarxa, sistema d'arxiu, aprovisionament, etc.

```ruby
    Vagrant.configure("2") do |config|

    end
```

## Configuració de la màquina Vagrant (config.vm)

Utilizant l'espai de noms *config.vm*, s'especificaran les configuracions de la màquina Vagrant. Les configuracions més importants són:

* *config.vm.boot_timeout*: especifica el temps (en segons) que s'espera Vagrant un cop indica al virtualitzador (proveïdor) per utiltizar-la. Si passat aquest temps no és capaç de comunicar-se amb ella, ens sortirà un missatge de *time out*. Per defecte el valor és de 300 segons.

* *config.vm.box*: serveix per indicar quin box s'utilitzarà. Aquest box pot estar ja instal·lat o es cercarà a Vagrant Cloud.

* *config.vm.box_check_update*: si es posa en true, cada cop que iniciem la màquina comprovarà si el box està actualitzat i si no ho està, descarregarà un box nou. No tots els boxes es poden actualitzar automàticament, però cal tenir-ho en compte per no haver-se d'esperar quan es vol aixecar un entorn de treball.

* *config.vm.box_url*: en aquest cas, enlloc d'indicar el nom del box i que per tant només ho pugui cercar a l'equip o al cloud de Vagrant, li passem una URL.

* *config.vm.box_version*: quan tenim diverses versions d'un box, podem triar quina versió utilitzem. Pot ser útil per fer proves de compatibilitat.

* *config.vm.box_check_update*: aquesta permet definir si volem que Vagrant comprovi si s'actualitzat el box cada cop que arranquem la instància. Això que per una banda és interessant perquè ens assegura tenir el box actualitzat, comporta dues pegues: els box es van descarregant però no se sobreescriuen i segon, pot fer que l'arrancada sigui molt lenta.

* *config.vm.communicator*: configura quin serà el mecanisme que utilitzarà per permetre iniciar sessió a la màquina Vagrant. Per defecte és el protocol ssh, però es pot utilitzar també *winrm* (escriptori remot) per màquines Windows.

* *config.vm.guest*: permet indicar quin SO tindrà la màquina Vagrant. Normalment, Vagrant ja és capaç de detectar automàticament el sistema.

* *config.vm.network*: serveix per configurar la xarxa. Per defecte si usem VirtualBox s'autoconfigura en mode NAT i reenviament del port 22 al 2222, però més endavant veurem com podem definir xarxes privades, públiques i reenviar altres ports. Al punt anterior hem vist un exemple senzill de reenviament de port per permetre exposar el port 80 de la màquina Vagrant.

* *config.vm.synced_folder*: permet definir les carpetes compartides entre la instància virtual i la màquina física. Per defecte la carpeta del projecte es mapeja com */vagrant* a dins la instància. Aquest aspecte també el tractarem amb més detall més endavant, però al punt anterior ja s'ha vist com mapejar una altra carpeta.

* *config.vm.provider*: aquest bloc serveix per definir les especificacions del provider (virtualitzador). Cada provider suporta diferents valors i configuracions. Per exemple, el més popular VirtualBox, es pot especificar la quantitat de RAM *memory*, el nombre de cores *cpus* i si volem que s'arrenqui el GUI de VirtualBox *gui*.

## Configuració SSH (config.ssh)

En aquest bloc es defineixen els paràmetres de la connexió ssh, per exemple, la definició del certificat per fer la connexió, l'opció de connexió via usuari/contrasenya, el port, etc. Normalment, no caldrà que ens preocupem de modificar aquest bloc de l'arxiu.

La part final de la configuració correspon als aprovisionadors, que també veurem amb més detall, però que ara per donar una petita pinzellada, permeten configurar la màquina creada. Això ens permet no haver de tenir diferents boxes d'Ubuntu segons el que volem instal·lar, si no un únic box (amb l'estalvi d'espai que això implica) i definir les configuracions internes mitjançant aquest aprovisionament.

Clicant sobre l'enllaç podeu veure un exemple d'arxiu [Vagrantfile](Vagrantfile) bàsic.

Un cop editat l'arxiu *Vagrantfile* si voleu comprovar que no té errors sintàctics podeu fer servir la comanda:

```bash
    vagrant validate
```

Si el fitxer no té errors retornarà el següent missatge:

```bash
    Vagrantfile validated successfully.
```

Cas contrari mostra un missatge d'errors i el lloc on troba el problema, compte que pot ser que l'error hi sigui abans:

```bash
    Vagrant failed to initialize at a very early stage:

    There is a syntax error in the following Vagrantfile. The syntax error message is reproduced below for convenience:

    /Users/cam/Documents/Vagrant/examen/Vagrantfile:9: syntax error, unexpected tIDENTIFIER, expecting keyword_end
    config.vm.provision "shell", inline: <<-SHELL
                               ^
    /Users/cam/Documents/Vagrant/examen/Vagrantfile:9: unterminated string meets end of file
```

[Tornar a índex AA2](../README.md)

[Activitats per practicar](../X)
