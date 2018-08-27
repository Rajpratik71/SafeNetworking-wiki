
Elasticsearch has a robust set of Backup and Restore tools.  The following instructions are basic, full backups of the SafeNetworking data.  For more information on the Snapshot/Restore features of Elasticsearch, see the [Elastic.co page](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html) for all options.
This document covers creating a Snapshot (backup).<br/>There are two main components to this:<br/>
1.) [Set up Elasticsearch for Snapshot/Restore](https://github.com/PaloAltoNetworks/safe-networking/wiki/Snapshot-Restore-Setup) - this only needs to be done once<br/>
2.) Perform snapshot procedures using the Dev Tools component of the Kibana UI<br/><br/>


## Perform snapshot procedures from Dev Tools

Snapshots are pushed (PUT) using the following format:
```PUT _snapshot/<location>/<snapshot name>?<options - if needed>```

```location``` was defined above in the PUT command, thus ours is "sfn"<br/>
```snapshot name``` is anything you want it to be, but it must be all lowercase, alpha-numeric and you can use the underscore "_", everything else will blow up and throw an error.<br/>
```options``` are, well, optional.  See [this page](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html) for more information on snapshot and restore options<br/>

Perform backup of SFN-DNS indices (this could timeout - no biggie). Change the date to the current date, or provide a naming convention that works for you:
```
PUT /_snapshot/sfn/09feb18?wait_for_completion=true
{
  "indices": "sfn-*,threat-*,traffic-*,af-details",
  "ignore_unavailable": true,
  "include_global_state": false
}
```

You should now have data underneath the /home/ubuntu/es_backup/sfn directory.  If you don't, something got messed up.  