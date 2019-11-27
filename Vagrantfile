# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.require_version ">= 1.8.4"

Vagrant.configure("2") do |config|
  config.vm.communicator = "winrm"

  config.vm.synced_folder ".", "/vagrant", disabled: true
  home = ENV['HOME'].gsub('\\', '/')
  config.vm.synced_folder home, home

  config.vm.network "forwarded_port", guest: 1433, host: 1433
  config.vm.network "forwarded_port", guest: 9000, host: 9000

  config.vm.define "docker-2019" do |cfg|
    cfg.vm.box = "2019-docker-parallels"
    cfg.vm.provision "shell", path: "#{__dir__}/scripts/create-machine.ps1", args: "-machineHome #{home} -machineName docker-2019"
    cfg.vm.provider "parallels" do |prl|
      prl.name = "docker-2019"
    end
  end

  config.vm.define "docker-2019-lcow", autostart: false do |cfg|
    cfg.vm.box = "2019-docker-parallels"
    cfg.vm.provision "shell", path: "#{__dir__}/scripts/create-machine.ps1", args: "-machineHome #{home} -machineName docker-2019-lcow -enableLCOW"
    cfg.vm.provider "parallels" do |prl|
      prl.name = "docker-2019-lcow"
      prl.memory = 5120
    end
  end

  config.vm.provider "parallels" do |prl|
    prl.cpus = 2
    prl.memory = 2048
    prl.update_guest_tools = true
  end
end
