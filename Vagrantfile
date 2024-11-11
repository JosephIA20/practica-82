Vagrant.configure("2") do |config|
    # Configuración para el servidor web Apache 1
    config.vm.define "apache1" do |apache1|
      apache1.vm.box = "ubuntu/bionic64"
      apache1.vm.memory = 256
      apache1.vm.cpus = 1
      apache1.vm.network "private_network", type: "dhcp"
  
      # Carpeta compartida para el servidor Apache
      apache1.vm.synced_folder "./www", "/var/www/html"
  
      # Provisionamiento para instalar Apache
      apache1.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
        sudo systemctl enable apache2
        sudo systemctl start apache2
      SHELL
    end
  
    # Configuración para el servidor web Apache 2
    config.vm.define "apache2" do |apache2|
      apache2.vm.box = "ubuntu/bionic64"
      apache2.vm.memory = 256
      apache2.vm.cpus = 1
      apache2.vm.network "private_network", type: "dhcp"
  
      # Carpeta compartida para el servidor Apache
      apache2.vm.synced_folder "./www", "/var/www/html"
  
      # Provisionamiento para instalar Apache
      apache2.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
        sudo systemctl enable apache2
        sudo systemctl start apache2
      SHELL
    end
  
    # Configuración para el balanceador de carga Nginx
    config.vm.define "nginx" do |nginx|
      nginx.vm.box = "ubuntu/bionic64"
      nginx.vm.memory = 256
      nginx.vm.cpus = 1
      nginx.vm.network "private_network", type: "dhcp"
  
      # Provisionamiento para instalar Nginx
      nginx.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y nginx
        sudo systemctl enable nginx
        sudo systemctl start nginx
  
        # Configuración de Nginx para balanceo de carga
        echo 'upstream backend {' | sudo tee /etc/nginx/conf.d/load_balancer.conf
        echo '    server apache1:80;' | sudo tee -a /etc/nginx/conf.d/load_balancer.conf
        echo '    server apache2:80;' | sudo tee -a /etc/nginx/conf.d/load_balancer.conf
        echo '}' | sudo tee -a /etc/nginx/conf.d/load_balancer.conf
        echo 'server {' | sudo tee -a /etc/nginx/conf.d/load_balancer.conf
        echo '    listen 80;' | sudo tee -a /etc/nginx/conf.d/load_balancer.conf
        echo '    location / {' | sudo tee -a /etc/nginx/conf.d/load_balancer.conf
        echo '        proxy_pass http://backend;' | sudo tee -a /etc/nginx/conf.d/load_balancer.conf
        echo '    }' | sudo tee -a /etc/nginx/conf.d/load_balancer.conf
        echo '}' | sudo tee -a /etc/nginx/conf.d/load_balancer.conf
  
        sudo systemctl restart nginx
      SHELL
    end
  end
  