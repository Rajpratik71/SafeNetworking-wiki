# The .panrc Configuration File

The .panrc configuration file allows the administrator to make changes to certain system settings that will affect how the SafeNetworking software performs, interacts with the infrastructure and for debugging problems that may arise with the system. 

The .panrc file is the ***only*** file that should be changed within the SafeNetworking system without help from the SP-Solutions or the Account Team.  The file is self documenting with comments for each setting available.  Below is a file that you can cut and paste and use as your own .panrc file.  It is a copy of the same file in the install/sfn directory from the repository.  You can also just copy that over to your home directory if you wish.  Leave anything you do not want changed commented out except for the AUTOFOCUS\_API_KEY setting, that must be filled in for SafeNetworking to run and process the events.

Each setting, as you see it, is the system default.  

### The .panrc file should be stored in the home directory of the user that runs SafeNetworking.


```python
################################################################################
# This file is used for all personalized/instance settings within the 
# application.  Only set what you need and accept the defaults for the rest.
# Consult the documentation and/or your account team for more 
# information on the settings in this file.  
#

################################################################################
#                           APPLICATION SPECIFIC
################################################################################
#
# Set this to true to only process 1 event at a time.  This slows down the 
# logging and allows us to see what is going on if there are bugs
# DEBUG_MODE = False

# Set the number of seconds for multi-threading to wait between processing calls 
# DNS_POOL_TIME = 10
# URL_POOL_TIME = 10
# AF_POOL_TIME = 600

# Turn on/off modules.  By default DNS is the only thing running. 
# DNS_PROCESSING = True
# IOT_PROCESSING = False
# URL_PROCESSING = False

# Number of AF points left to slow down processing so we don't run out of points
# When it reaches this point, it slows execution to 1 event at a time.
# AF_POINTS_LOW = 5000

# Number of AF points left to stop processing all together
# AF_POINT_NOEXEC = 500

# Number of seconds to wait when AF_POINT_NOEXEC gets triggered.  This stops all
# app execution and checks the AF points total at the specified interval.  When
# the points total is higher than AF_POINT_NOEXEC it resumes execution.
# AF_NOEXEC_CKTIME = 3600

# Set the number of processes to run the DNS module.  This cannot be more than
# 16 or it will kill the AF minute points.  The code will  take care of cases
# where it is greater than 16 and this should only be adjusted down (never up).
# Remember that DNS_POOL_COUNT and URL_POOL_COUNT have to share the total of 16. 
# The application will check and throw an error (but it will still run) if the 
# two settings together is more than 16.  
# DNS_POOL_COUNT = 16

# Set the number of processes to run the URL module.  This cannot be more than
# 16 or it will kill the AF minute points.  The code will  take care of cases
# where it is greater than 16 and this should only be adjusted down (never up).
# Remember that DNS_POOL_COUNT and URL_POOL_COUNT have to share the total of 16. 
# The application will check and throw an error (but it will still run) if the 
# two settings together is more than 16.
# URL_POOL_COUNT = 0

# When SafeNetworking is started, number of documents to read from the DB.  The
# larger the number, the longer it goes between queries to the local DB.  
# DNS_EVENT_QUERY_SIZE = 1000

# SafeNetworking caches domain info from AutoFocus.  This setting specifies, in 
# days, how long the cache is ok.  If there is cached info on this domain and it
# is older than the setting, SFN will query AF and update as necessary and reset
# the cache "last_updated" setting in ElasticSearch.  It is recommended to make
# this shorter than the default once you understand how it works and can 
# calculate how often the cache should be updated.
# DNS_DOMAIN_INFO_MAX_AGE = 30

# The Autofocus queries can take quite a bit of time due to the billions of 
# records that the query has to search through.  Usually, the most pertinent 
# info is within the first couple of minutes of query time.  So, set this to drop
# out of the processing loop and stop waiting for the query to finish. 
# This is set in minutes.
# AF_LOOKUP_TIMEOUT = 2

# The maximum percentage of the AF query we are willing to accept.  If, when we
# check the timer above, the value is greater than this percentage, we bail out
# of the loop.  The lower the number, the more likely that we may not get a 
# result.  Though, usually, 2 minutes and 20 percent is enough to get at least 
# the latest, most relevant, result(s).
# AF_LOOKUP_MAX_PERCENTAGE = 20

# The maximum age for tag info.  This doesn't need to be updated as often as
# the domain or other items, but should be done periodically just in case.
# Setting is in days.
# DOMAIN_TAG_INFO_MAX_AGE = 120
#
# Dictionary definition of confidence levels represented as max days and the 
# level associated  - i.e. 25:80 would represent an 80% confidence level if the 
# sample is no more than 25 days old
# CONFIDENCE_LEVELS = "{'15':90,'25':80,'40':70,'50':60,'60':50}"


################################################################################
#                                LOGGING
################################################################################
# Set debug level for Flask - if 'DEBUG = True' all debug messages will appear
# on console where SafeNetworking application was started
# DEBUG = True
#
# Set the logging level for the SafeNetworking application.  All messages will
# be sent to log/sfn.log
# LOG_LEVEL = "DEBUG"
#
# Size of Log file before rotating - in bytes
# LOG_SIZE = 100000000
#
# Number of log files to keep in log rotation
# LOG_BACKUPS = 10


################################################################################
#                              ELASTICSTACK
################################################################################
# The settings for this instance are bound to an interface on the
# system.  Only change the default settings to use that IP if you are not using
# nginx to broker the Kibana interface or you have a multi-system cluster using
# load balancing
# ELASTICSEARCH_HOST = "localhost"
# ELASTICSEARCH_PORT = "9200"
# ELASTICSEARCH_HTTP_AUTH = ""
# KIBANA_HOST = "localhost"
# KIBANA_PORT="5601"



################################################################################
#                                 API Keys
################################################################################
# API Key for Autofocus.  Allows SafeNetworking to query AF via the API 
# without having to have credentials stored somewhere.  The following MUST be 
# entered before SafeNetworking can process the events and gather data from 
# the external systems
# AUTOFOCUS_API_KEY = ""
#
```


