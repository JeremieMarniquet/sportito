$BOX = "ubuntu/xenial64"
$IP = "10.0.0.45"
$MEMORY = ENV.has_key?('VM_MEMORY') ? ENV['VM_MEMORY'] : "1024"
$CPUS = ENV.has_key?('VM_CPUS') ? ENV['VM_CPUS'] : "1"

required_plugins = ["vagrant-hostsupdater"]
required_plugins.each do |plugin|
    unless Vagrant.has_plugin? plugin
        system "vagrant plugin install #{plugin}" || exit!
        exec "vagrant #{ARGV.join' '}"
    end
end

Vagrant.configure("2") do |config|
  config.vm.hostname = "sportito.dev"
  config.vm.box = $BOX
  config.vm.network :private_network, ip: $IP
  config.ssh.forward_agent = true

  config.vm.synced_folder ".", "/var/www/sportito/current", type: "nfs"
  config.hostsupdater.aliases = ["sportito.local"]

  config.vm.provider "virtualbox" do |v|
    v.name = "sportito"
    v.customize ["modifyvm", :id, "--cpuexecutioncap", "100"]
    v.customize ["modifyvm", :id, "--memory", $MEMORY]
    v.customize ["modifyvm", :id, "--cpus", $CPUS]
  end
end
