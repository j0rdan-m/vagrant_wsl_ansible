Vagrant.configure("2") do |config|

  config.vm.box = "mcree/win2019"
  config.vm.box_version = "1.0.1584095692"

  config.vm.provider "virtualbox" do |v|
    v.name = "windows2019"
    v.memory = "2048"
  end

  config.vm.provider "virtualbox" do |v|
    v.name = "win2k19"
    v.memory = "2048"
  end

  config.vm.provision "shell", inline: <<-SHELL
  
    Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
    # choco install git -y
    # choco install vscode -y
    # choco install hugo -y
    Enable-WindowsOptionalFeature -FeatureName Microsoft-Windows-Subsystem-Linux -Online -NoRestart -WarningAction SilentlyContinue
  SHELL
  
  # trigger reload
  config.vm.provision :reload

  # execute code after reload
  config.vm.provision "shell", inline: <<-SHELL
    choco install wsl-kalilinux -y
  SHELL

  # trigger reload
  config.vm.provision :reload

  # execute code after reload
  config.vm.provision "shell", inline: <<-SHELL
    wsl sudo apt update
    wsl sudo apt install -y ansible
    wsl sudo apt install -y unzip
    wsl sudo bash -c "echo [self] >> /etc/ansible/hosts"
    wsl sudo bash -c "echo 127.0.0.1 >> /etc/ansible/hosts"
    wsl sudo bash -c "echo [self:vars] >> /etc/ansible/hosts"
    wsl sudo bash -c "echo ansible_port=5985 >> /etc/ansible/hosts"
    wsl sudo bash -c "echo ansible_connection=winrm >> /etc/ansible/hosts"
    wsl sudo bash -c "echo ansible_winrm_transport=basic >> /etc/ansible/hosts"
    wsl sudo bash -c "echo ansible_user=vagrant >> /etc/ansible/hosts"
    wsl sudo bash -c "echo ansible_password=vagrant >> /etc/ansible/hosts"
    wsl ansible self -m win_ping
    wsl wget https://github.com/j0rdan-m/vagrant_wsl_ansible/archive/master.zip
    wsl unzip master.zip
    wsl ansible-playbook vagrant_wsl_ansible-master/tools.yml
    wsl ansible-playbook vagrant_wsl_ansible-master/iis.yml
    wsl ansible-playbook vagrant_wsl_ansible-master/users.yml
  SHELL
end
