Vagrant.configure("2") do |config|
  config.vm.box = 'tdb-ubuntu1204-v9'
  config.vm.box_url = 'http://ci.mojolingo.com/job/Telephony-Dev-Box-Base-Boxen/9/artifact/tdb-ubuntu1204.box'
  config.berkshelf.enabled = true

  config.vm.define :freeswitch do |freeswitch|
    public_ip = "10.203.175.79"
    domain    = "freeswitch.local-dev.my-ahn-app.com"

    freeswitch.vm.network :private_network, ip: public_ip
    freeswitch.vm.hostname = domain

    freeswitch.vm.provider :virtualbox do |vb|
      vb.name = "TDB-freeswitch"
      vb.customize ["modifyvm", :id, "--memory", 1024]
    end

    freeswitch.vm.provision :chef_solo do |chef|
      chef.cookbooks_path = "cookbooks"
      chef.roles_path     = "roles"

      chef.add_role "freeswitch"
      chef.add_recipe "mysql::server"
      chef.add_recipe "redisio::install"
      chef.add_recipe "redisio::enable"
      chef.add_recipe "ejabberd"

      chef.log_level = :debug

      chef.json = {
        freeswitch: {
          local_ip: public_ip,
          domain: domain
        },
        mysql: {
          server_debian_password: "foobar",
          server_repl_password: "foobar",
          server_root_password: 'foobar'
        },
        ejabberd: {
          jabber_domain: public_ip
        }
      }
    end
  end
end
