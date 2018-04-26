Elasticsearch has a robust set of Backup and Restore tools.  The following instructions are basic, restoration of full backups of the SafeNetworking data.  For more information on the Snapshot/Restore features of Elasticsearch, see the [Elastic.co page](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html) for all options.
This document covers restoring a Snapshot (backup).<br/>There are two main components to this:<br/>
1.) [Set up Elasticsearch for Snapshot/Restore](https://github.com/PaloAltoNetworks/safe-networking/wiki/Snapshot-Restore-Setup) - this only needs to be done once and is for both Snapshots and Restores<br/>
2.) Perform restore procedures using the Dev Tools component of the Kibana UI<br/><br/>


## Perform restore procedures from Dev Tools

Login to the Kibana UI and select Dev Tools on the left hand side.  Under the console section is where you will paste the following:

### Before Restoring
BEFORE restoring, you have to get rid of the indices that we will be restoring.  There are ways to do partial restores, or just closing an index before restoring; see the Elastic documentation for those procedures and options.

To delete the indices that will be restored, perform the following:
```json
DELETE <index name>
```
That's pretty much it.
<br/>


To view what Snapshots are on the filesystem:<br/>
```
GET _snapshot/sfn/_all
```
NOTE: change 'sfn' above if yours is different. 
If you do not see the Snapshot you want to restore, verify that you have put the files in the right directory on the filesystem.  The default directory, if you followed the directions, would be /home/\<userid\>/es_backup/sfn.
<br/><br/>

## Restoring Snapshots
Snapshots are restored (PUT) using the following format:
```
PUT _snapshot/<location>/<snapshot name>/_restore?<options - if needed>
```

```location``` was defined in the [Set up Elasticsearch for Snapshot/Restore](https://github.com/PaloAltoNetworks/safe-networking/wiki/Snapshot-Restore-Setup) section, thus ours is "sfn"<br/>
```snapshot name``` is the name of the snapshot you want to restore.<br/>
```_restore``` is the directive to restore the snapshot you named in <snapshot name>
```options``` are, well, optional.  See [this page](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html) for more information on snapshot and restore options<br/>

#### Example
To restore just the SafeNetworking indices (this excludes the .kibana index from a full backup), you can specify the indices:
```
PUT /_snapshot/sfn/09feb18/_restore
{
  "indices": "sfn-*, af-details",
  "ignore_unavailable": true,
  "include_global_state": false
}
```

You should now have restored the backup and all SafeNetworking indices for that backup are populated.  