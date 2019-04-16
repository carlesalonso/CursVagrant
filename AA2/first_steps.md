# Primers pasos amb Vagrant

La documentació oficial la teniu disponible a:

[docs.vagrantup.com](http://docs.vagrantup.com)

## Help

Vagrant funciona sobre terminal i té tota una sèrie de comandes i arguments. Per veure les opcions disponibles, és molt útil consultar l'ajuda:

```
vagrant -h

Usage: vagrant [options] <command> [<args>]

    -v, --version                    Print the version and exit.
	-h, --help                       Print this help.
		
Common commands:
	box             manages boxes: installation, removal, etc.
	connect         connect to a remotely shared Vagrant environment
	destroy         stops and deletes all traces of the vagrant machine
	global-status   outputs status Vagrant environments for this user
	halt            stops the vagrant machine
	help            shows the help for a subcommand
	init            initializes a new Vagrant environment by creating a Vagrantfile
	login           log in to HashiCorp's Atlas
	package         packages a running vagrant environment into a box
	plugin          manages plugins: install, uninstall, update, etc.
	port            displays information about guest port mappings
	powershell      connects to machine via powershell remoting
	provision       provisions the vagrant machine
	push            deploys code in this environment to a configured destination
	rdp             connects to machine via RDP
	reload          restarts vagrant machine, loads new Vagrantfile configuration
	resume          resume a suspended vagrant machine
	share           share your Vagrant environment with anyone in the world
	snapshot        manages snapshots: saving, restoring, etc.
	ssh             connects to machine via SSH
	ssh-config      outputs OpenSSH valid configuration to connect to the machine
	status          outputs status of the vagrant machine
	suspend         suspends the machine
	up              starts and provisions the vagrant environment
	version         prints current and latest Vagrant version
																																	 
For help on any individual command run `vagrant COMMAND -h`

Additional subcommands are available, but are either more advanced
or not commonly used. To see all subcommands, run the command
`vagrant list-commands`.
```
Per les diferents comandes podem obtenir l'ajuda disponible afegint el "-h" al final, així per exemple, si volem saber amb més detall les opcions de gestió amb els boxes, faríem:

```
    vagrant box -h
```

## Activitat


1. Ús bàsic de vagrant: vagrant init, vagrant up, vagrant status, vagrant halt

	Crear un directori pel projecte i crear un fitxer de configuració bàsic amb:
    
   ```
   vagrant init -m generic/ubuntu1804
   ```
   La diferència entre posar el "-m" o no, és que d'aquesta manera crea un arxiu mínim enlloc de l'arxiu amb totes les opcions disponible però amb el # perquè no s'apliquin.

    A continuació arranquem la màquina:
   ```
   vagrant up
   ```
   A continuació accedim a la màquina via ssh (quan volguem sortir, recordeu que es fa amb 'exit' ):

   ```
   vagrant ssh
   ```
   Per comprovar l'estat de la màquina:

   ```
   vagrant status
   ```
   Per parar la màquina:

   ```
   vagrant halt
   ```
   Si es torna a aixecar la màquina, s'agafarà aquesta que ja està creada (i ocupant espai). Quant ja no es necessiti aquesta màquin s'elimina amb la comanda:

   ```
   vagrant destroy
   ```
