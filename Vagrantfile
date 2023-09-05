# -*- mode: ruby -*-
# vi: set ft=ruby :

# VirtualMachine Name
VM_NAME = "DevTastyTrade"

# vbguest
# "vagrant plugin install vagrant-vbguest"
# "vagrant vbguest"
# "vagrant vbguest --do install --force"

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  #config.vm.box = "ubuntu/focal64"										# Can't get audio to work. Always "Dummy Output". Missing snd-hda-intel.
  #config.vm.box = "generic/ubuntu1804"	
  #config.vm.box = "bento/ubuntu-20.04"									# Has Audio....After reboot, no audio, hang
  #config.vm.box = "bento/ubuntu-19.04"									# This is NOT LTS. End of support
  #config.vm.box = "generic/ubuntu2004"									# No Audio
# config.vm.box = "bento/ubuntu-18.04"									# 
#  config.vm.box = "jk/lubuntu-22.04"
  config.vm.box = "bento/ubuntu-22.04"

  #config.vm.box = "generic/ubuntu1904"									# Can't launch GUI. No Terminal Windows
  #config.vm.box = "viruzzo/xubuntu-xenial64"							# Good Audio, Use AC97. GLIBC_2_27 Required

  


  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false
  config.vbguest.auto_update = false
  

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.network :forwarded_port, guest: 22, host: 5668, id: "ssh", auto_correct: true
  
  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"
  config.vm.synced_folder ".", "/vagrant", disabled: false

  config.vm.define VM_NAME
  config.vm.hostname = VM_NAME

  #config.ssh.insert_key = true
  #config.ssh.username = 'ubuntu'
  #config.ssh.password = '379a8188cba8c5e64b9a429f'
  
  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
   config.vm.provider "virtualbox" do |vb|
     vb.name = VM_NAME

     # Display the VirtualBox GUI when booting the machine
     vb.gui = true
  
     # Video memory 256M
     vb.customize ["modifyvm", :id, "--vram", "256"]

     # Customize the amount of memory on the VM:
     vb.memory = "3524"
     
     #vb.default_nic_type = "82540EM"
     #vb.customize ["modifyvm", :id, "--nic2", "natnetwork", "--nat-network2", "Trade", "--nictype2", "82540EM"]
     #vb.customize ["modifyvm", :id, "--nic2", "natnetwork", "--nat-network2", "Trade", "--nictype2", "virtio"]

     # Multiple cpu
     vb.customize ["modifyvm", :id, "--cpus", "2"]

     # 3d acceleration
     vb.customize ['modifyvm', :id, '--accelerate3d', 'on']

     # VMSVGA graphics controller. vmsvga, vboxvga
     vb.customize ['modifyvm', :id, '--graphicscontroller', 'vmsvga']


     # USB
     vb.customize ["modifyvm", :id, "--usb", "on"]

     # Audio
     #vb.customize ["modifyvm", :id, "--audio", "dsound", "--audiocontroller", "hda"]
     #vb.customize ["modifyvm", :id, "--audio", "dsound", "--audiocontroller", "ac97"]
     vb.customize ["modifyvm", :id, "--audio", "alsa", "--audiocontroller", "ac97"]
	 
     # Enable Clipboard
     vb.customize ['modifyvm', :id, '--clipboard', 'bidirectional']

     # Use host DNS server
     vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
     vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]

   end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
   config.vm.provision "shell", inline: <<-SHELL
     apt-get -y update
     
     sudo apt-get install -y xfce4 xfce4-power-manager
     sudo apt-get install -y xfce4-screenshooter
     
     # Need to do it manually since have EULA prompt
     sudo add-apt-repository multiverse
     sudo apt update

     # Below will will EULA prompt
     sudo DEBIAN_FRONTEND=noninteractive apt-get install -y ubuntu-restricted-extras


	 #sudo apt-get -y upgrade
	 #sudo do-release-upgrade
	 #apt-get install -y xinit
	 
	 # MANUAL: Has prompt
     # sudo apt-get install -y xfce4
	 
	 # MANUAL: Restricted Extras....Need to run manually. Need to agree to EULA
	 # sudo apt-get install -y ubuntu-restricted-extras
	 # Test speaker: "speaker-test -t wav -c 6"
	 
     # Setup up sound driver
	 # See https://help.ubuntu.com/cummunity/SoundTroubleshootingProcedure
	 # Setup Audio in VirtualBox.  Enable Audio, Host Driver: Windows Direct Sound, Controller: ICH AC97
	 #sudo apt-add-repository ppa:ubuntu-audio-dev/alsa-daily
	 #sudo apt-get -y update
	 #sudo apt-get install -y linux-image-extra-`uname -r`
	 #sudo apt-get install -y --reinstall linux-image-extra-`uname -r`
	 #sudo apt-get install -y oem-audio-hda-daily-dkms

     # vlc
     sudo apt-get install -y vlc
	 
	 # MANUAL Steps
     # See https://wiki.ubuntu.com/Audio/UpgradingAlsa/DKMS
     # Go to this page:  https://code.launchpad.net/~ubuntu-audio-dev/+archive/ubuntu/alsa-daily/+packages
     # Download proper .deb file...e.g for 16.04Xenial download oem-audio-hda-daily-lts-xenial-dkms_0.201712110802-ubuntu16.04.1_all.deb
	 # "sudo dpkg -i ./oem-audio-hda-daily-lts-xenial-dkms_0.201712110802-ubuntu16.04.1_all.deb". This requires in virtual box, audio settings uses Intel HD Audio
     # Reboot
	 #wget https://code.launchpad.net/~ubuntu-audio-dev/+archive/ubuntu/alsa-daily/+files/oem-audio-hda-daily-dkms_0.201802112136~ubuntu16.04.1_all.deb
	 #sudo dpkg -i ./oem-audio-hda-daily-dkms_0.201802112136~ubuntu16.04.1_all.deb
	 
     #	 sudo apt-get install -y alsa
     #	 sudo apt-get install -y alsa-utils
     #	 pulseaudio --start
     #	 pavucontrol
	 
	 
     # Enable min/max/close buttons for windows
     gsettings set org.gnome.desktop.wm.preferences button-layout ":minimize,maximize,close"

     # Set timezone
     ln -sf /usr/share/zoneinfo/America/Los_Angeles /etc/localtime

     #encoding
     sudo echo "LANG=en_US.UTF-8" >> /etc/environment
     sudo echo "LANGUAGE=en_US.UTF-8" >> /etc/environment
     sudo echo "LC_ALL=en_US.UTF-8" >> /etc/environment
     sudo echo "LC_CTYPE=en_US.UTF-8" >> /etc/environment


     # Set Timezone ntp date
     sudo apt-get -y install ntp
     sudo apt-get -y install ntpdate
     ntpdate ntp.ubuntu.com pool.ntp.org
     # timedatectl list-timezones
     sudo timedatectl set-timezone America/Los_Angeles
     
     # Install dkms to resize screen
     #sudo apt-get install -y virtualbox-guest-dkms virtualbox-guest-utils
     # sudo apt-get install -y virtualbox-guest-x11
     sudo echo "allowed_users=anybody" > /etc/X11/Xwrapper.config
	 
     # Install firefox
     #sudo apt-get -y install firefox
	 
     # Add vagrant user
     #sudo useradd -m vagrant
     #sudo usermod -aG sudo vagrant
     #echo vagrant:vagrant | sudo /usr/sbin/chpasswd
	 
     # Add vagrant user to audio group
     sudo adduser vagrant audio

     # Allows copy and paste clipboard thru command-line
     sudo apt-get install -y xclip     

     # tasty dependency
     sudo apt-get install -y xdg-utils

     # Dev packages
     sudo apt-get install -y tesseract-ocr 
     
     # Network
     # Allow nonroot user to capture: "sudo dpkg-reconfigure wireshark-common", select Yes to non-superusers be able to capture packet.   
     # Add user to wireshark group"sudo usermod -a -G wireshark vagrant"
     # Change to executable permission: "sudo chmod +x /usr/bin/dumpcap"
     sudo DEBIAN_FRONTEND=noninteractive apt-get install -y tshark
     sudo DEBIAN_FRONTEND=noninteractive apt-get install -y wireshark
     sudo apt-get install -y ngrep
     sudo apt-get install -y tcpflow
     sudo apt-get install -y bettercap

     # Install netfilterqueue for python and libnetfilter
     sudo apt-get install -y build-essential python3-dev libnetfilter-queue-dev
     sudo apt-get install -y python3-pip

     # Install scapy.  To run "cd ~/src/scapy" "sudo ./run_scapy"
su vagrant << EOF
     pip install NetfilterQueue
     sudo pip install --pre scapy[complete]                       # "pip install --pre scapy[basic]" "pip install scapy"
     #mkdir -p ~/src
     #cd src
     #git clone --depth 1 https://github.com/secdev/scapy
     #cd scapy
     #mkdir -p build/lib                                          #
     #sudo python3 ./setup.py install                             # Setup scapy python
EOF
     
     

     # Install unbuffer
     sudo apt-get install -y expect
     
  

     #oracle java
     #See https://www.digitalocean.com/community/tutorials/how-to-install-java-on-ubuntu-with-apt-get
     #sudo apt-get install -y python-software-properties
     #sudo add-apt-repository ppa:webupd8team/java
     #sudo apt-get update
     
     # Need to install manually
     #sudo apt-get install -y oracle-java8-installer
     
     #Manage multiple java version, set which one to be default
     #sudo update-alternatives --config java     

	 # TastyWork as user vagrant
#	 sudo su vagrant <<END
#	 cd /vagrant
#	 dpkg -i -f tastyworks-1.8.0-1_amd64.deb
     # Will complain about missing packages. Use the command below
     # sudo apt-get -f install
      
#END	 
	 
   SHELL
end
