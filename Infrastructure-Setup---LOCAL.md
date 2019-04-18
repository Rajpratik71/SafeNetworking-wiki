#### Use the following instructions to download, install, configure and deploy the required versions of software for SafeNetworking
<br/>

## NOTE:  As of SFN3.4, these instructions are intended for Ubuntu 18.04.x LTS.  These differ from the 16.04 instructions and YMMV

# SOFTWARE REQUIREMENTS
**ElasticStack 6.7.x**  <br/>
**Nginx 1.10** - minimum<br/>
**Java 8**<br/>
**Python 3.6 or greater** - don't even *attempt* anything else<br/>
**Ubuntu 18.04 (Desktop or Server)** - no, it won't run on 14.04<br/>
<br/>

# SYSTEM SETUP 
</br>

#### Create host mapping for elasticsearch in /etc/hosts
Add the following line to you /etc/hosts file using the editor of your choice: <br/>
```127.0.0.1  elasticsearch```

#### Install supporting tools and pkgs for Ubuntu
```
sudo add-apt-repository "deb http://archive.ubuntu.com/ubuntu $(lsb_release -sc) universe"
sudo apt-get update && sudo apt-get install -y software-properties-common curl 
```

#### Get the release key for the ElasticStack software
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch |sudo apt-key add -
```

#### Create the ElasticStack repository listing for apt-get
```
echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list
```
</br>


## Python 3.6 Libraries
SafeNetworking requires Python 3.6 to run properly. The interpreter is on Ubuntu 18.04.x but some of these supporting libraries may not be.<br/>
```
sudo apt-get update && sudo apt-get -y install build-essential python-dev python-setuptools libncursesw5-dev libgdbm-dev libc6-dev zlib1g-dev libsqlite3-dev libssl-dev openssl libffi-dev python3-pip  python3.6-venv
```

## JAVA 8
The ElasticStack depends on Java to run, so we need to make sure that we have the Java 8 JDK installed before we install the stack.  
```
java -version
```
On systems with Java 8 installed, this command produces output similar to the following:
```java
openjdk version "1.8.0_162"
OpenJDK Runtime Environment (build 1.8.0_162-8u162-b12-1-b12)
OpenJDK 64-Bit Server VM (build 25.162-b12, mixed mode)
```
***If*** Java needs to be installed or upgraded to Java 8 (Java 9 is NOT supported and 10 breaks logstash)
```
sudo apt-get update
sudo apt-get install openjdk-8-jre-headless -y
sudo apt-get install openjdk-8-jdk-headless -y
```
*Rerun the java -version command to verify you now have Java 8 installed*
</br>
</br>

## Install Elasticsearch, Logstash & Kibana
```
sudo apt-get update && sudo apt-get install elasticsearch=6.7.0 kibana=6.7.0 logstash=1:6.7.0-1
```

## Set apt to hold the versions so it doesn't inadvertently update
```
sudo apt-mark hold elasticsearch
sudo apt-mark hold kibana
sudo apt-mark hold logstash
```

</br>
</br>


## Install NGINX
#### Because we configured Kibana to listen on localhost, we can set up a reverse proxy to allow external access to it on port 80. We will use Nginx for this purpose.

##### Install Nginx and Apache2-utils:
```
sudo apt-get install nginx apache2-utils
```
##### Use htpasswd to create an admin user, called "sfn" (or whatever you want), that can access the Kibana web interface:
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

##  Run SETUP
####  The setup utility uses the pan user and pan group as the owner on the Ubuntu server as that is how we build servers internally.  If you do not wish to use the pan:pan setting, on the first uncommented line in setup.sh, you will need to change the two instances of pan to your user and group for this to work properly.
```
cd install
<edit setup.sh if you wish to change the user and group>
sudo ./setup.sh
```

##### After you have run the setup.sh installer (with no errors), Kibana should now be accessible via your FQDN or the public IP address of your ELK Server i.e. http://elk-server-public-ip/. If you go there in a web browser, after entering the "sfn" credentials, you should see a Kibana welcome page.


#### You are now done with the infrastructure setup. Head back, from whence you came, and read the rest of the [README](https://github.com/PaloAltoNetworks/safe-networking) to finish the install.  

