# Primers pasos amb Vagrant

Veurem amb un exemple com desplegar el nostre primer projecte amb Vagrant.

## El nostre primer projecte

Per crear el nostre primer projecte, seguirem les següent passes, recordeu que heu creat una carpeta Vagrant que serà on crearem tota les carpetes dels projectes:

```bash
    mkdir primerprojecte
    cd primerprojecte
    vagrant init -m generic/ubuntu1804
```

Per arrancar aquesta instància farem:

```bash
    vagrant up
```

Pel que fa a la xarxa, per defecte al treballar amb VirtualBox com proveïdor, es configura en NAT automàticament, d'aquesta manera, la instància virtual tindrà connectivitat a l'exterior, però ja veurem com ho podem modificar per tal de tenir les opcions de xarxa privada (només tindria visibiliat amb l'equip hoste) o amb configuració pública (equivalent a la xarxa en pont de VirtualBox). 

Ara bé, un equip virtual en NAT no tindria connectivitat amb la màquina hoste, per solucionar això s'utilitza la redirecció de ports que permet mapejar ports de la instància virtual a ports de la màquina física. Per defecte, es mapeja el port 22 de la instància virtual al port 2222 per permetre la connexió ssh. Si necessitem mapejar altres ports, és tant senzill com editar l'arxiu Vagrantfile i afegir una línia com aquesta:

```bash
    config.vm.network "forwarded_port", guest: 80, host: 8080
```

En aquest cas, es redirigeix el port 80 de la instància virtual cap el port 8080 de la màquina física. D'aquesta manera, si estem treballant amb codi web, podem verificar el resultat directament al navegador posant com URI *http://localhost:8080/ruta*.

Podem connectar-nos a la màquina per treballar-hi dins, aquí l'opció per defecte és el protocol ssh (tot i que també es poden configurar connexions via RDP). Per connectar-s'hi no cal entrar ni usuari ni password, l'usuari amb el que es logueja és __vagrant__, tot i que podem elevar privilegis un cop dins. La comanda per connectar-nos és:

```bash
    vagrant ssh
```

Per evitar conflictes amb l'equip hoste o amb altres instàncies virtuals funcionant, el protocol SSH que escolta pel port 22 a la màquina virtual es mapeja a un altre port a la màquina física, per defecte al port 2222, però ja veurem que es pot modificar i com té un sistema per evitar conflictes entre sistemes.

Un cop dins de la màquina si ens anem a la carpeta __/vagrant__, veurem que és tracta del mapeig de la carpeta del projecte, en aquest cas __primerprojecte__. Aquesta és una característica molt interessant, ja que ens permet treballar les dades amb els editors del nostre equip (VisualCode, Sublime, Eclipse, etc.) a més d'assegurar la persitència de les dades encara que eliminem la instància virtual.

Ja veurem més endavant més informació de com mapejar entre màquina física i virtual amb protocols com NFS, SMB o sincronització amb rsync, però de moment, utilitzarem el model de sincronització més senzill, que és utilitzar la funció de VirtualBox per mapejar carpetes.

Per mapejar una carpeta a més de la per defecte, simplement cal obrir l'arxiu Vagrantfile i afegir la següent línia indicant la carpeta del vostre ordinador que voleu compartir (compte amb el doble \\) i la carpeta destinació dins la VM:

```bash
    config.vm.synced_folder "C:\\Users\\usuari\Documents\\html", "/var/html"
```

En el cas que hi treballeu des d'un ordinador *nix:

```bash
    config.vm.synced_folder "/home/usuari/html", "/var/html"
```

Recarregarem la configuració amb:

```bash
    vagrant reload
```

Un cop haguem acabat de treballar tenim diverses opcions per finalitzar com s'ha vist al punt anterior.

[Tornar a índex](../readme.md)                                                       [Definció d'entorns: Vagrantfile](vagrantfile.md)