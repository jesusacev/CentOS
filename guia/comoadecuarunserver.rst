Como adecuar un server en Centos
=====================================

- Debemos modificar el hostname del servidor en el archivo hostname ubicado en /etc. La nomenclatura es nombre.dominio. 
  Ejemplo: centosgit.local

- Luego se debe configurar las interfaces de red. Para ello nos movemos a la ruta /etc/sysconfig/network-scripts y editamos las interfaces que necesitemos::

	# vi ifcfg-enp0s3

	TYPE=Ethernet
	PROXY_METHOD=none
	BROWSER_ONLY=no
	BOOTPROTO=dhcp
	DEFROUTE=yes
	IPV4_FAILURE_FATAL=no
	IPV6INIT=yes
	IPV6_AUTOCONF=yes
	IPV6_DEFROUTE=yes
	IPV6_FAILURE_FATAL=no
	IPV6_ADDR_GEN_MODE=stable-privacy
	NAME=enp0s3
	UUID=0134c7d5-1f34-43c7-badc-0a4175a70429
	DEVICE=enp0s3
	ONBOOT=yes

Aquí dependerá de la configuración de su gusto, ya que se puede configurar asignación de ip fija o por DHCP. En nuestro caso solo modificamos del archivo por defecto, el prámetro ONBOOT=yes, para que cada vez que inicie el server, levante la interfaz.

- Seguidamente reiniciamos el servicio de network para que tome los cambios realizados::

	# systemctl restart network.service

- Al colocar el comando ipaddr ya nos debe mostrar la ip de la interfaz::

	
	# ip addr
	1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    	link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    	inet 127.0.0.1/8 scope host lo
       	valid_lft forever preferred_lft forever
    	inet6 ::1/128 scope host 
       	valid_lft forever preferred_lft forever
	2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group 		default qlen 1000
    	link/ether 08:00:27:ea:1d:21 brd ff:ff:ff:ff:ff:ff
    	inet 192.168.0.188/23 brd 192.168.1.255 scope global noprefixroute dynamic enp0s3
       	valid_lft 691134sec preferred_lft 691134sec
    	inet6 fe80::ff29:c6da:8984:609a/64 scope link noprefixroute 
       	valid_lft forever preferred_lft forever

- Instalamos el paquete **net-tools** para tener las funcionalidades de ifcongif y route::

	# ifconfig
	enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.0.188  netmask 255.255.254.0  broadcast 192.168.1.255
        inet6 fe80::ff29:c6da:8984:609a  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:ea:1d:21  txqueuelen 1000  (Ethernet)
        RX packets 83098  bytes 24951960 (23.7 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 10936  bytes 969247 (946.5 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

	lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 64  bytes 5376 (5.2 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 64  bytes 5376 (5.2 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

	# route -n
	Kernel IP routing table
	Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
	0.0.0.0         192.168.1.5     0.0.0.0         UG    100    0        0 enp0s3
	192.168.0.0     0.0.0.0         255.255.254.0   U     100    0        0 enp0s3

- Luego instalamos el repositorio de epel, que es un repositorio de paquetes rpm de la comunidad::

	# yum install epel-release.noarch

- Por último realizamos una actualización de los paquetes que están de manera local::

	# yum update






