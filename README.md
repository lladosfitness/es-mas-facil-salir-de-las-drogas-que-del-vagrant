# instalacion de nextcloud en vargreant

Vargrant es una herramienta libre para trabajar en entorno de desarrolamiento, esta herramienta nos servira mucho para la instalacion de nextcvloud,
lo primero que hay que hacer es instalar esta herramienta, primero te creas una carpeta donde tendras todo tu trabajo:

```
[alumne@elpuig ~]$ mkdir ejemplo
[alumne@elpuig ~]$ cd ejemplo/
``` 
# Ahora con el vagrant subiremos una maquina virutal, por ejemplo la de ubuntu/jammy64 o cualquiera que este en el cloud de vargrant:

```
[alumne@elpuig example]$ vagrant init ubuntu/jammy64
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
[alumne@elpuig example]$ ll
total 4
-rw-rw-r--. 1 alumne alumne 3020 15 jul. 14:23 Vagrantfile
[alumne@elpuig example]$
```
# ahora toca darle infraestructura a nuestra maquina virtual:

```
[alumne@elpuig example]$ vagrant up --provider=virtualbox
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Running 'pre-boot' VM customizations...
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default: 
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default: 
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
    default: The guest additions on this VM do not match the installed version of
    default: VirtualBox! In most cases this is fine, but in rare cases it can
    default: prevent things such as shared folders from working properly. If you see
    default: shared folder errors, please make sure the guest additions within the
    default: virtual machine match the version of VirtualBox you have installed on
    default: your host and reload your VM.
    default: 
    default: Guest Additions Version: 6.0.0 r127566
    default: VirtualBox Version: 6.1
==> default: Mounting shared folders...
    default: /vagrant => /home/alumne/example

[alumne@elpuig example]$
```
# ahora haz ssh para poder manipularla

```
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-77-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu Jul 15 12:38:47 UTC 2021

  System load:  0.0               Processes:               110
  Usage of /:   3.2% of 38.71GB   Users logged in:         0
  Memory usage: 19%               IPv4 address for enp0s3: 10.0.2.15
  Swap usage:   0%


1 update can be applied immediately.
To see these additional updates run: apt list --upgradable


vagrant@ubuntu-jammy:~$
```
# ahora instalaremos apache2 que para eso necesitamos ser root:

```
vagrant@ubuntu-jammy:~$ sudo -s
root@ubuntu-jammy:/home/vagrant#
```
# ahora utilizaremos estos comandos:

# este es para instalar apache2:

```
apt install -y apache2
```

# Este es para instalar el mysql una base de datos:

```
apt install -y mysql-server
```

# Y este es para instalar algunas librerias php:

```
apt install -y php libapache2-mod-php
apt install -y php-fpm php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-intl php-mysql php-cli php-ldap php-zip php-curl
```

# Ahora vamos a confirgurar mysql para poder entrar a nextcloud:

Primero entramos:

```
root@ubuntu-jammy:/home/vagrant# mysql
```

# Ahora creamos una base de datos:

```
GRANT ALL ON bbdd.* to 'usuario'@'localhost';
```

# Ahora salimos:

```
exit
```

# Ahora comprobamos conexion:

```
root@ubuntu-jammy:/home/vagrant# mysql -u usuario -p
```

# Ahora descomprimimos la carpeta donde esta el nextcloud

```
unzip latest(1).zip
```

# Ahora lo movemos al /var/www/html/

```
mv /vagrant/tlatest(/1/).zip /var/www/html/
```

# Eliminamos con rm los demas archivos que esten

```
rm latest(1).zip, rm index.html
```

# Ahora para hacerlo visible tenemos que hacer esto

```
vi vagrantfile
```

```
# Create a forwarded port mapping which allows access to a specific port
# within the machine from a port on the host machine. In the example below,
# accessing "localhost:8080" will access port 80 on the guest machine.
# NOTE: This will enable public access to the opened port
config.vm.network "forwarded_port", guest: 80, host: 8080
```

```
# Create a public network, which generally matched to bridged network.
# Bridged networks make the machine appear as another physical device on
# your network.
config.vm.network "public_network"
```

# Despues reiniciamos, hacemos ssh, copiamos nuestra nueva ip y listo ya tendremos acceso a nextcloud

```
smx2b@turing-115:~/Documents/ejemplo1$ vagrant ssh
Welcome to Ubuntu 22.04.3 LTS (GNU/Linux 5.15.0-84-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Fri Oct  6 08:51:24 UTC 2023

  System load:  0.03173828125     Processes:               116
  Usage of /:   8.3% of 38.70GB   Users logged in:         0
  Memory usage: 62%               IPv4 address for enp0s3: 10.0.2.15
  Swap usage:   0%                IPv4 address for enp0s8: 192.168.18.123

```