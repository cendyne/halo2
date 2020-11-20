$script = <<SCRIPT
  echo "=== Updating packages"
  sudo DEBIAN_FRONTEND=noninteractive apt-get install -y aptitude
  sudo DEBIAN_FRONTEND=noninteractive aptitude update
  sudo DEBIAN_FRONTEND=noninteractive aptitude -y safe-upgrade

  echo "=== Installing new packages"
  sudo DEBIAN_FRONTEND=noninteractive aptitude install -y build-essentials libcurl4-gnutls-dev curl git-core vim cmake

  echo "=== Installing janet"
  sudo git clone https://github.com/janet-lang/janet.git /tmp/janet
  cd /tmp/janet
  sudo make all test install
  sudo chmod 777 /usr/local/lib/janet

  echo "=== Setting default editor to vim"
  sudo echo "export EDITOR='vim'" >> /home/vagrant/.bashrc
  sudo chown vagrant:vagrant /home/vagrant/.bashrc

  echo "*********************"
  echo "PROVISIONING FINISHED"
  echo "*********************"
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.hostname = "halo2"
  config.vm.network :forwarded_port, guest: 9021, host: 9021

  config.vm.provision :shell, inline: $script, privileged: false

  # Performance optimizations
  config.vm.provider "virtualbox" do |v|
    v.customize ["guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 5000]
  end
end