## Install Vagrant

To download and install vagrant check this link [https://www.vagrantup.com/downloads](https://www.vagrantup.com/downloads)

## Look for vagrant box

To find a suitable vagrant box check this link [https://app.vagrantup.com/boxes/search](https://app.vagrantup.com/boxes/search)

## To init and start/stop vagrant file
$ vagrant init ubuntu/trusty64

$ vagrant up

## Suspend and resume vagrant box
$ vagrant suspend

$vagrant resume

## To restart vagrant box
$ vagrant reload


## To stop and destroy vagrant box
$ vagrant stop

$ vagrant destroy


## Change memory specs
```ruby
  config.vm.provider "virtualbox" do |vb
    
    # Customize the amount of memory on the VM:
    vb.memory = "1024"
  end
```
