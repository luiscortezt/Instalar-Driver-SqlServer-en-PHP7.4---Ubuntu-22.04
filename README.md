# Instalar-Driver-SqlServer-en-PHP7.4---Ubuntu-22.04

sudo su
1  apt update
2  apt upgrade
3  apt update
4  apt upgrade
5  apt install apache2
6  apt update
7  apt install software-properties-common
8  add-apt-repository ppa:ondrej/php
9  apt install php7.4 php7.4-cli php7.4-json php7.4-common php7.4-mysql php7.4-zip php7.4-gd php7.4-mbstring php7.4-curl php7.4-xml php-pear php7.4-bcmath php7.4-dev --allow-unauthenticated
10  curl https://packages.microsoft.com/keys/microsoft.asc | sudo tee /etc/apt/trusted.gpg.d/microsoft.asc
11  curl https://packages.microsoft.com/config/ubuntu/$(lsb_release -rs)/prod.list | sudo tee /etc/apt/sources.list.d/mssql-release.list
12  apt-get update
13  ACCEPT_EULA=Y apt-get install -y msodbcsql17
14  apt-get install mssql-tools unixodbc-dev
15  echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
16  echo 'export PATH="$PATH:/opt/mssql-tools17/bin"' >> ~/.bash_profile
17  source ~/.bashrc
18  apt update
19  apt upgrade
20  echo 'export PATH="$PATH:/opt/mssql-tools17/bin"' >> ~/.bash_profile
21  pecl channel-update pecl.php.net
22  pecl install sqlsrv-5.9.0
23  pecl install pdo_sqlsrv-5.9.0
24  printf "; priority=20\nextension=sqlsrv.so\n" > /etc/php/7.4/mods-available/sqlsrv.ini
25  printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /etc/php/7.4/mods-available/pdo_sqlsrv.ini
26  phpenmod  sqlsrv pdo_sqlsrv
27  systemctl restart apache2
28  php -m | grep sqlsrv
29  nano /etc/ssl/openssl.cnf

####### edit: /etc/ssl/openssl.cnf

####### In the first line of the file add
openssl_conf = default_conf

####### End of file append

[default_conf]
ssl_conf = ssl_sect

[ssl_sect]
system_default = system_default_sect

[system_default_sect]
MinProtocol = TLSv1
CipherString = DEFAULT@SECLEVEL=0

####### Not 100% sure why i had to restart apache2 for it to take effect, but I had to.
30  systemctl restart apache2
31  cd /var/www/html/
32  nano con.php
    <?php
        $serverName = '127.0.0.1';
        $uid = 'user';
        $pwd = 'password';
        $databaseName = 'database';
        $connectionInfo = array( 'UID'=>$uid,'PWD'=>$pwd,'Database'=>$databaseName);
        $conn = sqlsrv_connect($serverName,$connectionInfo);   
        if($conn){
            echo 'Conectado' . PHP_EOL;
        } else {
            echo 'Connection failure' . PHP_EOL;
            die(print_r(sqlsrv_errors(), TRUE));
        }
    ?>
33  ping 127.0.0.1
34  php con.php 
