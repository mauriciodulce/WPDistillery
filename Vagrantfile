# -*- mode: ruby -*-
# vi: set ft=ruby :
#
# WPDistillery Vagrantfile for Scotch Box
#
# File Version: 1.0

Vagrant.configure("2") do |config|

    config.vm.box = "scotch/box"
    config.vm.network "private_network", ip: "192.168.33.10"
    config.vm.hostname = "wpdsrc.dev" # can i read this form config? #
    config.vm.synced_folder ".", "/var/www", :mount_options => ["dmode=777", "fmode=666"]

    # Windows Support
    if Vagrant::Util::Platform.windows?
      config.vm.provision "shell",
      inline: "echo \"Converting Files For Windows\" && sudo apt-get install -y dos2unix && dos2unix wpdistillery/config.yml && dos2unix wpdistillery/provision.sh && dos2unix wpdistillery/wpdistillery.sh",
      run: "always", privileged: false
    end

    # Run Provisioning (update wp cli, set upload size to 64MB, run WPDistillery)
    # executed within the first `vagrant up` and every `vagrant provision`
    config.vm.provision "shell", path: "wpdistillery/provision.sh"

    # Update WordPress and all Plugins on vagrant up
    # executed within every `vagrant up`
    config.vm.provision "shell",
    inline: "echo \"== Update WordPress & Plugins ==\" && cd /var/www/public && wp core update && wp plugin update --all", run: "always", privileged: false

    # Enable NFS
    # config.vm.synced_folder ".", "/var/www", :nfs => { :mount_options => ["dmode=777","fmode=666"] }

end
