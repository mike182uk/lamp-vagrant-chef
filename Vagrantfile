USE_PRECONFIGURED_LAMP_BOX = false

Vagrant.configure("2") do |config|
    # Vagrant plugins config
    config.cache.scope = :box
    config.omnibus.chef_version = :latest
    config.berkshelf.enabled = true

    # Box
    if USE_PRECONFIGURED_LAMP_BOX
        config.vm.box = "lamp-vagrant-chef-solo"
        config.vm.box_url = "https://dl.dropboxusercontent.com/u/13070740/vagrant-base-boxes/lamp-vagrant-chef-solo.box"
    else
        config.vm.box = "precise64"
        config.vm.box_url = "http://files.vagrantup.com/precise64.box"
    end

    # Networking
    config.vm.network :private_network, ip: "10.10.10.2"

    # Synced folders
    config.vm.synced_folder "./", "/var/www", type: "nfs"

    # Provision via chef solo
    config.vm.provision :chef_solo do |chef|
        chef.add_recipe "apt"
        chef.add_recipe "vim"
        chef.add_recipe "apache2"
        chef.add_recipe "apache2::mod_rewrite"
        chef.add_recipe "apache2::mod_alias"
        chef.add_recipe "apache2::mod_php5"
        chef.add_recipe "base::mysql"
        chef.add_recipe "php"
        chef.add_recipe "php::module_apc"
        chef.add_recipe "php::module_curl"
        chef.add_recipe "php::module_gd"
        chef.add_recipe "php::module_mcrypt"
        chef.add_recipe "php::module_mysql"
        chef.add_recipe "php::apache2"
        chef.add_recipe "xdebug"

        chef.json = {
            "apache" => {
                "default_site_enabled" => true
            },
            "php" => {
                "ini_settings" => {
                    "date.timezone" => "Europe/London"
                }
            },
            "xdebug" => {
                "version" => "2.2.7",
                "config_file" => "/etc/php5/conf.d/xdebug.ini",
                "directives" => {
                    "remote_autostart" => 1,
                    "remote_connect_back" => 1,
                    "remote_enable" => 1,
                    "remote_log" => "/tmp/xdebug-remote.log"
                }
            }
        }
    end
end
