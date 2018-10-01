#### Because we configured Kibana to listen on localhost, we can set up a reverse proxy to allow external access to it on port 80. We will use Nginx for this purpose.

##### Install Nginx and Apache2-utils:
```
sudo apt-get install nginx apache2-utils
```
##### Use htpasswd to create an admin user, called "sfnadmin" (or whatever you want), that can access the Kibana web interface:
```
sudo htpasswd -c /etc/nginx/htpasswd.users sfn
```
##### Enter a password at the prompt. Remember this login, as you will need it to access the Kibana web interface.

##### Edit the nginx server block file 
```
sudo vi /etc/nginx/sites-available/default   
```

##### Delete the file's contents, and paste the following code block into the file. Be sure to update the server_name to match your server's name and use whatever port you want it to listen it on:
```
server {
        listen 80;

        server_name sfn.com;

        auth_basic "Restricted Access";
        auth_basic_user_file /etc/nginx/htpasswd.users;

        location / {
            proxy_pass http://localhost:5601;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;        
        }
    }
```
##### Now restart Nginx so it listens on the port:
```
sudo service nginx restart
```

##### If you have run the setup.sh installer, Kibana should now be accessible via your FQDN or the public IP address of your ELK Server i.e. http://elk-server-public-ip/. If you go there in a web browser, after entering the "sfn" credentials, you should see a Kibana welcome page.


#### You are now done with the infrastructure setup. Head back, from whence you came, and read the rest of the [README](https://github.com/PaloAltoNetworks/safe-networking) to finish the install.  