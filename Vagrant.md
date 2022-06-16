# Vagrant
- You can find list of images in vagrant cloud.
- Virtual box is just a provider for the Vagrant. You can use the Vagrant with other providers like VMware, Hyper-V, Docker or...

## Basic Commands

```bash
# Shows the running boxes
vagrant status 
# Shutdown a box
vagrant halt
```

### init

The `init` initialize the vagrant box and create a new `vagrantfile` in the current directory.

```bash
vagrant init centos/7
```

### up

To start the vagrant box

```bash
vagrant up
```

### ssh

To jump into the vagrant box you can use `ssh`  command. It takes care of the ssh keys and all of thoses stuff.

```bash
vagrant ssh
```

### Snapshot

This helps us to rollback at a later time.

```bash
vagrant snapshot save <option> <vm-name> <name>
```

### Boxes

```bash
# To list all of installed boxes
vagrant box list
```

## Vagrant File

```python
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
	# The image name or the box name
	config.vm.box = "centos/6"
	# You can config networking
	config.vm.network :private_network, ip: "192.168.58.111"
	# You can create shared directory and map it to the box
	config.vm.synced_folder "../data", '/vagrant_data'
	# You can customize the hardware allocated to the box
	config.vm.provider :virtualbox do |vb|
		vb.memory = "1024"
	end
	# You can add some boot up script
	config.vm.provision "shell", inline: <<-SHELL
		apt-get update
		apt-get install -y apache2
	SHELL
end
```

Each time you make a change to the `Vagrantfile`, you need to apply it.

```bash
vagrant reload
```

# Resources

- [DevOps Prerequisites Course - Getting started with DevOps](https://www.youtube.com/watch?v=Wvf0mBNGjXY)