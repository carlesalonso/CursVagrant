# Exercici 2

En primer lloc l'arxiu Vagrantfile

```ruby
    Vagrant.configure("2") do |config|
        config.vm.box = "ubuntu/ubuntu1804"
        config.vm.box_check_update = false
        config.vm.provision "shell", path: "bootstrap.sh"
    end
```

I l'arxiu corresponent a l'script:

```bash
    #!/bin/bash
    sudo apt update
    sudo apt install nginx -y
    sudo ln -fs /vagrant/web /var/www
```
