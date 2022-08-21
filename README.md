# Project Provisioning 3 nodes

Step 1: Install the vagrant

Step 2: Install the virtualbox

Step 3: Configure the host network manager for the ip, this located in virtualbox > host network manager

Step 4: Initialize the vagrant script to create the 3 nodes and 1 workstation, copy the given scipts found below or copy in my repository

```
Vagrant.configure("2") do |config|
    servers=[
        {
          :hostname => "control",
          :box => "bento/ubuntu-22.04",
          :ip => "192.168.56.10",
          :ssh_port => '2200'
        },
        {
          :hostname => "node1",
          :box => "bento/ubuntu-22.04",
          :ip => "192.168.56.11",
          :ssh_port => '2201'
        },
        {
          :hostname => "node2",
          :box => "bento/ubuntu-22.04",
          :ip => "192.168.56.12",
          :ssh_port => '2202'
        },
        {
          :hostname => "node3",
          :box => "bento/ubuntu-22.04",
          :ip => "192.168.56.13",
          :ssh_port => '2203'
        }
      ]

    servers.each do |machine|
        config.vm.define machine[:hostname] do |node|
            node.vm.box = machine[:box]
            node.vm.hostname = machine[:hostname]
            node.vm.network :private_network, ip: machine[:ip]
            node.vm.network "forwarded_port", guest: 22, host: machine[:ssh_port], id: "ssh"
            node.vm.provider :virtualbox do |vb|
                vb.customize ["modifyvm", :id, "--memory", 512]
                vb.customize ["modifyvm", :id, "--cpus", 1]
            end
        end
    end
end
```
Step 5: Start the vagrant that we create a script by using the command:

```
Shell Command
$ vagrant up

```

Step 6: Access the workstion name "Control" vm

```
Shell Command
$ vagrant ssh control

```

Step 7: access the mount folder of vagrant
```
Shell Command
$ cd /vagrant

```
Note: Option for us there will be no coding in your part ;)

Step 7.1: Copy the host file given to the workstation
```
Shell Command
$ sudo cp hosts /etc/hosts

```

Step 8: add a ip for each node and workstation for us to have a nameresolution

```
Shell Command
$ sudo vim /etc/hosts

```

Step 9:  Add the following in the host file
```
192.168.56.10 control
192.168.56.11 node1
192.168.56.12 node2
192.168.56.13 node3
```
Step 10: Try to connect server using ssh
```
Shell Command

$ ssh vagrant@node1

``

Step 11: Make a copy of ssh key to our servers, 
In making a keygen in our workstation the procedure in allowing all with no passphrase

```
Shell Command

$ ssh-keygen

``

Step 12: Copy the generate key to the server and test it ping using access e.g ssh vagrant@node1

```
Shell Command

$ ssh-copy-id node1 && ssh-copy-id node2 && ssh-copy-id node3

``
Step 13: Create a ansible inventory and playbook in /vagrant

Note: Here are the link for the documentation of making basic inventory https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html

```
Shell Command

$ touch inventory
$ touch playbook.yml

``
