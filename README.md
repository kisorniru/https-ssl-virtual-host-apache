<p align="center">
  <a href="https://nasiddique.com">
  	<img src="https-ssl-virtual-host-apache.jpeg">
  </a>
</p>

We, most of the time face https problem in out local domain in local virtualhost. This readme file will help you to generate ssl certificate in your local machine and it's fee of cost.

### Target
* http://localhost-1.local >= https://localhost-1.local
* http://localhost-2.local >= https://localhost-2.local

### Requirements
* ubuntu version >= 16.04
* php version >= 7.0.*

### Installation

I hope that you already have PHP / apache / and others (except ssl) setup in your machine.
* Create a folder in your favorate location first, lets say `/home/your-pc-name/ssl`
* Make folder read/write/executable `sudo chmod -R 777 /home/your-pc-name/ssl`
* Now, come to the fun part 

```sh
$ sudo openssl req -x509 -days 365 -newkey rsa:2048 -keyout /home/your-pc-name/ssl/localhost-1.key -out /home/your-pc-name/ssl/localhost-1.crt
  Enter PEM pass phrase: 123456*put your password
  Verifying - Enter PEM pass phrase: 123456*reenter your password
  Country Name []: BD*change
  State Name []: Dhaka*change
  Locality Name []: Bangladeshi*change
  Organization Name []: company-name*change
  Common Name []: localhost-1.local*make it your file name
  Email Address []: admin@localhost-1.com*change
```

* Now open your virtualhost
```sh
$ sudo gedit /etc/apache2/sites-available/localhost-1.conf
```
*  And edit
```sh
<VirtualHost *:443>

	ServerName localhost-1.local
	ServerAdmin admin@localhost-1.com
	
	ServerAlias www.localhost-1.local
	DocumentRoot /var/www/html/localhost-1.local

	SSLEngine on
	SSLCertificateFile "/home/your-pc-name/ssl/localhost-1.crt"
	SSLCertificateKeyFile "/home/your-pc-name/ssl/localhost-1.key"

	<Directory /var/www/html/localhost-1.local/>
		Options Indexes FollowSymLinks MultiViews
		AllowOverride All
		Order allow,deny
		allow from all
	</Directory>

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```
* Enable ssl for your virtualhost
```sh
$ sudo a2enmod ssl
$ sudo service apache2 restart
  Enter PEM pass phrase: 123456*type your password
$ sudo systemctl status apache2.service
  Enter PEM pass phrase: 123456*type your password
```

### After Installation

* Make it for `localhost-2.local` and for rest of the virtual host [what you have]
* make it for every virtualhost you have otherwise you may redirect
* Now, browse `https://localhost-1.com`

### Owned By
* [Siddique Md. Noor-A-Alam](https://www.nasiddique.com)

### Developed By
* [Siddique Md. Noor-A-Alam](https://www.nasiddique.com)