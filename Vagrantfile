PROVISION = <<-SCRIPT
sudo apt-get update -y
sudo apt-get install python3 -y
[ -d /home/ubuntu/.ssh ] && echo '%{public_key}' >> /home/ubuntu/.ssh/authorized_keys; true
[ -d /home/ubuntu/.ssh ] && chmod 600 /home/ubuntu/.ssh/authorized_keys; true
[ -d /home/vagrant/.ssh ] && echo '%{public_key}' >> /home/vagrant/.ssh/authorized_keys; true
[ -d /home/vagrant/.ssh ] && chmod 600 /home/vagrant/.ssh/authorized_keys; true
SCRIPT

def define_vm(config, hostname, ip)
  public_key_path = File.join(Dir.home, ".ssh", "id_rsa.pub")
  public_key = IO.read(public_key_path)

  config.vm.define hostname do |vm|
      vm.vm.box = "ubuntu/bionic64"
      config.vm.box_url =
        "http://cloud-images.ubuntu.com/bionic/current/bionic-server-cloudimg-amd64-vagrant.box"
      vm.vm.hostname = hostname
      vm.vm.network 'private_network', ip: ip

      vm.vm.provision :shell, inline: (PROVISION % { public_key: public_key })
  end
end

Vagrant.configure("2") do |config|
  define_vm(config, "wireguard", "10.1.1.254")
end
