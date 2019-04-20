# Exercici 3

```ruby
Vagrant.configure("2") do |config|

  config.vm.box = "generic/ubuntu1804"
  config.vm.box_check_update = false
  config.vm.network "forwarded_port", guest: 443, host: 4300
  config.vm.synced_folder "C:\\Users\\ElteuUsuari\\Documents\\exercici03", "/home/vagrant/exercici03"

end
```
