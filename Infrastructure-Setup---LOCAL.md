#### Use the following instructions to download, install, configure and deploy the required versions of software for SafeNetworking
<br/>

## NOTE:  As of SFN3.4, these instructions are intended for Ubuntu 18.04.x LTS.  These differ from the 16.04 instructions and YMMV

# SOFTWARE REQUIREMENTS
**ElasticStack 6.4.x**  <br/>
**Nginx 1.10** - minimum<br/>
**Python 3.6 or greater** - don't even *attempt* anything else<br/>
**Ubuntu 18.04 (Desktop or Server)** - no, it won't run on 14.04<br/>
<br/>

# SYSTEM SETUP 
</br>

#### Install supporting tools and pkgs for Ubuntu
```
sudo apt-get update && sudo apt-get install software-properties-common
sudo add-apt-repository ppa:git-core/ppa
sudo apt-get install apt-transport-https sysv-rc-conf curl git
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
sudo apt-get update && sudo apt-get -y install build-essential checkinstall 
sudo apt-get -y install python-dev python-setuptools python-pip python-smbus libncursesw5-dev libgdbm-dev libc6-dev zlib1g-dev libsqlite3-dev tk-dev libssl-dev openssl libffi-dev 
```

## JAVA 8
The ElasticStack depends on Java to run, so we need to make sure that we have the Java 8 JDK installed before we install the stack.
```
java -version
```
On systems with Java 8 installed, this command produces output similar to the following:
```java
java version "1.8.0_65"
Java(TM) SE Runtime Environment (build 1.8.0_65-b17)
Java HotSpot(TM) 64-Bit Server VM (build 25.65-b01, mixed mode)
```
***If*** Java needs to be installed or upgraded to Java 8 (Java 9 is NOT supported)
```
sudo apt-get update && sudo apt-get install default-jdk
```
*Rerun the java -version command to verify you now have Java 8 installed*
</br>
</br>

## Install Elasticsearch, Logstash & Kibana
```
sudo apt-get update && sudo apt-get install elasticsearch=6.4.0 kibana=6.4.0 logstash=1:6.4.0-1
```

## Set apt to hold the versions so it doesn't inadvertently update
```
sudo apt-mark hold elasticsearch
sudo apt-mark hold kibana
sudo apt-mark hold logstash
```

</br>
</br>



# ELASTICSEARCH CONFIGURATION AND STARTUP

#### Edit the network.host setting (it is commented out) in the elasticsearch config file (as root)
```
sudo vi /etc/elasticsearch/elasticsearch.yml 
```

```network.host: 0.0.0.0```
</br>

#### Configure Elasticsearch to start with the system
```
sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable elasticsearch.service
```

#### Start Elasticsearch
```
sudo /bin/systemctl start elasticsearch.service
```

#### Verify Elasticsearch is running by issuing this at the command prompt
```
curl 127.0.0.1:9200
```
#### You should get JSON similar to the following:
```json
{
 "name" : "79dHjEG",
 "cluster_name" : "elasticsearch",
 "cluster_uuid" : "S09jzgZgTOa3ZO-xJVXFbA",
 "version" : {
   "number" : "6.2.2",
   "build_hash" : "10b1edd",
   "build_date" : "2018-02-16T19:01:30.685723Z",
   "build_snapshot" : false,
   "lucene_version" : "7.2.1",
   "minimum_wire_compatibility_version" : "5.6.0",
   "minimum_index_compatibility_version" : "5.0.0"
 },
 "tagline" : "You Know, for Search"
}
```
*If you see JSON similar to the above, Elasticsearch is now up and running*




# KIBANA CONFIGURATION AND STARTUP
#### Edit the elasticsearch.url setting (it is commented out) in the kibana config file (as root)
```
sudo vi /etc/kibana/kibana.yml 
```
```elasticsearch.url:"http://localhost:9200"```

</br>

#### Configure Kibana to start with the system
```
sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable kibana.service
```

#### Start Kibana
```
sudo /bin/systemctl start kibana.service
```

##### NOTE: The above commands provide no feedback as to whether Kibana was started successfully or not. Instead, this information will be written in the log files located in /var/log/kibana

#### Check to make sure Kibana is running by opening this link on the server: http://localhost:5601

*If you see the Kibana UI, Kibana is now up and running*
</br>
</br>


# LOGSTASH CONFIGURATION AND STARTUP
#### Edit the LS_USER setting (it is logstash by default) in the logstash startup.options file 
```
sudo vi /etc/logstash/startup.options
```
```LS_USER=root```

#### Create a symbolic link for the configuration for logstash (becasue, apparently, it is stupid)
```
sudo ln -s /etc/logstash /usr/share/logstash/config
```
</br>


#### Configure Logstash to start with the system
```
sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable logstash.service
```

#### Start Logstash
```
sudo /bin/systemctl start logstash.service
```


#### You are now done with the infrastructure setup*.  The rest of the SafeNetworking setup depends on cloning this repository.  Head back, from whence you came, and read the rest of the [README](https://github.com/PaloAltoNetworks/safe-networking) to finish the install.  

\*If you need access to SafeNetworking on port 80, see [NGINX SETUP](https://github.com/PaloAltoNetworks/safe-networking/wiki/NGINX-Setup) to allow for external connections to SafeNetworking.