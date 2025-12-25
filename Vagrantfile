ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'

Vagrant.configure("2") do |config|
  config.vm.box = "centos/8"

  config.vm.provider :virtualbox do |v|
    v.memory = 4096
    v.cpus = 2
  end

  boxes = [
    { :name => "ipa.otus.lan", :ip => "192.168.57.10" },
    { :name => "client1.otus.lan", :ip => "192.168.57.11" },
    { :name => "client2.otus.lan", :ip => "192.168.57.12" }
  ]

  boxes.each do |opts|
    config.vm.define opts[:name] do |node|
      node.vm.hostname = opts[:name]
      node.vm.network "private_network", ip: opts[:ip]
    end
  end
end
