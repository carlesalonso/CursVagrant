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
També podem buscar ajuda específica per una comanda, per exemple:
````
	vagrant box add -h
````
Ara veurem amb un exemple les comandes bàsiques per iniciar un projecte amb Vagrant.

## El nostre primer projecte

Per crear el nostre primer projecte, seguirem les següent passes, recordeu que heu creat una carpeta Vagrant que serà on crearem tota les carpetes dels projectes:
````
    mkdir primerprojecte
    cd primerprojecte
    vagrant init -m generic/ubuntu1804
````

Amb aquesta comanda __vagrant init -m generic/ubuntu1804__ es crea l'arxiu Vagrantfile bàsic per desplegar una instància a partir del box que vam baixar anteriorment. El fet de posar l'argument -m, fa que l'arxiu creat sigui bàsic, a diferència del que es crea sense l'argument, que inclou tota una sèrie de línies comentades amb diferents opcions i exemples per aplicar.

Per arrancar aquesta instància farem:
```
    vagrant up
```
Aquesta comanda s'encarrega d'aixecar la instància, baixant si és necessari el box. A continuació crearà la màquina virtual i l'aplicarà la configuració que defineixi l'arxiu, així com els processos post instal·lació que es defineixin a la secció d'aproviosionament.

Podem veure les instàncies virtuals que tenim aixecades amb la comanda:
````
    vagrant status
````
Pel que fa a la xarxa, per defecte al treballar amb VirtualBox com proveïdor, es configura en NAT automàticament, d'aquesta manera, la instància virtual tindrà connectivitat a l'exterior, però ja veurem com ho podem modificar per tal de tenir les opcions de xarxa privada (només tindria visibiliat amb l'equip hoste) o amb configuració pública (equivalent a la xarxa en pont de VirtualBox). 

Ara bé, un equip virtual en NAT no tindria connectivitat amb la màquina hoste, per solucionar això s'utilitza la redirecció de ports que permet mapejar ports de la instància virtual a ports de la màquina física. Per defecte, es mapeja el port 22 de la instància virtual al port 2222 per permetre la connexió ssh. Si necessitem mapejar altres ports, és tant senzill com editar l'arxiu Vagrantfile i afegir una línia com aquesta:
```
    config.vm.network "forwarded_port", guest: 80, host: 8080
```
En aquest cas, es redirigeix el port 80 de la instància virtual cap el port 8080 de la màquina física. D'aquesta manera, si estem treballant amb codi web, podem verificar el resultat directament al navegador posant com URI *http://localhost:8080/ruta*.

Podem connectar-nos a la màquina per treballar-hi dins, aquí l'opció per defecte és el protocol ssh (tot i que també es poden configurar connexions via RDP). Per connectar-s'hi no cal entrar ni usuari ni password, l'usuari amb el que es logueja és __vagrant__, tot i que podem elevar privilegis un cop dins. La comanda per connectar-nos és:
````
	vagrant ssh
````
Per evitar conflictes amb l'equip hoste o amb altres instàncies virtuals funcionant, el protocol SSH que escolta pel port 22 a la màquina virtual es mapeja a un altre port a la màquina física, per defecte al port 2222, però ja veurem que es pot modificar i com té un sistema per evitar conflictes entre sistemes.

Un cop dins de la màquina si ens anem a la carpeta __/vagrant__, veurem que és tracta del mapeig de la carpeta del projecte, en aquest cas __primerprojecte__. Aquesta és una característica molt interessant, ja que ens permet treballar les dades amb els editors del nostre equip (VisualCode, Sublime, Eclipse, etc.) a més d'assegurar la persitència de les dades encara que eliminem la instància virtual.

Ja veurem més endavant més informació de com mapejar entre màquina física i virtual amb protocols com NFS, SMB o sincronització amb rsync, però de moment, utilitzarem el model de sincronització més senzill, que és utilitzar la funció de VirtualBox per mapejar carpetes.

Per mapejar una carpeta a més de la per defecte, simplement cal obrir l'arxiu Vagrantfile i afegir la següent línia indicant la carpeta del vostre ordinador que voleu compartir (compte amb el doble \\) i la carpeta destinació dins la VM:
````
	config.vm.synced_folder "C:\\Users\\usuari\Documents\\html", "/var/html"
````
En el cas que hi treballeu des d'un ordinador *nix:
````
	config.vm.synced_folder "/home/usuari/html", "/var/html"
````
I ara us podeu preguntar, què he de fer per tornar a carregar la configuració, per això tenim la comanda:
````
	vagrant reload
````
que bàsicament fa un **vagrant halt** i **vagrant up** per carregar els canvis al Vagrantfile.

Un cop haguem acabat de treballar tenim diverses opcions per finalitzar:
````
	vagrant halt
````
Equival a aturar la màquina virtual, és a dir, es para la instància però es manté creada.

Si simplement necessitem pausar momentàniament la màquina mantenint l'estat (vigileu perquè això implica emmagatzemar l'estat al disc) per després recuperar-lo, haurem d'usar les comandes:
````
	vagrant suspend (per pausar)
	vagrant resume (per restaurar)
````
Finalment, l'opció més dràstica és eliminar la instància, és a dir, esborrar la màquina virtual, de manera que quan tornem a fer el **vagrant up** es tornarà a crear des de zero a partir del box. 
````
	vagrant destroy
````
Sovint és l'opció més utilitzada perquè a part de l'estalvi evident de disc, permet treballar sempre amb una versió fresca del sistema.