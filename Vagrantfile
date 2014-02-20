Vagrant.configure("2") do |config|
 
  #config.berkshelf.enabled = true
  #config.omnibus.version = :lastest
  config.cache.auto_detect = true

  config.vm.hostname = "gems-amember"
  config.vm.box = "precise64chef11"


  [1].each do |node_num|

     config.vm.provider :virtualbox do |vb|
      # Don't boot with headless mode
      # vb.gui = true
      # Use VBoxManage to customize the VM. For example to change memory:
      # vb.customize ["modifyvm", :id, "--memory", "1536"]
    end
    
    node = "gems-amember#{node_num}".to_sym
    config.vm.define node do |node_config|
      # node_config.vm.network :public_network , ip: "192.168.1.191", netmask: "255.255.255.0"
      node_config.vm.network :public_network, ip: "192.168.1.#{121 + node_num}", netmask: "255.255.255.0"
      node_config.vm.network :private_network, ip: "33.33.33.#{121 + node_num}"
      node_config.vm.host_name = node.to_s
      node_config.vm.provision :chef_solo do |chef|
        chef.cookbooks_path = %w(cookbooks berks-cookbooks site-cookbooks)
        chef.roles_path     = 'roles'
        chef.data_bags_path = 'data_bags'

           chef.json = { 
               :mysql => {
                  :server_root_password => 'rootpass',
                  :server_debian_password => 'debpass',
                  :server_repl_password => 'replpass'
                }
               # "php" => {
               #    "install_method" => "source"
               #  }
          }   
        chef.add_recipe "apt"
        chef.add_recipe "build-essential"
        chef.add_recipe "git"
        chef.add_recipe "vim"
        chef.add_recipe "php"
        chef.add_recipe "php::module_mysql"
        chef.add_recipe "apache2"
        chef.add_recipe "apache2::mod_php5"
        chef.add_recipe "apache2::mod_rewrite"
        chef.add_recipe "mysql::server" 
        chef.add_recipe "ioncube"
        chef.log_level = :debug
      end
    end
  end
end

