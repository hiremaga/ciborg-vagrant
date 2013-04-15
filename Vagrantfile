Vagrant.configure('2') do |config|
  config.ssh.forward_agent = true
  config.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true

  config.vm.define :local do |local|
    local.vm.box = "precise64"
    local.vm.network :forwarded_port, guest: 443, host: 8443
  end

  config.vm.define :remote do |remote|
    remote.vm.box = "precise64"
    remote.ssh.private_key_path = ENV['SSH_PRIVATE_KEY_PATH']
    remote.vm.provider :digital_ocean do |provider|
      provider.client_id = ENV['DIGITAL_OCEAN_CLIENT_ID']
      provider.api_key   = ENV['DIGITAL_OCEAN_API_KEY']
      provider.ca_path   = ENV['SSL_CERT_FILE']
      provider.size      = '2GB'
    end
  end

  config.vm.provision :shell, path: 'bootstrap.sh'

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["cookbooks"]
    chef.add_recipe 'pivotal_ci::jenkins'
    chef.add_recipe 'pivotal_ci::limited_travis_ci_environment'
    chef.add_recipe 'pivotal_ci'
    chef.json = {
      travis_build_environment: {
        :user => "jenkins",
        :group => "nogroup",
        :home => "/var/lib/jenkins"
      },
      nginx: {
        basic_auth_user: "ci",
        basic_auth_password: "changeme"
      },
      jenkins: {
        builds: []
      }
    }
  end
end
