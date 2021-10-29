# Jarkom-Modul-2-IUP3-2021

Kelompok IUP 3 


Rafi Akbar Rafsanjani (05111942000004)  


Drigo Alexander Sihombing (05111942000020)


Gilbert KurniawanH. (05111942000025)

8. After configuring the server, then the Webserver configuration is done. First with the webserver www.franky.yyy.com. First, luffy needs a webserver with DocumentRoot on /var/www/franky.yyy.com.

Skypie

We need to install ```apachew```, ```wget```, ```php```, ```unzip```, ```libapache2-mod-php7.0```. Then we download all files required by using ```wget``` command and edit ```nano /root/franky.iup3.com.conf```.

![prak2 no8](https://user-images.githubusercontent.com/74300479/139356118-c217ac1f-d3d7-430c-9a9d-b8ab72fec7d5.jpg)

Then we check at the loguetown ```lynx franky.iup3.com```.

![prak2 no8a](https://user-images.githubusercontent.com/74300479/139356331-e2e6320d-51f8-4534-8e66-6789352ffd8a.jpg)

9. After that, Luffy also needs that the url www.franky.yyy.com/index.php/home can be www.franky.yyy.com/home.

Run the ```a2enmod rewrite``` and ```service apache2 restart```. After that we edit the files ```nano /root/franky.iup3.com.conf```.

![prak2 no9](https://user-images.githubusercontent.com/74300479/139357839-f57c56df-70fb-48f6-8885-0ddd3d532cdc.jpg)

10. After that, on the subdomain www.super.franky.yyy.com, Luffy needs to store assets that have DocumentRoot on /var/www/super.franky.yyy.com.

Edit ```nano /root/franky.iup3.com.conf``` like this.

![prak2 no10](https://user-images.githubusercontent.com/74300479/139358503-1891c896-d44c-4ad5-9281-dda3f3aa2be5.jpg)

Then ```lynx www.super.franky.iup3.com```.

![prak2 no10a](https://user-images.githubusercontent.com/74300479/139358637-c4e0a249-0b5b-499d-88e3-fc124ab90814.jpg)

11. However, in the /public folder, Luffy wants to only be able to do directory listings.

12. Not only that, Luffy also prepared a 404.html error file in the /error folder to replace the error code in apache.

Edit ```lynx www.super.franky.iup3.com/asd```. It will show the error.

![prak2 no12](https://user-images.githubusercontent.com/74300479/139359031-1950b7a1-6203-4e67-a0a8-6fcbcdb5d6d9.jpg)

13. Luffy also asked Nami to make a virtual host configuration. This virtual host aims to be able to access the asset file www.super.franky.yyy.com/public/js to www.super.franky.yyy.com/js.

Edit ```lynx www.super.franky.iup3.com/js```. Then it will show the index of js.

![prak2 no13](https://user-images.githubusercontent.com/74300479/139359626-bff58826-ba8c-43de-adef-b7361c1e8ce2.jpg)









