#### Use the following instructions to use the docker based infrastructure for SafeNetworking
<br/>

# SOFTWARE REQUIREMENTS
**ElasticStack 6.3**  <br/>
**Python 3.6 or greater** <br/>
**Ubuntu 16.04 (Desktop or Server)** <br/>
**Nginx 1.10** - optional<br/>


NOTE: Currently, only the underlying ELK stack is setup using containers. In the future, SafeNetworking will also be containerized, but is not as of today. 
# 
<br/>

### SYSTEM SETUP 
***
Install supporting tools and pkgs for Ubuntu
```
sudo apt-get update && sudo apt-get -y install software-properties-common python-software-properties
sudo add-apt-repository ppa:git-core/ppa
sudo apt-get -y install apt-transport-https sysv-rc-conf curl git
```
# 
<br/>

### DOCKER
***
Get the release key for docker
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Create the docker repository listing for apt-get
```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

Make sure you are going to install from the Docker CE repo and not the default Ubuntu 
```
apt-cache policy docker-ce
```

*You should see output similar to the following:*
```python
     docker-ce:
      Installed: (none)
      Candidate: 17.03.1~ce-0~ubuntu-xenial
      Version table:
         17.03.1~ce-0~ubuntu-xenial 500
            500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
         17.03.0~ce-0~ubuntu-xenial 500
            500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
```

Install docker
```
sudo apt-get install -y docker-ce
```

Docker should now be installed, the daemon started, and the process enabled to start on boot. Check that it's running:
```
sudo systemctl status docker
```

*The output should be similar to the following, showing that the service is active and running:*
```
 Output
    ● docker.service - Docker Application Container Engine
       Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
       Active: active (running) since Sun 2016-05-01 06:53:52 CDT; 1 weeks 3 days ago
         Docs: https://docs.docker.com
     Main PID: 749 (docker)
```


# 
<br/>

### DOCKER COMPOSE
***
Install Docker Compose from the Docker's GitHub repository
```
sudo curl -L https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```
Set the permissions
```
sudo chmod +x /usr/local/bin/docker-compose
```
# 
<br/>

### PYTHON 3.6
***
SafeNetworking requires Python 3.6 to run properly. 
<br/>

NOTE: Most of these should already be installed.  This ensures it.  
```
sudo add-apt-repository ppa:jonathonf/python-3.6
sudo apt-get update && sudo apt-get -y install build-essential checkinstall 
sudo apt-get -y install python-dev python-setuptools python-pip python-smbus libncursesw5-dev libgdbm-dev libc6-dev zlib1g-dev libsqlite3-dev tk-dev libssl-dev openssl libffi-dev 
sudo apt-get -y install python3.6
```
# 

</br></br>
### THE ELASTICSTACK
***
```
cd infra
docker-compose up
```
#### Elasticsearch
Verify Elasticsearch is running by issuing this at the command prompt
```
curl 127.0.0.1:9200
```
You should get JSON similar to the following:
```json
    {
      "name" : "79dHjEG",
      "cluster_name" : "elasticsearch",
      "cluster_uuid" : "S09jzgZgTOa3ZO-xJVXFbA",
      "version" : {
        "number" : "6.3.0",
        "build_hash" : "10b1edd",
        "build_date" : "2018-05-16T19:01:30.685723Z",
        "build_snapshot" : false,
        "lucene_version" : "7.2.1",
        "minimum_wire_compatibility_version" : "5.6.0",
        "minimum_index_compatibility_version" : "5.0.0"
      },
      "tagline" : "You Know, for Search"
}
```
*If you see JSON similar to the above, Elasticsearch is now up and running*

#### Kibana
Verify Kibana is running by opening this link on the server: 
```
http://localhost:5601
```
*If you see the Kibana UI, Kibana is now up and running*
</br>
</br>
#### Logstash
Verify Logstash is running by running the following: 
```
telnet localhost 5514
```
*If you see the something similar to the following, Logstash is running and listening on ALL interfaces*
```
    Trying 127.0.0.1...
    Connected to localhost.
    Escape character is '^]'.
```
NOTE: To get out of the telnet session above, Ctrl + ] and type exit at the prompt - in case that wasn't clear.

### OPTIONAL NGINX SETUP
Using the default setup, Kibana only listens on port 5601 (localhost).  To allow for external connections on port 80, follow the [SFN NGINX SETUP GUIDE](https://github.com/PaloAltoNetworks/safe-networking/wiki/NGINX-Setup) that also provides a username/password setup to access the system. 