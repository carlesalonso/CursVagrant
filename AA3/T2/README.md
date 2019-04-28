# Aprovisionament per scripts

Quan parlem d'aprovisionar el que estem plantejant és configurar la màquina en el moment d'aixecar-la amb el *vagrant up*. L'objectiu d'aprovisionar és poder personalitzar la màquina Vagrant a partir d'un box més genèric, el que ens estalvia haver de tenir molts boxes instal·lats.

Quan s'aprovisiona algunes de les accions que farem seran: instal·lar software, modificar configuracions o fer canvis del sistema operatiu.

Més endavant, veurem que hi ha eines específiques d'aprovisionament que no són exclusives per Vagrant, si no que s'utilitzen per la configuració de sistemes. Algunes d'aquestes eines són: Ansible, Chef, Puppet o Salt.

Tot i això, una primera forma d'aprovisionar els nostres entorns serà usar el llenguatge d'script pel sistema operatiu específic, en els nostres exemples com treballem amb entorns Linux, serà el shell.

Per definir el bloc d'aprovisionament s'utilitza la següent definició:

```ruby
    config.vm.provision "shell"
```

A partir d'aquí es poden definir configuracions *inline*:

```ruby
    config.vm.provision "shell", inline "sudo apt update -y"
```

O utilitzar una configuració de bloc:

```ruby
    config.vm.provision "shell", inline: <<-SHELL
        sudo yum install python  -y -q
        sudo hostnamectl set-hostname python
        sudo timedatectl set-timezone Europe/Madrid
        SHELL
```

Aquest model de bloc és més llegible, però per configuracions més complexes, seria molt més còmode utilitzar scripts. Per fer això, el format és com el del següente exemple on usem un script inline dins el Vagrantfile.

```ruby
    $installscript = <<-SCRIPT
    sudo apt update -y
    sudo apt install -y nginx
    SCRIPT

    config.vm.provision "shell", inline: $installscript
```

Però un dels principis del desenvolupament és el DRY (don't repeat yourshelf) i amb els scripts inline, em veig obligat a repetir les mateixes línies en diversos Vagrantfile. Seria molt més pràctic escriure els scripts en arxius i llençar-los durant l'aprovisionament:

```ruby
    vm.provision :shell do |shell|
      shell.path = "install.sh"
    end
```

On en aquest cas l'script es troba a la carpeta de configuració del projecte. Si estigués a un servidor extern, doncs hauríem d'especificar la seva ruta, per exemple *example.com/scripts/install.sh*.

Si cal passar arguments, es poden passar de la següent manera:

```ruby
    vm.provision :shell do |shell|
      shell.inline = "echo $1"
      shell.args =["hola món"]
    end
````

[Aquí](https://github.com/carlesalonso/vagrant-demo-web) podeu veure un repositori que conté un projecte Vagrant amb aprovisionament per script.

[<< Tornar a índex AA3](../README.md)

[>> AA3. Entorns multimàquina](../T3)