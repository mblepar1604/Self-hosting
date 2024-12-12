Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"
  config.vm.hostname = "selfhosting-debian"
  
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 443, host: 8443

  config.vm.network "public_network", bridge: "en0"

  config.vm.provision "shell", inline: <<-SHELL

    # Actualizamos los repositorios
    sudo apt update -y

    # Instalamos apache y curl
    sudo apt-get install -y apache2 curl cron

    # Habilitamos el inicio automático de apache y de cron
    sudo systemctl enable apache2
    sudo systemctl enable cron

  SHELL

  config.vm.provision "shell", inline: <<-SHELL

  sudo apt install certbot python3-certbot-apache
  sudo certbot --apache -d mblesapardo.es -d www.mblesapardo.es

  SHELL

end