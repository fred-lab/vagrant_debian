# Debian 9 with Vagrant 
A Vagrant machine to try Ansible

## Require
- Vagrant ```sudo pacman -S vagrant```
- Virtual Box 

## Create an SSH key
Create the key : ```ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/vagrant_key -C "vagrant@ip_vm"```  
Add the key to the ssh-agent: ```ssh-add ~/.ssh/vagrant_key```  
Check if the key is available for the ssh-agent : ```ssh-add -L```  
Start the VM : ```vagrant up```  
With the current configuration, the VM should be accessible with : ```ssh vagrant@192.168.50.50```

### Tips
Edit /etc/hosts : ```sudo gedit /etc/hosts```  
Add  ```192.168.50.50  vagrant``` to the file  
Now, you can connect with ```ssh vagrant@vagrant```

## Usefull command
See [this article](https://blog.sleeplessbeastie.eu/2016/08/01/how-to-get-started-with-vagrant/)

```vagrant init debian/stretch64``` : initialize a new config for Vagrant with a Debian 9

```vagrant up``` : Start the VM. 

```vagrant status``` : Give the status of the VM

```vagrant halt``` : Stop the VM

```vagrant reload``` : Restart the VM

```vagrant suspend``` : Suspend the VM

```vagrant resume``` : Resume the VM

```vagrant destroy``` : Delete the VM

```vagrant ssh-config``` : Generate configuration for SSH client

```vagrant ssh``` : connect with SSH to the VM

```vagrant provision``` : forces reprovisioning of the vagrant machine

## Settings

Define an @IP for the Vagrant machine  
**config.vm.network "private_network", ip: "192.168.33.10"** 

Tell Vagrant to no regenerate an insecure key, it's useless since we will use our own public key instead  
**config.ssh.insert_key = false**

Copy the public key into the Vagrant machine and rename it into "authorized_keys"  
**config.vm.provision "file", source: "~/.ssh/vagrant_key.pub", destination: "~/.ssh/authorized_keys"**

Install Python for Ansible : 
  ```
    config.vm.provision "shell", inline: <<-SHELL
         apt-get update
         echo "Install Python for Ansible"
         apt-get install -y python
       SHELL
    end
  ```

Config for RAM & CPU :
```aidl
  config.vm.provider "virtualbox" do |vb|
     vb.memory = "1024"
     vb.cpus = 1
  end
```
