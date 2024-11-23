Vagrant.configure("2") do |config|
  servers=[
    {
      :hostname => "nfs-server",
      :box => "boxomatic/centos-stream-9",
      :ip => "192.168.56.101",
      :ssh_port => '2210'
    },
    {
      :hostname => "client1",
      :box => "boxomatic/centos-stream-9",
      :ip => "192.168.56.102",
      :ssh_port => '2211'
    },
    {
      :hostname => "client2",
      :box => "boxomatic/centos-stream-9",
      :ip => "192.168.56.103",
      :ssh_port => '2212'
    }, 
    {
      :hostname => "client3",
      :box => "boxomatic/centos-stream-9",
      :ip => "192.168.56.104",
      :ssh_port => '2213'
    },
    {
      :hostname => "client4",
      :box => "boxomatic/centos-stream-9",
      :ip => "192.168.56.105",
      :ssh_port => '2214'
    }
  ]

  servers.each do |machine|

    config.vm.define machine[:hostname] do |node|
      node.vm.box = machine[:box]
      node.vm.hostname = machine[:hostname]
    
      node.vm.network :private_network, ip: machine[:ip]
      node.vm.network "forwarded_port", guest: 22, host: machine[:ssh_port], id: "ssh"

      node.vm.provider :virtualbox do |v|
        v.customize ["modifyvm", :id, "--memory", 2048]
        v.customize ["modifyvm", :id, "--name", machine[:hostname]]
      end
    end
  end

  id_rsa_key_pub = File.read(File.join(Dir.home, ".ssh", "id_rsa.pub"))

  config.vm.provision :shell,
        :inline => "echo 'appending SSH public key to ~vagrant/.ssh/authorized_keys' && echo '#{id_rsa_key_pub }' >> /home/vagrant/.ssh/authorized_keys && chmod 600 /home/vagrant/.ssh/authorized_keys"

  config.ssh.insert_key = false
end