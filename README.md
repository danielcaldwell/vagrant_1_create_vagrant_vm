# Creating a simple Virtual Machine using Vagrant

## Install VirtualBox

I use virtual box as the Virtual Machine Software. You can [download and install virtual box from oracle](https://www.virtualbox.org/).


## Installing Vagrant

You can [download and install vagrant from vagrantup.com](https://www.vagrantup.com/downloads.html)

## Choosing a vm box

Vagrant has a good list of boxes. I usually stick to [Ubuntu Boxes](https://app.vagrantup.com/boxes/search?provider=virtualbox&q=Ubuntu&sort=downloads). 

## Create a vagrant file

This simple command will create a *default* Vagrantfile for you to customize:

```
vagrant init
```

The Vagrantfile will be in the directory where you ran the command. I have removed most of the comments and in the end have a minimal vagrant file. 

## Create the Vagrant VM

```
vagrant up
```

This will create download the Vagrant Box to your local machine, create a VirtualBox Virtual machine with that box, and start the virtual machine. Note, you do not specify a file name. 


## Check the status of the Vagrant VM

```
vagrant status
```
This will output the status of the Vagrant machine specified in the Vagrantfile

```
$ vagrant status
Current machine states:

default                   running (virtualbox)

The VM is running. To stop this VM, you can run `vagrant halt` to
shut it down forcefully, or you can run `vagrant suspend` to simply
suspend the virtual machine. In either case, to restart it again,
simply run `vagrant up`.
```

## Check the status of all Vagrant VM's on the computer

```
vagrant global-status
```
This will query all the Vagrant Virtual Machines running on the computer.

```
$ vagrant global-status
id       name      provider   state    directory                                                        
--------------------------------------------------------------------------------------------------------               
bf81b26  vm1    virtualbox poweroff /Users/daniel/github/vagrant_1_create_vagrant_vm/2_multiple_machines
da9d716  vm2    virtualbox poweroff /Users/daniel/github/vagrant_1_create_vagrant_vm/2_multiple_machines 
280951a  def    virtualbox running  /Users/daniel/github/vagrant_1_create_vagrant_vm/1_single_machine      
 
The above shows information about all known Vagrant environments
on this machine. This data is cached and may not be completely
up-to-date. To interact with any of the machines, you can go to
that directory and run Vagrant, or you can use the ID directly
with Vagrant commands from any directory. For example:
"vagrant destroy 1a2b3c4d"
```

## Stop the Vagrant VM

```
vagrant halt
```

This will poweroff the Virtual Box VM's but it will keep them around. 


## Listing the Vagrant Boxes that are available on the machine

```
vagrant box list
```

This will list all the downloaded boxes that are on the machine. I have 3 versions of xenial (16.04), and one of bionic (18.04)

```
$ vagrant box list
ubuntu/bionic64 (virtualbox, 20191205.0.0)
ubuntu/xenial64 (virtualbox, 20180419.0.0)
ubuntu/xenial64 (virtualbox, 20180427.0.0)
ubuntu/xenial64 (virtualbox, 20191204.0.0)
```

Checking the ~/.vagrant/boxes directory will show me them as well: 

```
$ ls -1 ~/.vagrant.d/boxes/
ubuntu-VAGRANTSLASH-bionic64
ubuntu-VAGRANTSLASH-xenial64

$ ls -1 ~/.vagrant.d/boxes/ubuntu-VAGRANTSLASH-bionic64/
20191205.0.0
metadata_url

$ ls -1 ~/.vagrant.d/boxes/ubuntu-VAGRANTSLASH-xenial64/
20180419.0.0
20180427.0.0
20191204.0.0
metadata_url
```

## Checking for updated boxes from Vagrant

```
vagrant box outdated
```
Here we can see that my bionic box was updated and I can get the latest one. Node that these may be large to download so it may take a while. 

```
$ vagrant box outdated
Checking if box 'ubuntu/bionic64' is up to date...
A newer version of the box 'ubuntu/bionic64' for provider 'virtualbox' is
available! You currently have version '20191205.0.0'. The latest is version
'20191209.0.0'. Run `vagrant box update` to update.
```

## Updating the box from Vagrant

``` 
vagrant box update
```
## Updating the Vagrant VM

```
vagrant halt
vagrant reload
```



## Teardown the Vagrant VM

```
vagrant destroy
```

This will delete the vagrant vm's and the Virtual Box Virtual Machines. 

For example: 

```
$ vagrant destroy
    vagrantmachine: Are you sure you want to destroy the 'vagrantmachine' VM? [y/N] y
==> vagrantmachine: Forcing shutdown of VM...
==> vagrantmachine: Destroying VM and associated drives...
```

## Teardown a Vagrant VM from outside the Vagrantfile Directoryu

```
vagrant global-status
vagrant destroy ####
```

For example: 

```
$ vagrant destroy af2b909
    vagrantmachine: Are you sure you want to destroy the 'vagrantmachine' VM? [y/N] y
==> vagrantmachine: Forcing shutdown of VM...
==> vagrantmachine: Destroying VM and associated drives...
```

# Troubleshooting

## Checking VirtualBox for a Vagrant VM

```
vboxmanage list vms
```
This will list all the VirtualBox's Virtual Machines. Easy to access from the command line:

```
$ vboxmanage list vms
"pgconfsv2015_default_1447673194608_60521" {1d729039-4254-4f5b-a72b-dbcbb88fe001}
"vagrantmachine" {2378e4b9-7630-4369-9d87-7902a48a7bbb}
```


## Removing bad entries from global-status

Soemtimes things go wrong and vagrant thinks there is a vm when there isn't.

For example here I list all of my Vagrant Machines: 

```
$ vagrant global-status
id       name           provider   state    directory                                                                    
-------------------------------------------------------------------------------------------------------------------------
690e028  default        virtualbox poweroff /Users/daniel/github/pgconfsv2015             
370eaad  vagrantmachine virtualbox running  /Users/daniel/github/vagrant_1_create_vagrant_vm                  
af2b909  vagrantmachine virtualbox running  /Users/daniel/github/vagrant_1_create_vagrant_vm/2_single_machine 
```

I had moved the Vagrantfile into it's own directory so I could show the difference between the default and the one I create. When I tried to remove it, I get this error: 

```
$ vagrant destroy 370eaad
The machine with the name 'vagrantmachine' was not found configured for
this Vagrant environment.
```

So, I pruned the global-status list using the `--prune` option: 

```
vagrant global-status --prune
```

Now the vm is no longer there
```
$ vagrant global-status 
id       name           provider   state    directory                                                                    
-------------------------------------------------------------------------------------------------------------------------
690e028  default        virtualbox poweroff /Users/dcaldwel/github/pgconfsv2015                          
af2b909  vagrantmachine virtualbox running  /Users/dcaldwel/github/vagrant_1_create_vagrant_vm/2_single_machine 
 
The above shows information about all known Vagrant environments
on this machine. This data is cached and may not be completely
up-to-date. To interact with any of the machines, you can go to
that directory and run Vagrant, or you can use the ID directly
with Vagrant commands from any directory. For example:
"vagrant destroy 1a2b3c4d"
```





