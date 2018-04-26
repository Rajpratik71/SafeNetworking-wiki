### 1. Create and configure the .panrc for your installation
[Changing the .panrc file](https://github.com/PaloAltoNetworks/safe-networking/wiki/Default-.panrc-configuration-file)
<br/><br/>
### 2. Copy the SafeNetworking logstash configuration files to the logstash config directory
```
sudo cp install/logstash/sfn-dns.conf /etc/logstash/conf.d/
```
<br/>

### 3. Edit the /etc/logstash/conf.d/sfn-dns.conf file and replace the "CHANGEME" with your logstash listener and elasticsearch server where appropriate (3 places)
Example Input and Output stanzas.  Do not delete any of the lines. The filter stanza has been omitted and only sections of the input and output stanzas are show for clarity.

```
input {
  http {
    host => "10.10.10.10"
    port => '9563'
...

output {
  if "SFN-DNS" in [tags] {
    elasticsearch {
      hosts => ["10.10.10.10:9200"]
      index => ["sfn-dns-event"]
    }
    stdout { codec => rubydebug }
  }
  else if "_grokparsefailure" in [tags] {
    elasticsearch {
      hosts => ["10.10.10.10:9200"]
      index => ["sfn-dns-unknown"]
    }
...
```
<br/><br/>
### 4. Install the index mappings into ElasticSearch
NOTE: The setup script runs against localhost. If ES is bound to a particular IP address, you will need to edit the file and change it to reflect that.
```
cd install
bash ./setup.sh
```
<br/><br/>

### Now you must [configure the NGFW](https://github.com/PaloAltoNetworks/safe-networking/wiki/NGFW-Configuration)