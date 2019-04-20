# Solució

Iniciar i aixecar la màquina:

> vagrant init -m alpine/alpine64
>
> vagrant up

Vagranfile:

```ruby
    Vagrant.configure("2") do |config|
        config.vm.box = "alpine/alpine64"
    end
```

Usuari connexió ssh:

> alpine:~$ id
>
> uid=1000(vagrant) gid=1000(vagrant) groups=10(wheel),1000(vagrant)

Comprovació carpeta compartida per defecte (default synced folder):

> alpine:~$ ls /vagrant/
>
> Vagrantfile

Tancar connexió ssh:

> alpine:~$ exit

Comprovar estat de la màquina:

> vagrant status
>
> default                   running (virtualbox)

Aturar la màquina i comprovar que no deixar aturar la màquina (observar l'error):

> vagrant halt
>
> vagrant status
>
> bash: line 4: shutdown: command not found

Forçar l'aturada de la màquina:

> vagrant halt -f

