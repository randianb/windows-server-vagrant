Vagrant.configure("2") do |config|

  puts "****** Setting up Windows Server 2019..."
  config.vm.define "windowsserver" do |windowsserver|
    # Use Windows Server 2019 box
    windowsserver.vm.box = "gusztavvargadr/windows-server"

    # Disk disk
    config.disksize.size = '130GB'

    # Configure VirtualBox provider settings
    windowsserver.vm.provider "virtualbox" do |vb|
      # Set name
      vb.name = "Windows Server 2019 by Matej"

      # Do not display the VirtualBox GUI when booting the machine
      vb.gui = false

      # Customize the resources on the VM (RAM and CPUs):
      vb.memory = "2048"
      vb.cpus = 2
      # If guest VM uses 100% of its CPU, only 80% of host's CPU is used
      vb.customize ["modifyvm", :id, "--cpuexecutioncap", "80"]

      # Enable sound (some games require soundcard and drivers)
      # This heavily depends on the host! Read more here:
      #   - https://github.com/paulsturgess/mopidy-vagrant/issues/3#issuecomment-36438930
      #   - https://github.com/srvk/eesen-transcriber/issues/1#issuecomment-152680334
      #   - https://www.virtualbox.org/manual/ch08.html#vboxmanage-cmd-overview
      # I disabled it because I did not need it
      vb.customize ["modifyvm", :id, "--audio", "none"]
      # Here is an example if you do need sound:
      #vb.customize ["modifyvm", :id, "--audio", "null"]
      #vb.customize ["modifyvm", :id, "--audiocontroller", "ac97"]
      ##vb.customize ["modifyvm", :id, "--audiocodec", "sb16"]
      #vb.customize ["modifyvm", :id, "--audioin", "on"]
      #vb.customize ["modifyvm", :id, "--audioout", "on"]

      # Set vram
      vb.customize ["modifyvm", :id, "--vram", "16"]
      # Set graphics controller
      vb.customize ["modifyvm", :id, "--graphicscontroller", "VBoxSVGA"]

    end

    # Run initial configuration
    windowsserver.vm.provision :shell, path: "./provisioning/init.ps1"

    # ADD VIRTUAL AUDIO CABLE SOFTWARE - A DUMMY SOUNDCARD AS SOME APPS REQUIRE SOUNDCARD DRIVERS TO workspace
    ## Downloaded from: https://www.vb-audio.com/Cable/
    windowsserver.vm.provision "file", source: "./provisioning/vbcable.zip", destination: "C:\\Users\\vagrant\\Desktop"

    # Network setup
    # Create bridge network to my ethernet adapter of my host, and add ip 192.168.1.199, mask 255.255.255.0, gateway 192.168.1.1, dns1 8.8.8.8, dns2 8.8.4.4
    config.vm.network "public_network", ip: "192.168.1.199", bridge: "enp4s0"
    config.vm.provision "shell",
      run: "always",
      inline: "netsh interface ip set address \"Ethernet 2\" static 192.168.1.199 255.255.255.0 192.168.1.1"
    config.vm.provision "shell",
      run: "always",
      inline: "netsh interface ip add dns \"Ethernet 2\" 8.8.8.8"
    config.vm.provision "shell",
      run: "always",
      inline: "netsh interface ip add dns \"Ethernet 2\" 8.8.4.4 index=2"

    # Forwarding range of guest TCP and UDP to host, or single ports
    # RDP
    config.vm.network :forwarded_port, guest: 3389, host: 3389, protocol: "tcp", auto_correct: true

    # Vietcong 1
    config.vm.network :forwarded_port, guest: 15425, host: 15425, protocol: "tcp", auto_correct: true
    config.vm.network :forwarded_port, guest: 15425, host: 15425, protocol: "udp", auto_correct: true
    config.vm.network :forwarded_port, guest: 5425, host: 5425, protocol: "tcp", auto_correct: true
    config.vm.network :forwarded_port, guest: 5425, host: 5425, protocol: "udp", auto_correct: true

    # Vietcong 2
    config.vm.network :forwarded_port, guest: 5000, host: 5000, protocol: "tcp", auto_correct: true
    config.vm.network :forwarded_port, guest: 5000, host: 5000, protocol: "udp", auto_correct: true
    config.vm.network :forwarded_port, guest: 15000, host: 15000, protocol: "tcp", auto_correct: true
    config.vm.network :forwarded_port, guest: 15000, host: 15000, protocol: "udp", auto_correct: true
    config.vm.network :forwarded_port, guest: 55154, host: 55154, protocol: "tcp", auto_correct: true
    config.vm.network :forwarded_port, guest: 55154, host: 55154, protocol: "udp", auto_correct: true
    config.vm.network :forwarded_port, guest: 19968, host: 19968, protocol: "tcp", auto_correct: true
    config.vm.network :forwarded_port, guest: 19968, host: 19968, protocol: "udp", auto_correct: true
    config.vm.network :forwarded_port, guest: 19967, host: 19967, protocol: "tcp", auto_correct: true
    config.vm.network :forwarded_port, guest: 19967, host: 19967, protocol: "udp", auto_correct: true

    # S.W.A.T. 4
    for i in 10480..10581
      config.vm.network :forwarded_port, guest: i, host: i, protocol: "tcp", auto_correct: true
      config.vm.network :forwarded_port, guest: i, host: i, protocol: "udp", auto_correct: true
    end

    # SSH
    config.vm.network :forwarded_port, guest: 22, host: 2222, protocol: "tcp", auto_correct: true, disabled: true
    config.vm.network :forwarded_port, guest: 22, host: 2222, protocol: "udp", auto_correct: true, disabled: true
    config.vm.network :forwarded_port, guest: 22, host: 2223, protocol: "tcp", auto_correct: true
    config.vm.network :forwarded_port, guest: 22, host: 2223, protocol: "udp", auto_correct: true

    # Range - uncomment and set correct range if needed
    #for i in 1..65535
    #  config.vm.network :forwarded_port, guest: i, host: i, protocol: "tcp", auto_correct: true
    #  config.vm.network :forwarded_port, guest: i, host: i, protocol: "udp", auto_correct: true
    #end
    windowsserver.vm.hostname = "Windows-Server-2019-VM"
  end
  puts "****** Set up done!"

end
