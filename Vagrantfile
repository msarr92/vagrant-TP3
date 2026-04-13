Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/jammy64"

  # VM BACKEND
  config.vm.define "server-back" do |back|
    back.vm.hostname = "server-back"
    back.vm.network "private_network", ip: "192.168.56.10"
    back.vm.network "forwarded_port", guest: 8080, host: 8080
    back.vm.synced_folder "./backend", "/home/vagrant/backend"

    back.vm.provider "virtualbox" do |vb|
      vb.name = "server-back"
      vb.memory = 2048
      vb.cpus = 2
    end

    back.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y openjdk-17-jdk maven
      java -version
      mvn -version
    SHELL
  end

  # VM DATABASE
  config.vm.define "server-dba" do |dba|
    dba.vm.hostname = "server-dba"
    dba.vm.network "private_network", ip: "192.168.56.11"
    dba.vm.network "forwarded_port", guest: 3306, host: 3306

    dba.vm.provider "virtualbox" do |vb|
      vb.name = "server-dba"
      vb.memory = 2048
      vb.cpus = 2
    end

    dba.vm.provision "shell", inline: <<-SHELL
      export DEBIAN_FRONTEND=noninteractive
      apt-get update
      apt-get install -y mysql-server

      systemctl enable mysql
      systemctl start mysql

      mysql -e "CREATE DATABASE IF NOT EXISTS demo_db;"
      mysql -e "CREATE USER IF NOT EXISTS 'demo_user'@'%' IDENTIFIED BY 'demo_pass';"
      mysql -e "GRANT ALL PRIVILEGES ON demo_db.* TO 'demo_user'@'%';"
      mysql -e "FLUSH PRIVILEGES;"

      sed -i "s/^bind-address.*/bind-address = 0.0.0.0/" /etc/mysql/mysql.conf.d/mysqld.cnf || true
      systemctl restart mysql
    SHELL
  end

  # VM FRONTEND
  config.vm.define "server-front" do |front|
    front.vm.hostname = "server-front"
    front.vm.network "private_network", ip: "192.168.56.12"
    front.vm.network "forwarded_port", guest: 80, host: 3000
    front.vm.synced_folder "./frontend", "/home/vagrant/frontend"

    front.vm.provider "virtualbox" do |vb|
      vb.name = "server-front"
      vb.memory = 2048
      vb.cpus = 2
    end

    front.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y nginx curl
      curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
      apt-get install -y nodejs
      systemctl enable nginx
      systemctl start nginx
      node -v
      npm -v
    SHELL
  end

end