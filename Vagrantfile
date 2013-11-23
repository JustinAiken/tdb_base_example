Vagrant.configure("2") do |config|
  config.vm.box = 'precise64'
  config.vm.box_url = 'http://files.vagrantup.com/precise64.box'
  config.librarian_chef.cheffile_dir = "."

  config.vm.define :freeswitch do |freeswitch|
    public_ip = "10.203.175.73"

    freeswitch.vm.network :private_network, ip: public_ip
    freeswitch.vm.hostname = "freeswitch.local-dev.my-ahn-app.com"

    freeswitch.vm.provider :virtualbox do |vb|
      vb.name = "TDB-freeswitch"
    end

    freeswitch.vm.provision :chef_solo do |chef|
      chef.cookbooks_path = "cookbooks"
      chef.data_bags_path = "data_bags"
      chef.add_recipe "apt"
      chef.add_recipe "mysql::server"
      chef.add_recipe "freeswitch::default"
      chef.add_recipe "chef-solo-search"
      chef.add_recipe "ejabberd"
      chef.add_recipe "redisio::install"
      chef.add_recipe "redisio::enable"

      chef.log_level = :debug

      chef.json = {
        freeswitch: {
          tls_only: false,
          local_ip: public_ip,
          dialplan: {
            head_fragments: '<extension name="adhearsion">
              <condition>
                <action application="rayo"/>
              </condition>
            </extension>'
          }
        },
        mysql: {
          server_debian_password: "foobar",
          server_repl_password: "foobar",
          server_root_password: 'foobar'
        },
        ejabberd: {
          domain: public_ip,
          registration_allowed: true
        },
        jabber_domain: public_ip
      }
    end
  end
end
