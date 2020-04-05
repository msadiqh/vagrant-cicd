Vagrant.configure("2") do |config|

  config.vm.box = "centos/7"

  config.vm.define "jenkins_box" do |jenkins|
      jenkins.vm.hostname = "jenkins.local"
      jenkins.vm.network :private_network, ip: "192.168.2.101"
      jenkins.vm.network :forwarded_port, host: 8080, guest: 8080
      jenkins.vm.provider :virtualbox do |v|
         v.gui = false
         v.memory = 4096
      end
  end
  
  config.vm.define "awx_box" do |awx|
      awx.vm.hostname = "awx.local"
      awx.vm.box = "scorputty/centos-awx"
      awx.vm.network :private_network, ip: "192.168.2.100"
      awx.vm.network :forwarded_port, host: 80, guest: 80
      awx.vm.provider :virtualbox do |v|
         v.gui = false
         v.memory = 4096
         v.cpus = 2
         
      end
  end


  config.vm.define "sonar_box" do |sonar|
    sonar.vm.hostname = "sonar.local"
    sonar.vm.network :private_network, ip: "192.168.2.102"
    sonar.vm.network :forwarded_port, host: 9000, guest: 9000
    sonar.vm.provider :virtualbox do |v|
        v.gui = false
        v.memory = 4096
    end
 end

  config.vm.define "nexus_box" do |nexus|
    nexus.vm.hostname = "nexus.local"
    nexus.vm.network :private_network, ip: "192.168.2.103"
    nexus.vm.network :forwarded_port, host: 8081, guest: 8081
    nexus.vm.provider :virtualbox do |v|
        v.gui = false
        v.memory = 1024
    end
  end

  config.vm.define "app_box" do |app|
      app.vm.hostname = "app.local"
      app.vm.network :private_network, ip: "192.168.2.104"
      app.vm.network :forwarded_port, host: 9090, guest: 8080
      app.vm.provider :virtualbox do |v|
        v.gui = false
        v.memory = 3000
      end
  end

  config.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/alm.yml"

      ansible.groups = {
          "jenkins_server" => ["jenkins", "jenkins_box"],
          "sonar_server" => ["sonar", "sonar_box"],
          "nexus_server" => ["nexus", "nexus_box"],
          "awx_server" => ["awx", "awx_box"],
          "app_server" => ["app", "app2", "app_box", "app2_box"],
      }
  end

  if Vagrant.has_plugin?("vagrant-hostmanager")
      config.hostmanager.enabled = true
      config.hostmanager.manage_host = true
      config.hostmanager.manage_guest = true
  end
end

