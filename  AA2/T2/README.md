# Comandes bàsiques

La documentació oficial la teniu disponible a:

[docs.vagrantup.com](http://docs.vagrantup.com)

Ara veurem les primeres comandes de vagrant que necessitarem per començar a treballar, d'altres les anirem veient en els temes posteriors.

## Help

Vagrant funciona sobre terminal i té tota una sèrie de comandes i arguments. Per veure les opcions disponibles, és molt útil consultar l'ajuda:

```bash
vagrant -h

Usage: vagrant [options] <command> [<args>]

    -v, --version                    Print the version and exit.
	-h, --help                       Print this help.
		
Common commands:
	Common commands:
     box             manages boxes: installation, removal, etc.
     cloud           manages everything related to Vagrant Cloud
     destroy         stops and deletes all traces of the vagrant machine
     global-status   outputs status Vagrant environments for this user
     halt            stops the vagrant machine
     help            shows the help for a subcommand
     init            initializes a new Vagrant environment by creating a Vagrantfile
     login           
     package         packages a running vagrant environment into a box
     plugin          manages plugins: install, uninstall, update, etc.
     port            displays information about guest port mappings
     powershell      connects to machine via powershell remoting
     provision       provisions the vagrant machine
     push            deploys code in this environment to a configured destination
     rdp             connects to machine via RDP
     reload          restarts vagrant machine, loads new Vagrantfile configuration
     resume          resume a suspended vagrant machine
     snapshot        manages snapshots: saving, restoring, etc.
     ssh             connects to machine via SSH
     ssh-config      outputs OpenSSH valid configuration to connect to the machine
     status          outputs status of the vagrant machine
     suspend         suspends the machine
     up              starts and provisions the vagrant environment
     upload          upload to machine via communicator
     validate        validates the Vagrantfile
     version         prints current and latest Vagrant version
     winrm           executes commands on a machine via WinRM
     winrm-config    outputs WinRM configuration to connect to the machine


																																	 
For help on any individual command run `vagrant COMMAND -h`

Additional subcommands are available, but are either more advanced
or not commonly used. To see all subcommands, run the command
`vagrant list-commands`.
```

Per les diferents comandes podem obtenir l'ajuda disponible afegint el "-h" al final, així per exemple, si volem saber amb més detall les opcions de gestió amb els boxes, faríem:

```bash
    vagrant box -h
    vagrant help box
```

També podem buscar ajuda específica per una comanda, per exemple:

```bash
    vagrant box add -h
```

## version

Aquesta comanda serveix per conèixer la versió de Vagrant que heu instal·lat i us informa si és la darrera versió disponible.

```bash
    $vagrant version
        Installed Version: 2.2.4
        Latest Version: 2.2.4
```

## box

La comanda *box* permet gestionar els diferents boxes en el nostre sistema. Es podran instal·lar, actualitzar, esborrar i eliminar versions antigues. Tot i que més endavant ho veurem amb més detall, ara explicarem les opcions més habituals.

* *vagrant box add* nom_del_box: descarrega el box al nostre equip per poder ser utilitzat.

* *vagrant box list*: llista tots els boxes que tenim descarregats a l'equip.

* *vagrant box outdated*: permet comprovar si el box està desactualitzat.

* *vagrant box prune*: esborra les versions antigues de boxes instal·lats. És important tenir clar que quan es baixen les actualitzacions, no s'esborren per defecte els boxes anteriors. Per tant, és convenient fer aquesta acció per alliberar espai al disc.

* *vagrant box remove* nom_del_box: esborra el box indicat.

## init

Serveix per inicialitzar l'arxiu Vagrantfile, que serà la base del projecte. Existeixen diverses formes de fer aquesta inicialització:

```bash
    vagrant init
```

Això genera un arxiu esquelet sense definir quin box utilitzarem, és útil per veure totes les diferents opcions que conté un arxiu Vagrantfile, que no és més que un programa en Ruby.

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "base"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
```

Normalment, inciarem el projecte triant un box determinat, per fer això caldrà fer:

```bash
    vagrant init nom_del_box
    vagrant init generic/alpine39
```

La diferència, és que ara a la línia corresponent al box, ja es defineix el que s'utilitzarà

 ```ruby
     config.vm.box = "generic/alpine39"
```

Ara bé, sovint no necessitem totes les opcions a l'arxiu, si no que simplement volem configurar nosaltres mateixos allò que necessitem. Per aconseguir això, utilitzarem l'argument *"-m"*:

```bash
    vagrant init -m generic/alpine39
```

## status i global-status

 Serveix per conèixer l'estat de la màquina vagrant, de forma que ens dirà si es troba funcionant *running* o aturada *poweroff* o pausada *halted*.

 En el cas de global-status el que obtenim és l'estat de totes les màquines vagrant que tinguem al nostre equip.

## up

Amb aquesta comanda arrancarem un etorn Vagrant. Aquest procés pot implicar descarregar el box si no es troba o si no està actualitzat. Un cop fet això desplegarà l'entorn amb les opcions definides a l'arxiu Vagrantfile i finalment aprovisionarà la màquina.

Disposa de diversos arguments, però pel funcionament bàsic no els utilitzarem de manera que els comentarem més endavant.

# halt

Atura la màquina, de la mateixa manera que podem aturar una màquina virtual des del virtualitzador. Quan una màquina no s'atura, per exemple, per un procés que no li permet, tenim l'opció de forçar l'aturada, però hem de tenir clar que es poden perdre dades (equival a desconnectar l'alimentació d'un ordinador).

```bash
    vagrant halt
    vagrant halt --force
```

## suspend

A diferència de la comanda *halt* aquí es guarda l'estat de la màquina, de manera que quan es torni a iniciar estarà exactament al mateix estat on es va deixar. Equival a pausar la màquina al virtualitzador.

## resume

Servirà per recuperar una màquina que ha estat suspesa.

## destroy

Atura i elimina la màquina Vagrant. Això se sol fer periòdicament perquè ja veurem que no guardarem dades persistents a dins la màquina i d'aquesta manera alliberem espai i a més assegurem de treballar sobre una instància fresca.

## reload

Sovint modificarem l'arxiu Vagrantfile amb la instància Vagrant executant-se. Si volem aplicar els canvis utilitzarem aquesta comanda, que bàsicament fa un *halt* i un *up* però aplicar els canvis. En fer la recàrrega podem triar aplicar l'aprovisionament o no, això es fa per si no volem tornar a executar un script d'aprovisionament perquè ja s'ha fet anteriorment. 

```bash
    vagrant reload --provision
    vagrant reload --no-provision
```

### ssh

Si necessitem entrar dins la màquina Vagrant, el mètode per defecte serà mitjançant el protocol ssh. A més es genera el parell de claus pública/privada de manera que no caldrà validar-se amb usuari/contrasenya. L'usuari dins la instància s'anomena **vagrant**.

[<< Tornar a índex](../readme.md)

[>> Primeres pases](first_steps.md)