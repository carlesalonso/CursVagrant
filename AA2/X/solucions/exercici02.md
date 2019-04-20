# Exercici 02

Si no s'havia configurat prèviament

> vagrant init -m /alpine/alpine64

Arrancar la màquina i connectar-se:

> vagrant up
>
> vagrant ssh

Instal·lar Apache:

>alpine:~$ sudo apk update
>
>alpine:~$ sudo apk add apache2
>
>alpine:~$ sudo rc-service apache2 start

Editar arxiu Vagrantfile:

```ruby
     Vagrant.configure("2") do |config|
        config.vm.box = "alpine/alpine64"
        config.vm.network "forwarded_port", guest: 80, host: 8080
    end
```

> vagrant reload

Podem comprovar que funciona posant la següent URL al navegador: localhost:8080