# Linux-Server-Configuration

IP Address: 54.172.195.119
Port: 2200

Publicly available URL: http://ec2-54-172-195-119.compute-1.amazonaws.com/

Steps to login to the server as the grader:

ssh grader@54.172.195.119 -p 2200 -i graderKey


Software installed and config changes made:

Updating all packages on the server

-> sudo apt-get update
-> sudo apt-get upgrade


Update timezone

-> sudo dpkg-reconfigure tzdata
-> sudo apt-get install ntp


Install finger

-> sudo apt-get install finger


Configure sshd-config file to change port from 22 to 2200

-> sudo nano /etc/ssh/sshd_config

-> Change port from 22 to 2200

-> Change PermitRootLogin from prohibited-password to no

-> sudo service ssh restart

-> Add 2200 to the available port in lightsail


Configure UFW

-> sudo ufw status 

-> sudo ufw default deny incoming

-> sudo ufw default allow outgoing

-> sudo ufw allow 2200/tcp

-> sudo ufw allow 80/tcp

-> sudo ufw allow 123/udp

-> sudo ufw enable


Install apache and mod_wdgi

-> sudo apt-get install apache2

-> sudo a2enmod wsgi

-> sudo service apache2 start


Install git 

-> sudo apt-get install git


Clone Item Catalog project

-> cd /var/www

-> sudo mkdir catalog

-> cd catalog

-> git clone https://github.com/kennethsilvere/Item-Catalog-on-Server.git catalog

-> create catalog.wsgi file


Install dependencies for Item-Catalog project

-> sudo apt-get install python-psycopg2

-> sudo apt-get install python-flask

-> sudo apt-get install python-sqlalchemy

-> sudo apt-get install python-pip

-> sudo pip install oauth2client

-> sudo pip install requests

-> sudo pip install htpplib2


Open project files and changes all files from using SQLite to PostgreSQL


Install and setup PostgreSQL


-> sudo apt-get install postgresql

-> sudo su - postgres

-> psql

-> create user vagrant with password vagrant;

-> alter user catalog createdb;

-> create database restrauntmenuwithusers WITH OWNER vagrant;

-> revoke all on schema public from public;

-> grant all on schema public to vagrant;

-> \q

-> exit

-> python database_setup.py

-> python lotsofmenus.py

-> python init.py
