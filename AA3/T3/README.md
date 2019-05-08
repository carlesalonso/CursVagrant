# Entorns multimàquina

## Creació de l'entorn

Podem utilitzar Vagrant per definir entorns amb més d'una màquina. Això ens permet, des d'un sol arxiu Vagrantfile desplegar un entorn de desenvolupament que a l'igual que el de producció tingui per exemple, un servidor amb la funció de servidor d'aplicacions i un segon com  a servidor de base de dades.

L'arxiu Vagrantfile té un bloc *config* que engloba totes les configuracions. Dins seu tindrem un bloc per cadascuna de les màquines virtuals que volguem definir. És important, definir un nom per cadascuna de les màquines, per poder identificar-les posteriorment en accions com l'aprovisionament o la connexió via ssh. Per exemple:

```ruby
    Vagrant.configure("2") do |config|
        # Configure first machine
        config.vm.define "vm1" do |vm1|
        end
        # Configure second webmachine
        config.vm.define "vm2" do |vm2|
        end
    # Configure third  machine
        config.vm.define "vm3" do |vm3|”
        end
    end
 ```

## Connexió SSH

Si necessitem connectar-nos a les màquines d'aquest entorn múltimple, caldrà especificar a quina de les màquines ho farem, de fet, si simplmement escrivim *vagrant ssh* ens preguntarà a quina de les màquines específiques s'ha de fer la connexió.

Per tant, la forma habitual de connectar-nos serà:

```bash
    vagrant ssh vm1
```

## Comunicació entre màquines

Normalment, definirem una xarxa privada per tal que les màquines es comuniquin utilitzant adreces estàtiques, tal com es farà posteriorment a l'entorn de producció. Tot i que també, ho podem fer amb adreces públiques (bridged).

Un exemple seria quelcom com això:

```ruby
    Vagrant.configure("2") do |config|
        config.vm.define "vm1" do |vm1|
            vm1.vm.box = "ubuntu/bionicl64"
            vm1.vm.network "private_network", ip: "10.0.0.11"
        end
        config.vm.define "web2" do |vm2|
            vm2.vm.box = "ubuntu/bionic64"
            vm2.vm.network "private_network", ip: "10.0.0.12"
        end
    end
```

**Fixeu-vos com ara dins el bloc de cadascuna de les instàncies es comença amb el nom de la màquina virtual**.

## Control de les diferents màquines

Si fem *vagrant up* s'aixecaran tots els entorns simultàniament, tot i que és posible invocar únicament a una sola de les màquines fent *vagrant up nom*

Si editem l'arxiu Vagrantfile per modificar les característiques d'una sola màquina, no caldrà recarregar totes les instàncies de l'entorn, sino únicament, l'afectada fent *vagrant reload nom*.

Altres comandes com *vagrant status* també es poden aplicar únicament a una de les màquines

Per finalitzar *vagrant halt* aturarà tot l'entorn, de la mateixa manera que *vagrant destroy* eliminarà totes les màquines creades per l'arxiu Vagrantfile. Tot i que també podrem aplicar les comandes a una sola d'elles.

Com a curiositat, no només podem especificar el nom de la instància virtual a la que volguem aplicar una comanda, també podem utilitzar la comanda referint-nos a vàries màquines, per exemple:

```bash
    vagrant reload web1 web2 web3
```

I fins i tot, utilitzant expressions regulars, en aquest cas, la comanda anterior quedaria com:

```bash
    vagrant reload /web\d/
```

## Aprovisionament

Pel que respecte l'aprovisionament de les diferents màquines utilitzarem scripts que permetran desplegar els entorns específics, tal com es pot veure al següent exemple:

```ruby
    Vagrant.configure("2") do |config|
        # Configurar primerVM
        config.vm.define "vm1" do |vm1|
            vm1.vm.box = "ubuntu/bionic64"
            vm1.vm.network "private_network", ip: "10.0.0.10"
            vm1.vm.provision :shell do |shell|
                shell.path = "vm1Provision.sh"
            end
        end
    # Configure segonaVM
        config.vm.define "vm2" do |vm2|
            vm2.vm.box = "ubuntu/bionic64"
            vm2.vm.network "private_network", ip: "10.0.0.11"
            vm2.vm.provision :shell do |shell|
                shell.path = "vm2Provision.sh"
            end
        end
    end
```

[<< Tornar a índex AA3](../README.md)

[>> AA3. Gestió de boxes](../T4)
