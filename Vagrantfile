Vagrant.configure("2") do |config|

  DEBIAN = "debian/buster64"
  PFSENSE = "ksklareski/pfsense-ce"

  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = false
  end

  config.vm.provider "virtualbox" do |v|
    v.memory = 254
    v.cpus = 1
  end

  config.vm.define "hote" do |subconfig|
    subconfig.vm.box = DEBIAN
    subconfig.vm.hostname = "hote"
    subconfig.vm.network :private_network, ip: "10.0.1.2"
    subconfig.vm.synced_folder '.', '/vagrant', disabled: true
  end

  config.vm.define "pfw1" do |subconfig|
    subconfig.vm.box = DEBIAN
    subconfig.vm.hostname = "pfw1"
    subconfig.vm.network :private_network, ip: "10.0.2.1"
    subconfig.vm.network :private_network, ip: "10.0.1.1"
    subconfig.vm.provision "shell", privileged: true, run: "always",inline: "echo 'net.ipv4.ip_forward=1' >> /etc/sysctl.conf"
    subconfig.vm.synced_folder '.', '/vagrant', disabled: true
  end

  config.vm.define "http" do |subconfig|
    subconfig.vm.box = DEBIAN
    subconfig.vm.hostname = "http"
    subconfig.vm.network :private_network, ip: "10.0.2.5"
    subconfig.vm.network :private_network, ip: "10.0.3.5"
    subconfig.vm.provision "shell", privileged: true, run: "always",inline: "apt update --fix-missing"
    subconfig.vm.provision "shell", privileged: true, run: "always",inline: "apt install net-tools -y"
    subconfig.vm.provision "shell", privileged: true, run: "always",inline: "eval `route -n | awk '{ if ($8 ==\"eth0\" && $2 != \"0.0.0.0\") print \"route del default gw \" $2; }'`"
    subconfig.vm.provision "shell", privileged: true, run: "always",inline: "route add default gw 10.0.2.1"
    subconfig.vm.synced_folder '.', '/vagrant', disabled: true
  end

  config.vm.define "router1" do |subconfig|
    subconfig.vm.box = DEBIAN
    subconfig.vm.hostname = "router1"
    subconfig.vm.network :private_network, ip: "10.0.2.3"
    subconfig.vm.network :private_network, ip: "10.0.3.3"
    subconfig.vm.provision "shell", privileged: true, run: "always",inline: "echo \"nameserver 8.8.8.8\" >> /etc/resolv.conf"
    subconfig.vm.provision "shell", privileged: true, run: "always",inline: "apt update --fix-missing"
    subconfig.vm.provision "shell", privileged: true, run: "always",inline: "apt install net-tools -y"
    subconfig.vm.provision "shell", privileged: true, run: "always",inline: "eval `route -n | awk '{ if ($8 ==\"eth0\" && $2 != \"0.0.0.0\") print \"route del default gw \" $2; }'` "
    subconfig.vm.provision "shell", privileged: true, run: "always",inline: "route add default gw 10.0.2.1"
    subconfig.vm.synced_folder '.', '/vagrant', disabled: true
  end

  config.vm.define "dns1" do |subconfig|
    subconfig.vm.box = DEBIAN
    subconfig.vm.hostname = "dns1"
    subconfig.vm.network :private_network, ip: "10.0.2.4"
    subconfig.vm.network :private_network, ip: "10.0.3.4"
    subconfig.vm.provision "shell", privileged: true, run: "always",inline: "apt update --fix-missing"
    subconfig.vm.provision "shell", privileged: true, run: "always",inline: "apt install net-tools bind9 dnsutils -y "
    subconfig.vm.provision "shell", privileged: true, run: "always",inline: "eval `route -n | awk '{ if ($8 ==\"eth0\" && $2 != \"0.0.0.0\") print \"route del default gw \" $2; }'` "
    subconfig.vm.provision "shell", privileged: true, run: "always",inline: "route add default gw 10.0.2.1"
    subconfig.vm.synced_folder '.', '/vagrant', disabled: true
  end

  config.vm.define "pfw2" do |subconfig|
    subconfig.vm.box = DEBIAN
    subconfig.vm.hostname = "pfw2"
    subconfig.vm.network :private_network, ip: "10.0.3.1"
    subconfig.vm.network :private_network, ip: "10.0.4.1"
    subconfig.vm.network :private_network, ip: "10.0.5.1"
    subconfig.vm.network :private_network, ip: "10.0.6.1"
    subconfig.vm.provision "shell", privileged: true, run: "always", inline: "apt update --fix-missing"
    subconfig.vm.provision "shell", privileged: true, run: "always",inline: "apt install net-tools -y"
    subconfig.vm.provision "shell", privileged: true, run: "always",inline: "echo 'net.ipv4.ip_forward=1' >> /etc/sysctl.conf"
    subconfig.vm.provision "shell", privileged: true, run: "always",inline: "eval `route -n | awk '{ if ($8 ==\"eth0\" && $2 != \"0.0.0.0\") print \"route del default gw \" $2; }'`"
    subconfig.vm.synced_folder '.', '/vagrant', disabled: true
  end

  config.vm.define "dns2" do |subconfig|
    subconfig.vm.box = DEBIAN
    subconfig.vm.hostname = "dns2"
    subconfig.vm.network :private_network, ip: "10.0.6.2"
    subconfig.vm.provision "shell", privileged: true, run: "always",inline: "apt update --fix-missing"
    subconfig.vm.provision "shell", privileged: true, run: "always",inline: "apt install net-tools bind9 dnsutils -y "
    subconfig.vm.synced_folder '.', '/vagrant', disabled: true
  end

  config.vm.define "admin" do |subconfig|
    subconfig.vm.box = DEBIAN
    subconfig.vm.hostname = "admin"
    subconfig.vm.network :private_network, ip: "10.0.4.2"
    subconfig.vm.synced_folder '.', '/vagrant', disabled: true
  end

  config.vm.define "pc1" do |subconfig|
    subconfig.vm.box = DEBIAN
    subconfig.vm.hostname = "pc1"
    subconfig.vm.network :private_network, ip: "10.0.5.2"
    subconfig.vm.synced_folder '.', '/vagrant', disabled: true
  end

  config.vm.define "pc2" do |subconfig|
    subconfig.vm.box = DEBIAN
    subconfig.vm.hostname = "pc2"
    subconfig.vm.network :private_network, ip: "10.0.5.3"
    subconfig.vm.synced_folder '.', '/vagrant', disabled: true
  end
end
