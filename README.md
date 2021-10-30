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

![image](https://user-images.githubusercontent.com/64368640/139527306-a1e98d36-87ce-469c-b2ca-ee141f3f6635.png)
![image](https://user-images.githubusercontent.com/64368640/139527317-56e72edb-0eac-4e82-b365-0d981b3bf7f8.png)


### 3. After that create a subdomain super.franky.yyy.com with the alias www.super.franky.yyy.com whose DNS is set at EniesLobby and points to Skypie

In eniesloby edit the franky.iup3.com with ```nano /etc/bind/kaizoku/franky.iup3.com``` and add

      super	          IN	    A	      10.39.2.4 ; IP Skypie
      www.super       IN      CNAME   franky.iup3.com. ;
 
 then restart using ```service bind9 restart```
 
 In loguetown, we ping and check using ```ping super.franky.iup3.com``` and ```host -t A super.franky.iup3.com```

![image](https://user-images.githubusercontent.com/64368640/139527350-5a146381-be3f-4498-8ae1-38bb00385a99.png)
![image](https://user-images.githubusercontent.com/64368640/139527367-aeb41615-e5e5-4900-92d1-a54d2145b146.png)


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

![image](https://user-images.githubusercontent.com/64368640/139527383-f0b8db31-7089-429a-b751-634ff829e9f9.png)


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

![image](https://user-images.githubusercontent.com/64368640/139527423-49518b1b-aa75-4a38-b082-1f5678a61d66.png)

in loguetown we edit the resolv conft using ```nano /etc/resolv.conf``` and add ```nameserver 10.39.2.2 nameserver 10.39.2.3```

then ping franky using ```ping franky.iup3.com```

![image](https://user-images.githubusercontent.com/64368640/139527604-3c8ffd92-8330-4a2f-b5d0-a2d3ace16e18.png)


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

![image](https://user-images.githubusercontent.com/64368640/139527586-8e514c36-589c-4897-96eb-b73d77423a11.png)


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

![image](https://user-images.githubusercontent.com/64368640/139527663-f59c4232-f392-4182-8bc2-03a7cb217084.png)


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


### 14. Luffy requested so that www.general.mecha.franky.yyy.com can only be accessed by using port 15000 and port 15500.

we first go to the `/etc/apache2/sites-available` directory, create a new file from copy of `000-default.conf` into `general.mecha.franky.iup3.com.conf`

We then change the setting on `general.mecha.franky.iup3.com.conf` into `<VirtualHost *:15000 *:15500>`, add or change lines to `ServerName general.mecha.franky.iup3.com`, `ServerAlias www.general.mecha.franky.iup3.com`, and `DocumentRoot /var/www/general.mecha.franky.iup3.com`.


on `/etc/apache2/ports.conf`, we also add additional port information by putting:

```bash
    Listen 15000
    Listen 15500
```

we also create a new directory named `general.mecha.franky.iup3.com` on `/var/www/`. We then extract the zip file of `general.mecha.franky` downloaded previously into the new folder `/var/www/general.mecha.franky.iup3.com`.

finally, we run the command `a2ensite general.mecha.franky.iup3.com` and also `service apache2 restart`

![image](https://user-images.githubusercontent.com/64368640/139527140-a8fd9bd6-c89c-4dc6-8744-9cc0c6f3a994.png)
![image](https://user-images.githubusercontent.com/64368640/139527067-800ef211-bf36-4c89-bcb2-5b6a2e7afbcd.png)


### 15. using authentication username luffy and password onepiece from file in /var/www/general.mecha.franky.yyy


We first run command `htpasswd -c /etc/apache2/.htpasswd luffy` which will save the username and password into the file `/etc/apache2/.htpasswd` that will save username `luffy`, and there will be a prompt to input the password.

We can then edit the file `/etc/apache2/sites-available/general.mecha.franky.iup3.com.conf` by adding:

```bash
    <Directory /var/www/general.mecha.franky.iup3.com>
        Options +FollowSymLinks -Multiviews
        AllowOverride All
    </Directory>
```

we also edit the file `/var/www/general.mecha.franky.iup3.com/.htaccess` into:

```bash
    AuthType Basic
    AuthName "Restricted Content"
    AuthUserFile /etc/apache2/.htpasswd
    Require valid-user
```

![image](https://user-images.githubusercontent.com/64368640/139526899-afa4f4a7-2b1e-4fc8-b66b-22d9772db2d9.png)
![image](https://user-images.githubusercontent.com/64368640/139527004-f9276633-b072-4beb-8a4b-75714fb44b83.png)


### no. 16 Everytime we access EniesLobby IP, we will be redirected into www.franky.yyy.com

We edit the file `/var/www/html/.htaccess` by adding:

```bash
    RewriteEngine On
    RewriteBase /
    RewriteCond %{HTTP_HOST} ^10\.39\.2\.4$
    RewriteRule ^(.*)$ http://www.franky.iup3.com [L,R=301]
```

We also edit the file `/etc/apache2/sites-available/000-default.conf` by adding:

```bash
    <Directory /var/www/html>
        Options +FollowSymLinks -Multiviews
        AllowOverride All
    </Directory>
```

![image](https://user-images.githubusercontent.com/64368640/139527221-7e8bb7f4-abed-41b6-aef0-4eb7f31cdf45.png)
![image](https://user-images.githubusercontent.com/64368640/139527215-4f9d0f53-e087-4ea8-b2ea-ab5b1b51ad63.png)


### no. 17 Because Franky also wants to invite his friends to be able to contact him through the website www.super.franky.yyy.com, and because web server visitors will definitely be confused by the random images that exist, Franky also request to replace the image request that has the substring "franky" will redirected to franky.png.

First, we edit file `/etc/apache2/sites-available/super.franky.iup3.com.conf` by adding:

```bash
    <Directory /var/www/super.franky.iup3.com>
        Options +FollowSymLinks -Multiviews
        AllowOverride All
    </Directory>
```

We also edit `/var/www/super.franky.iup3.com/.htaccess` by adding:

```bash
    RewriteEngine On
    RewriteRule ^(.*)franky(.*)\.(jpg|gif|png)$ http://super.franky.iup3.com/public/images/franky.png [L,R]
```

Problem: There seems to be a problem with the rewrite rule so when we tested it still not workin
![image](https://user-images.githubusercontent.com/64368640/139527273-2f710729-d53b-4a13-90da-3f7449dc82ae.png)
![image](https://user-images.githubusercontent.com/64368640/139527266-338ba57a-8b98-44f7-aab2-4a2586edaf53.png)







