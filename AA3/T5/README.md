# Sincronització carpetes

La sincronització de carpetes (*synced folders*) és el mecanisme amb el qual compartim arxius entre l'ordinador i la màquina  Vagrant. D'aquesta manera podem editar els arxius al nostre ordinador, tenir-los emmagatzemats, versionar-los, etc. però provar el funcionament directament a la instància virtual sense haver de copiar-los.

Ja hem vist en el moment de crear els nostres primers entorns, que la forma de compartir una carpeta és introduint la següent configuració a l'arxiu Vagrantfile:

```ruby
    Vagrant.configure(2) do |config|
        config.vm.box = "ubuntu/bionic64"
        config.vm.synced_folder "/web", "/var/www"
    end
```

Recordem que normalment, els boxes ja porten configurada la compartició de la carpeta arrel del projecte que es mapeja dins la instància virtual amb la ruta **/vagrant**. Cas que un box concret no ho faci i calgui mapejar aquesta carpeta, simplement cal afegir la següent línia a la configuració:

```ruby
    config.vm.synced_folder ".", "vagrant"
```

Aquí cal fer un parell de comentaris. El primer és que cal que recordeu les rutes relatives i les absolutes. El "." representa l'inici de la ruta relativa (la carpeta del projecte). Per accedir a carpetes que estiguin dins la carpeta del projecte usarem rutes relatives. En general, a un Vagrantfile **mai usarem rutes absolutes per les carpetes del nostre ordinador** perquè farà que només funcioni al nostre ordinador i per tant, és una mala política a l'hora de compartir entorns de treball. Pel que respecta a la carpeta creada en la instància virtual, aquí sí que és convenient treballar amb rutes absolutes per comoditat.

Una altra preacució a tenir és mai mapejar contra la carpeta **/home/vagrant** que és la carpeta d'usuari que es crea amb la màquina. Si ho fem, ens trobarem que no podrem connectar-vos via ssh. El motiu és que aquesta carpeta conté el perfil d'usuari de connexió i al mapejar contra ella, perdríem aquestes dades.

La sincronització de carpetes es pot fer utilitzant diferents opcions:

* Sincronització bàsica
* NFS
* SMB
* RSync

## Sincronització bàsica

És una  sincronització bidireccional i que gestiona el provider (virtualitzador). El seu principal avantatge és la senzillesa de configuració, però té dos seriosos inconvenients: per una banda, no tots els providers suporten aquest tipus de compartició (a nivell de hipervisor local hyperV no en té) i a més el seu rendiment és molt més lent que amb la resta d'alternatives.

## NFS

El protocol NFS (network file system) és un dels mètodes clàssics per la compartició de carpetes i fitxers en entorns derivats de Unix, tot i que també es pot utilitzar en sistemes Windows. L'avantatge d'usar NFS és un increment important en velocitat respecte la sincronització bàsica i a més el fet que es pot utilitzar entre sistemes, especialment útil si treballem desplegant contra un servidor fora de la nostra màquina de desenvolupament.

Des del punt de vista de la configuració, l'únic canvia a tenir en compte és afegir el tipus nfs:

```ruby
config.vm.synced_folder ".", "/home/vagrant/files", type: "nfs"
```

Òbviament, cal que tant la màquina virtual com l'ordinador suportin NFS. En el cas de la màquina desenvolupament cal que sigui el servidor. En el cas d'ordinadors Windows per defecte, no funciona aquest tipus de sincronització i ignorarà la directiva.

Un detall important és que per usar NFS si usem VirtualBox cal definir una xarxa privada o pública, ja que per NAT no es veuen les dues màquines.

Es poden especificar més detalls de la compartició com podeu veure a la documentació oficial [link](https://www.vagrantup.com/docs/synced-folders/nfs.html)

## SMB

El protocol SMB (server message block) és el protocol de compartició que utilitzen els sistemes Windows tot i que també s'ha popularitzat en altres sistemes amb el servei Samba. Si utilitzem hyperV serà la forma de sincronitzar carpetes entre ordinador i instància Vagrant. També resulta més eficient que la compartició bàsica.

La forma bàsica de compartir una carpeta mitjançant SMB és la següent:

```ruby
  config.vm.box = "ubuntu/bionic64"
  config.vm.synced_folder ".", "/vagrant", type: "smb"
end
```

**Per utilitzar SMB en Windows cal tenir privilegis d'administració**. Una llista completa de les diferents opcions les podeu trobar [la documentació](https://www.vagrantup.com/docs/synced-folders/smb.html).

## RSync

El protocol RSync és una mica diferent i s'utilitza quan no és pot compartir una carpeta entre les dues màquines amb els mètodes anteriors. La principal característica és que es tracta d'una sincronització en un sol sentit, de l'ordinador cap la màquina virtual i que no és permanent. Això vol dir que sincronitza o bé al principi o quan hi hagi un canvi (si es configura així), però no tenim un mapeig permanent.

To start using RSync, our Vagrant file just needs an extra parameter on the config.vm.synced_folders option:

```ruby
config.vm.synced_folder ".", "/home/vagrant/files", type: "rsync"
```

Una característica és que permet excloure arxius o carpetes de la sincronització mitjançant la directiva *exclude*

```ruby
    Vagrant.configure("2") do |config|
        config.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: ".git/"
    end
```

De la mateixa manera que els dos mètodes anteriors, a la guia oficial de Vagrant trobareu una explicació més detallada dels diferents arguments [link](https://www.vagrantup.com/docs/synced-folders/rsync.html).

[<< Tornar a índex AA3](../README.md)

[>> AA3. Aprovisionadors](../T6)