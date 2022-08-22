# Project Provisioning 3 nodes

## Step 1: Install the vagrant

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
Step 14: Dockerize sample python apps

I have a separate tutorial in dockerize the python apps

Step 15: Update the inventory in thru this belown
```
[control]
control


[nodes]
node1
node2
node3



[manager]
node1

[worker]
node2
node3

```       
Step 16: Clone or fork the ansible swarm playbook 

```
Shell Command

$ git clone https://github.com/mgelvoleo/ansible-swarm-playbook

```


Step 17: Open the swarm.yml and replace in the playbook that has a eth0 to eth1

Step 18: Run the playbook

```
Shell Command

$ ansible-playbook -i inventory -K swarm.yml

```

Step 19: Access the each node with ssh and type the command to check if the swarm is working

```
Shell Command

$  ssh node1
$  docker node ls

```

Step 20: Pull the images from the dockerhub of the python apps to swarm Note: You should be in the properpery folder to execute the command

```
Shell Command

$  docker stack deploy --compose-file docker-compose.yml myapp



```


Step 21: View the successfull deploy of images in the swarm
```
Shell Command

$  docker stack ls
$  docker stack services myapp
```
Step 22: Scale the services that we have by typing:
```
Shell Command
$ docker service scale  myapp_helloworld=3
$ docker service ps myapp_web
```










