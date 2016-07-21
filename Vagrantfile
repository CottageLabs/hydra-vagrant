# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.
  if ENV['DO_TOKEN'].nil?
    raise "Make sure the DO_TOKEN (Digital Ocean Personal Access API token) env variable is set correctly. An easy way is to put it in a gitignored env.sh file and do `. env.sh` before running Vagrant on this Vagrantfile. Put the env.sh outside of this repo however - the contents of this repo are uploaded to the VM after it's created."
  end

  config.vm.hostname = "hydrademo"

  config.vm.box = "ubuntu/trusty64"

  config.vm.network :forwarded_port, guest: 3000, host: 3000 # Rails
  config.vm.network :forwarded_port, guest: 8983, host: 8983 # Solr 5.4
  config.vm.network :forwarded_port, guest: 8984, host: 8984 # Fedora 4.5

  #config.vm.provider "virtualbox" do |v|
  #  v.memory = 2048
  #end
  config.ssh.username = 'vagrant'

  config.vm.provider :digital_ocean do |provider, override|
    provider.setup = true
    override.ssh.private_key_path = '~/.ssh/hydrademo'
    override.vm.box = 'digital_ocean'
    override.vm.box_url = "https://github.com/devopsgroup-io/vagrant-digitalocean/raw/master/box/digital_ocean.box"

    provider.token = ENV['DO_TOKEN']
    provider.image = 'ubuntu-14-04-x64'
    provider.region = 'lon1'
    provider.size = '2gb'
  end

  shared_dir = "/vagrant"

  config.vm.provision "shell", path: "./install_scripts/bootstrap.sh", args: shared_dir
  config.vm.provision "shell", path: "./install_scripts/java.sh", args: shared_dir
  config.vm.provision "shell", path: "./install_scripts/ruby.sh", args: shared_dir
  config.vm.provision "shell", path: "./install_scripts/fits.sh", args: shared_dir
  config.vm.provision "shell", path: "./install_scripts/demo-app.sh", args: shared_dir, privileged: false  # installs everything under /root, not /home/vagrant!

end
