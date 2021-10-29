# Jarkom-Modul-2-IUP3-2021

Kelompok IUP 3 


Rafi Akbar Rafsanjani (05111942000004)  


Drigo Alexander Sihombing (05111942000020)


Gilbert KurniawanH. (05111942000025)


### 2. Luffy wants to contact Franky who is in EniesLobby with denden mushi. You are asked by Luffy to create the main website by accessing franky.yyy.com with the alias www.franky.yyy.com in the kaizoku folder.

In Enieslobby we input name server to /etc/resolv.conf using ```echo nameserver 192.168.122.1 > /etc/resolv.conf``` and then use command ```apt-get update``` and ```apt-get install bind9 -y```

edit the conf using command ```nano /etc/bind/named.conf.local```

and input the configuration with this syntax : ```zone "franky.iup3.com" {
	type master;
	file "/etc/bind/kaizoku/franky.iup3.com";
};```

and then create the folder in /etc/bin with ```mkdir /etc/bind/kaizoku```

copy the db.local in /etc/bind to new folder that we create
```cp /etc/bind/db.local /etc/bind/kaizoku/franky.iup3.com```

after that, we open the franky.iup3.com using nano ```nano /etc/bind/kaizoku/franky.iup3.com```

    $TTL    604800
      @       IN      SOA     franky.iup3.com. root.franky.iup3.com. (
                     2021102501         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
      ;
      @       IN      NS      franky.iup3.com.
      @       IN      A       10.39.2.2;
      www     IN      CNAME   franky.iup3.com.;
      
after that , restart the bind ```service bind9 restart```

in loguetown, use the command ```echo nameserver 192.168.122.1 > /etc/resolv.conf``` ```apt-get update``` ```apt-get install bind9 -y``` ```apt-get install dnsutils``` then open and edit the resolv conf ```nano /etc/resolv.conf``` and add the ```nameserver 10.39.2.2```

then we ping the franky and check the host with  ```ping franky.iup3.com``` ```host -t CNAME www.franky.iup3.com```

### 3. After that create a subdomain super.franky.yyy.com with the alias www.super.franky.yyy.com whose DNS is set at EniesLobby and points to Skypie

In eniesloby edit the franky.iup3.com with ```nano /etc/bind/kaizoku/franky.iup3.com``` and add

      super	          IN	    A	      10.39.2.4 ; IP Skypie
      www.super       IN      CNAME   franky.iup3.com. ;
 
 then restart using ```service bind9 restart```
 
 In loguetown, we ping and check using ```ping super.franky.iup3.com``` and ```host -t A super.franky.iup3.com```
 
### 4. Also create a reverse domain for the main domain

in eniesloby we edit the conf local using ```nano /etc/bind/named.conf.local``` then add


     zone "2.39.10.in-addr.arpa" {
          type master;
          file "/etc/bind/kaizoku/2.39.10.in-addr.arpa";
      };

then copy the db.local into the file using ```cp /etc/bind/db.local /etc/bind/kaizoku/2.39.10.in-addr.arpa```

edit the file using ```nano /etc/bind/kaizoku/2.39.10.in-addr.arpa```

      $TTL    604800
      @       IN      SOA     franky.iup3.com. root.franky.iup3.com. (
                              2021102501      ; Serial
                               604800         ; Refresh
                                86400         ; Retry
                              2419200         ; Expire
                               604800 )       ; Negative Cache TTL
      ;
      2.39.10.in-addr.arpa. IN      NS      franky.iup3.com.
      2                       IN      PTR     franky.iup3.com.


then restart using ```service bind9 restart```

in loguetown edit the resolv conft using ```nano /etc/resolv.conf``` and add the ```nameserver ```

check using ```host -t PTR 10.39.2.2```


### 5. In order to still be able to contact Franky if the Enies Lobby server is damaged, then make Water7 the DNS Slave for the main domain

in enies lobby we edit the conf local using ```nano /etc/bind/named.conf.local``` and edit the zone become

      zone "franky.iup3.com" {
          type master;
          notify yes;
          also-notify { 10.39.2.3; }; 
          allow-transfer { 10.39.2.3; }; 
          file "/etc/bind/kaizoku/franky.iup3.com";
      };

in water 7 
```echo nameserver 192.168.122.1 > /etc/resolv.conf``` , update and install bind9 using ```apt-get update``` , ```apt-get install bind9 -y```

edit the conf local using ```nano /etc/bind/named.conf.local``` and add 
      
      zone "franky.iup3.com" {
          type slave;
          masters { 10.39.2.2; }; 
          file "/var/lib/bind/franky.iup3.com";
      };

then restart using  ```service bind9 restart```

in enies lobby , we stop using ```service bind9 stop```

in loguetown we edit the resolv conft using ```nano /etc/resolv.conf``` and add ```nameserver 10.39.2.2 nameserver 10.39.2.3```

then ping franky using ```ping franky.iup3.com```

###  6. After that there is a subdomain mecha.franky.yyy.com with the alias www.mecha.franky.yyy.com which was delegated from EniesLobby to Water7 with the IP going to Skypie in the sunnygo folder

in enies lobby 
edit the file franky using ```nano /etc/bind/kaizoku/franky.iup3.com``` and add

      mecha	IN	A	10.39.2.4 ;
      ns1	  IN	A	10.39.2.3 ; 
      mecha	IN	NS	ns1 ;

then edit the conf options using ```nano /etc/bind/named.conf.options``` and add ```allow-query{any;};```

edit the conf local using ```nano /etc/bind/named.conf.local```

     zone "mecha.franky.iup3.com" {
          type master;
          file "/etc/bind/sunnygo/mecha.franky.iup3.com";
      };

make the directory sunnygo using ```mkdir /etc/bind/sunnygo```

copy the db.local into sunnygo mecha using ```cp /etc/bind/db.local /etc/bind/sunnygo/mecha.franky.iup3.com```

###  7. To facilitate communication between Luffy and his colleagues, a subdomain was created through Water7 with the name general.mecha.franky.yyy.com with the alias www.general.mecha.franky.yyy.com which points to Skypie

edit the file mecha franky using ```nano /etc/bind/sunnygo/mecha.franky.iup3.com```

      $TTL    604800
      @       IN      SOA     mecha.franky.iup3.com. root.mecha.franky.iup3.com. (
                            2021102501     	; Serial
                             604800         ; Refresh
                              86400         ; Retry
                            2419200         ; Expire
                             604800 )       ; Negative Cache TTL
      ;
      @             IN      NS      mecha.franky.iup3.com.
      @             IN      A       10.39.2.4	; IP Skypie
      general       IN      A       10.39.2.4	; IP Skypie

restart using ```service bind9 restart```

in loguetown 

      echo nameserver 192.168.122.1 > /etc/resolv.conf
      apt-get update
      apt-get install bind9 -y
      
we concatenate the local using ```cat named.conf.local >> /etc/bind/named.conf.local```

make directory kaizoku ```mkdir /etc/bind/kaizoku```

then copy into the directory
 
      cp franky.iup3.com /etc/bind/kaizoku/franky.iup3.com
      cp 2.39.10.in-addr.arpa /etc/bind/kaizoku/2.39.10.in-addr.arpa

concatenate the conf options ```cat named.conf.options > /etc/bind/named.conf.options```
then restart using ```service bind9 restart```

### 8. After configuring the server, then the Webserver configuration is done. First with the webserver www.franky.yyy.com. First, luffy needs a webserver with DocumentRoot on /var/www/franky.yyy.com.

Skypie

We need to install ```apachew```, ```wget```, ```php```, ```unzip```, ```libapache2-mod-php7.0```. Then we download all files required by using ```wget``` command and edit ```nano /root/franky.iup3.com.conf```.

![prak2 no8](https://user-images.githubusercontent.com/74300479/139356118-c217ac1f-d3d7-430c-9a9d-b8ab72fec7d5.jpg)

Then we check at the loguetown ```lynx franky.iup3.com```.

![prak2 no8a](https://user-images.githubusercontent.com/74300479/139356331-e2e6320d-51f8-4534-8e66-6789352ffd8a.jpg)

### 9. After that, Luffy also needs that the url www.franky.yyy.com/index.php/home can be www.franky.yyy.com/home.

Run the ```a2enmod rewrite``` and ```service apache2 restart```. After that we edit the files ```nano /root/franky.iup3.com.conf```.

![prak2 no9](https://user-images.githubusercontent.com/74300479/139357839-f57c56df-70fb-48f6-8885-0ddd3d532cdc.jpg)

### 10. After that, on the subdomain www.super.franky.yyy.com, Luffy needs to store assets that have DocumentRoot on /var/www/super.franky.yyy.com.

Edit ```nano /root/franky.iup3.com.conf``` like this.

![prak2 no10](https://user-images.githubusercontent.com/74300479/139358503-1891c896-d44c-4ad5-9281-dda3f3aa2be5.jpg)

Then ```lynx www.super.franky.iup3.com```.

![prak2 no10a](https://user-images.githubusercontent.com/74300479/139358637-c4e0a249-0b5b-499d-88e3-fc124ab90814.jpg)

### 11. However, in the /public folder, Luffy wants to only be able to do directory listings.

### 12. Not only that, Luffy also prepared a 404.html error file in the /error folder to replace the error code in apache.

Edit ```lynx www.super.franky.iup3.com/asd```. It will show the error.

![prak2 no12](https://user-images.githubusercontent.com/74300479/139359031-1950b7a1-6203-4e67-a0a8-6fcbcdb5d6d9.jpg)

### 13. Luffy also asked Nami to make a virtual host configuration. This virtual host aims to be able to access the asset file www.super.franky.yyy.com/public/js to www.super.franky.yyy.com/js.

Edit ```lynx www.super.franky.iup3.com/js```. Then it will show the index of js.

![prak2 no13](https://user-images.githubusercontent.com/74300479/139359626-bff58826-ba8c-43de-adef-b7361c1e8ce2.jpg)









