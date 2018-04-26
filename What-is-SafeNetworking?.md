SafeNetworking is a dynamically observable threat intelligence system. It allows for the gathering, classification and visualization of threat and traffic information from many Palo Alto Networks based sources within the network. While SafeNetworking gathers all traffic and threat data, and it is all view-able, there are certain types of threat intelligence that are further classified using the Palo Alto Networks Threat Intelligence Cloud.  These, currently, are IP addresses, domains and URLs that have been observed to be related to malicious activity.

SafeNetworking pulls in both threat and traffic data-observations from security "sensors", usually the Palo Alto Networks NGFW, and creates a series of visualizations that allow the user to view, in real time, everything that is passing through these sensors. The data is fully query-able, using the [lucene](https://lucene.apache.org/core/2_9_4/queryparsersyntax.html) syntax and there are also visual tools to perform like queries against the entire data-set or a sub-set of the data. When you query for the data, you'll get back all events that match that query and then you can visualize that data or manipulate the data further to your liking.

SafeNetworking helps to ingest, normalize and store events, process certain types of events for further classification and then query and produce visualizations of threat intelligence. 

## The Process

### Ingest, Normalize and Store

SafeNetworking supports ingesting many different sources of threat and traffic data of differing types; for example syslog, https logs or even formatted files on the filesystem. Each type of data is then parsed and normalized into a data-model that SafeNetworking modules can use to further process the event. Each event, once parsed and normalized, is stored in an ElasticSearch database that has schemas that are highly optimized for the differing types of event data.


### Processing

SafeNetworking modules have the ability to determine the type of event based on information in the event data-model.  If the event matches the requirements for processing by the module, the module will then use post processors to derive additional intelligence for that event. A simple example would be that a malware name can be associated with a DNS query to a known malicious domain.


### Query

SafeNetworking can be queried via a web browser or directly using the API. The database schema is highly optimized for queries against a database of millions of records in a matter of seconds.  The queries can then be stored for use in the future and also for building visualizations of the data-set


### Visualizations

SafeNetworking allows for creating new data-sets from the stored data and then viewing the results in many different forms: charts, graphs, tables, etc.  These visualizations can then be saved and shared with anyone you want.  