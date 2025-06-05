The following file is documentation of my web server found at http://zanemurdoch.ddns.net. The website is a photo marketplace site called Serendipity. 
Serendipity is mainly used as a platform to display images taken by photographers and also “sell” the photos to users by being able to purchase the content without watermarks.

Serendipity is hosted on Amazon EC2 and has a seperate DNS and hostname ran through noip.com that reroutes and redirects to the ip configured on Amazon EC2.

This readme file can also be used to functionally and easily replicate my web server within a timeframe of an hour or less following the instructions, documentation, instructions 
and code listed below:

As the first step to produce the web server I created an Amazon EC2 instance on the free plan of service, the management console of EC2 can be found at this link : 
http://aws.amazon.com/ec2/. From here navigate to EC2 services and then the instances tab on the left. After this point simply follow the on screen instructions of Amazon EC2 leaving most details to default
and select a free tier plan. At some point in its configuration you will be prompted to configure a security group, follow the on screen instructions and name it something easy 
such as "ssh-and-web", after this you will need to add the HTTP rule as this is the internet protocol that the server runs on. After this review and launch and this first step will
be completed. As the new root user of the web server you may want to create a key pair, give it a name that you will remember and save it somewhere that you won't lose easily such
as a usb drive or on the cloud as the server may become unretrievable to you if you lose it.

Once the specifications of the web server has been completed, launch the server and connect with the EC2 client. In the terminal that appears you will now need to install apache2 
which is the software that the server runs on, however before this run sudo apt update to ensure that the client is on the current repository. After this to install apache2 run the
command line sudo apt install apache2. This will retrieve and install all of the necessary information of the software that is required to run the server. Once this is finished
downloading run sudo nano /var/www/html/index.html in the command and this will open the index html file of the server which must be edited to the web server's specifications.
Initially this will be filled with an arbitary placeholder file for the apache2 system to verify that the server can run properly and this will need to be completely erased, 
simply delete all of the lines of this file and replace them with:


Once the html structure and elements have been replicated and implemented you will need to create the DNS from noip.com. This is a relatively easy and self explanatory process 
but nonetheless the procedure is as follows:

Simply press the create hostname button as you log in (or as otherwise prompted to) and follow the onscreen instructions to create the free Dynamic Domain Name System. 
Set the domain name to zanemurdoch.ddns.net as that is what the html link is appropriately named, use the default instructions for the steps presented in the creation of 
the DDNS, you may be asked to supply your home or current location internet router details (whichever one the server will be primarily made and run on) and then set the software 
to apache (the field will drop down and select it). At some point in the instructions of no-ip you will be given your DDNS key hostname and the username, record these similarly to
the key pair. After this you can skip port forwarding or utilise them if you feel necessary, use port 80 as default under all circumstances. Once this is done return to the 
amazon EC2 console and navigate to your instance details, under "Public IPV4 address you will find a string of numbers in the format similar to 3.106.75.55 (the currrent ipv4),
copy this number into the no-ip dns under the modify tab and this will connect them successfully. This will now enable users (and yourself) to enter http://zanemurdoch.ddns.net to
reach the web server instead of needing to know the long and random string of numbers that is an ip address. The final change to the server that will virtually complete all of its
technical setup is to create a new virtual host for the domain which can be done by returning to the console of the instance.

At this point enter sudo nano /etc/apache2/sites-available/zanemurdoch.conf and paste the following into the new blank file:

<VirtualHost *:80>
    ServerName zanemurdoch.ddns.net
    DocumentRoot /var/www/html

    <Directory /var/www/html>
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>

Once this is done enable the new site and disable the old default settings:

sudo a2ensite zanemurdoch.conf

sudo a2dissite 000-default.conf

and reload the page
sudo systemctl reload apache2

Once you visit this page the domain and web server will work exactly as intended and will be replicated with almost 100% similarity to the original instance.

Thanks for reading this guide.
Zane Robinson student number 35577763 of Murdoch University
Original root user of serendipity.net
