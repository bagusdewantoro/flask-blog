How I deploy this repo in Linode ubuntu server 20.04 LTS:

1. Setup linode server

2. Download and install apache:
- sudo apt update
- sudo apt install apache2
- apache2 -version

3. Configure Firewall
- sudo ufw app list
- sudo ufw allow ‘Apache’

4. Configure apache
- sudo systemctl status apache2  

5. Install and enable mod_wsgi
- sudo apt install apache2-dev libapache2-mod-wsgi-py3
- sudo pip3 install mod_wsgi

6. Creating flask app
- cd /var/www 
- sudo mkdir webApp
- cd webApp

7. Install flask
- sudo apt-get install python3-pip
- sudo pip3 install Flask 
- sudo pip3 install flask_sqlalchemy

8. Use git clone to clone this repo

9. Configure and enable virtual host
- sudo nano /etc/apache2/sites-available/webApp.conf
- code:
<VirtualHost *:80>
		ServerName "ip address"
		ServerAdmin email@mywebsite.com
		WSGIScriptAlias / /var/www/webApp/webApp.wsgi
		<Directory /var/www/webApp/webApp/>
			Order allow,deny
			Allow from all
		</Directory>
		Alias /static /var/www/webApp/webApp
		<Directory /var/www/webApp/webApp/>
			Order allow,deny
			Allow from all
		</Directory>
		ErrorLog ${APACHE_LOG_DIR}/error.log
		LogLevel warn
		CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

- sudo a2ensite webApp 
- systemctl reload apache2

10. Create .wsgi file
- sudo nano webApp.wsgi 
- code:
#!/usr/bin/python3
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/webApp/")

from webApp import app as application
application.secret_key = 'hard_to_guess'

11. Restart apache
- sudo service apache2 restart 


Credits:
- techwithtim youtube channel
- digitalocean about configuring wsgi
- linode documentation about configuring apache
