### 1. Create and configure the .panrc for your installation
[Changing the .panrc file](https://github.com/PaloAltoNetworks/safe-networking/wiki/Default-.panrc-configuration-file)
<br/><br/>
### 2. Copy the SafeNetworking logstash configuration files to the logstash config directory
```
sudo cp install/logstash/pan-sfn.conf /etc/logstash/conf.d/
```
<br/>

### 3. Edit the /etc/logstash/conf.d/pan-sfn.conf file and replace the "CHANGEME" with your logstash listener and elasticsearch server where appropriate (4 places)
Example Input and Output stanzas.  Do not delete any of the lines. The filter stanza has been omitted and only sections of the input and output stanzas are shown for clarity.

```
input {
  syslog {
    host => "192.168.1.140"
    port => "5514"
    type => "syslog"
    tags => [ "PAN-OS_syslog" ]

...[SNIP]...

output {
    if "PAN-OS_traffic" in [tags] {
        elasticsearch {
            index => "traffic-%{+YYYY.MM.dd}"
            hosts => ["192.168.1.140:9200"]
        }
        stdout { codec => rubydebug }
    }
    else if "PAN-OS_threat" in [tags] {
        elasticsearch {
            index => "threat-%{+YYYY.MM.dd}"
            hosts => ["192.168.1.140:9200"]
        }
        stdout { codec => rubydebug }
    }
    else {
       elasticsearch {
           index => "parsefailure-%{+YYYY.MM.dd}"
           hosts => ["192.168.1.140:9200"]
       }
} 

```
<br/><br/>
### 4. Install the index mappings into ElasticSearch
NOTE: The setup script runs against localhost. If ES is bound to a particular IP address, you will need to edit the file and change it to reflect that.
```
cd install
bash ./setup.sh
```
<br/><br/>

### Now you must [configure the NGFW](https://github.com/PaloAltoNetworks/safe-networking/wiki/Home#config-ngfw)