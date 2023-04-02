# basic-digest-auth-http
A repository that implements the main authentication methods in HTTP.

## Description:
This repository shows the configuration of two Virtual Hosts using the Apache2 webserver, on a Linux environment (Ubuntu).

The first virtualhost is called *www.basic.local* and it provides **Basic HTTP authentication** (username: *myuser*, password: *mypass*). 

The second virtualhost is called *www.digest.local* and it provides **Digest HTTP authentication** (username: *myuser2*, password: *mypass2*). 

## Instructions:

### **1. Set up local hosts**

Change the */etc/hosts* file by adding these two lines:

    127.0.0.1 www.basic.local
    127.0.0.1 www.digest.local

This will allow the browser to redirect requests towards these domain names to localhost.

### **2. Create websites folders**
Create two folders, *basic* and *digest* in */var/www/* which will be the websites' directories. Write an *index.html* file in both directories (this will be the page that the browser sees once it connects to the one of the websites).

### **3. Enable virtual hosts**
Enable the virtual hosts by copying the */etc/apache2/sites-available/000-default.conf* file. The provided file will add two virtual hosts: *www.basic.local* and *www.digest.local*, pointing to the folders created before.

### **4. Change apache configuration file**
Write these lines in the */etc/apache2/apache2.conf* file:
    
    <Directory /var/www/basic>
        Options Indexes FollowSymLinks
        AllowOverride AuthConfig
        Require all granted
    </Directory>
    <Directory /var/www/digest>
        Options Indexes FollowSymLinks
        AllowOverride AuthConfig
        Require all granted
    </Directory> 
The **AllowOverride** directives, when set to **AuthConfig**,  will turn on *.htaccess* on both server directories.

### **5. Create the .htaccess files**
Copy the *.htaccess* files in the */var/www/basic/* and */var/www/digest* folders.

### **6. Create the password files**
Run the commands

    $sudo htpasswd -c /etc/apache2/.htpasswd myuser
and insert the password for the user (in the example it's **mypass**)

    $sudo htdigest -c /etc/apache2/.htdigest "Secure Content" myuser2
and insert the password for the user (in the example it's **mypass2**)

These will create the *.htpasswd* and the *.htdigest* files which will contain the credentials of the users registered to the basic and digest websites respectively.

### **7. Restart Apache2**
Run the command:

    $sudo service apache2 restart
