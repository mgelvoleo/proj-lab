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
