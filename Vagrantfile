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
    sudo apt-get install -y apache2 curl cron certbot python3-certbot-apache

    # Habilitamos el inicio automático de apache y de cron
    sudo systemctl enable apache2
    sudo systemctl enable cron

    # Copiamos los archivos del sitio web a la máquina virtual
    sudo cp -r /vagrant/mblesapardo.es/* /var/www/html/
    sudo cp /vagrant/mblesapardo.es/apache2.conf /etc/apache2/sites-available/000-default.conf
    sudo cp /vagrant/mblesapardo.es/apache2-https.conf /etc/apache2/sites-available/default-ssl.conf

    # Asignamos los permisos adecuados
    sudo chown -R www-data:www-data /var/www/html/
    sudo chmod -R 755 /var/www/html/

    # Creamos el archivo .htpasswd y agregamos los usuarios con las contraseñas
    sudo htpasswd -b -c /etc/apache2/.htpasswd admin asir
    sudo htpasswd -b /etc/apache2/.htpasswd sysadmin risa

    # Ejecutamos certbot para obtener los certificados
    sudo certbot --apache -d mblesapardo.es -d www.mblesapardo.es --email mblesapardo1@gmail.com --agree-tos --no-eff-email

    # Habilitamos SSL en Apache
    sudo a2enmod ssl
    sudo a2ensite default-ssl.conf
        
    # Reiniciamos Apache para aplicar los cambios
    sudo systemctl restart apache2

  SHELL
  

end