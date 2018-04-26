## Set up Elasticsearch from the command prompt 
#### Elasicsearch has to know the file path for all backups:<br/>
```sudo grep path.repo /etc/elasticsearch/elasticsearch.yml```

#### If the above returns nothing, you will have to edit the file and add the line in, pointing to the filesystem directory where all backups will take place.  This is usually /home/ubuntu/es_backup  So, the Paths section should look like this:
```
# ----------------------------------- Paths ------------------------------------
#
# Path to directory where to store the data (separate multiple locations by comma):
#
path.data: /var/lib/elasticsearch
#
# Path to log files:
#
path.logs: /var/log/elasticsearch
#
# Path to backups:
#
path.repo: /home/ubuntu/es_backup
#
```
#### If you do edit the file, you will need to restart Elasticsearch for it to find the changes. 
```sudo systemctl restart elasticsearch```<br/><br/>
#### Ensure that the directory exists and is world writeable:
``` ls -al /home/ubuntu```<br/>
#### Should return
```drwxrwxrwx 3 ubuntu ubuntu      4096 Feb  9 12:31 es_backup```
<br/><br/>
If it is not world writeable do this:<br/>
```chmod -R 777 /home/ubuntu/es_backup/```<br/><br/>
From a cli standpoint, you should be ready to run the snapshot/restore commands
<br/><br/>


Next, login to the Kibana UI and select Dev Tools on the left hand side.  Under the console section is where you will paste the following:

Tell Elasticsearch where to place the backup:
```
PUT _snapshot/sfn
{
  "type": "fs",
  "settings": {
    "location": "sfn"
  }
}
```


Check to make sure it is there - you should get back the json you put in the command above:
```
GET _snapshot
```
