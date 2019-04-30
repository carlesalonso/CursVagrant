# Exercici 3

En primer lloc definim l'arxiu Vagrantfile:

```ruby
    Vagrant.configure("2") do |config|
        config.vm.define "vm1" do |vm1|
            vm1.vm.box = "ubuntu/bionicl64"
            vm1.vm.network "private_network", ip: "192.168.100.100"
            vm1.vm.synced_folder "/web", "/srv/web"
            vm1.vm.provision :shell do |shell|
                shell.path = "bootstrap.sh"
            end
        end
        config.vm.define "vm2" do |vm2|
            vm2.vm.box = "ubuntu/bionic64"
            vm2.vm.network "private_network", ip: "192.168.100.102"
            vm2.vm.synced_folder "/web", "/srv/web"
            vm2.vm.provision :shell do |shell|
                shell.path = "bootstrap.sh"
            end
        end
    end
```

L'arxiu *bootstrap.sh* seria similar al de l'exercici2

```bash
    #!/bin/bash
    sudo apt update
    sudo apt install nginx -y
    sudo ln -fs /srv/web /var/www
```

El arxius html corresponents al dos servidors podrien ser:

```html
    <html>
        <head>
            <title>vm1</title>
        <head>
        <body>
            <h1> Servidor VM1 </h1>
            <a href="http://192.168.100.102/web">Ves cap VM2</a>
        </body>
    </html>
```

```html
    <html>
        <head>
            <title>vm2</title>
        <head>
        <body>
            <h1> Servidor VM2 </h1>
            <a href="http://192.168.100.100/web">Ves cap VM1</a>
        </body>
    </html>
```
