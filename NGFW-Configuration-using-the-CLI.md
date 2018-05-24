### Command Line Syntax 
Use the following commands to configure the NGFW to forward all traffic and threat logs to your SFN instance.<br/>
### NOTE: Make sure to have the appropriate IP (line 1) and port (line 3) for your SFN implentation<br/>

```
set shared log-settings syslog syslog-SFN-5514 server SFN-server server <IP address of your server>
set shared log-settings syslog syslog-SFN-5514 server SFN-server transport TCP
set shared log-settings syslog syslog-SFN-5514 server SFN-server port 5514
set shared log-settings syslog syslog-SFN-5514 server SFN-server format BSD
set shared log-settings syslog syslog-SFN-5514 server SFN-server facility LOG_USER
set profiles url-filtering SFN-Alert credential-enforcement mode disabled 
set profiles url-filtering SFN-Alert credential-enforcement log-severity medium
set profiles url-filtering SFN-Alert safe-search-enforcement no
set profiles url-filtering SFN-Alert action block
set profiles url-filtering SFN-Alert alert [ abortion abused-drugs adult alcohol-and-tobacco auctions business-and-economy command-and-control computer-and-internet-info content-delivery-networks copyright-infringement dating dynamic-dns educational-institutions entertainment-and-arts extremism financial-services gambling games government hacking health-and-medicine home-and-garden hunting-and-fishing insufficient-content internet-communications-and-telephony internet-portals job-search legal malware military motor-vehicles music news not-resolved nudity online-storage-and-backup parked peer-to-peer personal-sites-and-blogs philosophy-and-political-advocacy phishing private-ip-addresses proxy-avoidance-and-anonymizers questionable real-estate recreation-and-hobbies reference-and-research religion search-engines sex-education shareware-and-freeware shopping social-networking society sports stock-advice-and-tools streaming-media swimsuits-and-intimate-apparel training-and-tools translation travel unknown weapons web-advertisements web-based-email web-hosting ]
set profiles spyware SFN-Alert botnet-domains lists default-paloalto-dns action sinkhole 
set profiles spyware SFN-Alert botnet-domains sinkhole ipv4-address pan-sinkhole-default-ip
set profiles spyware SFN-Alert botnet-domains sinkhole ipv6-address ::1
set profiles spyware SFN-Alert rules SFN-Anti-Spyware action alert 
set profiles spyware SFN-Alert rules SFN-Anti-Spyware severity any
set profiles spyware SFN-Alert rules SFN-Anti-Spyware threat-name any
set profiles spyware SFN-Alert rules SFN-Anti-Spyware category any
set profiles spyware SFN-Alert rules SFN-Anti-Spyware packet-capture disable
set profiles vulnerability SFN-Alert rules SFN-Vulnerability-Alert action alert 
set profiles vulnerability SFN-Alert rules SFN-Vulnerability-Alert vendor-id any
set profiles vulnerability SFN-Alert rules SFN-Vulnerability-Alert severity any
set profiles vulnerability SFN-Alert rules SFN-Vulnerability-Alert cve any
set profiles vulnerability SFN-Alert rules SFN-Vulnerability-Alert threat-name any
set profiles vulnerability SFN-Alert rules SFN-Vulnerability-Alert host any
set profiles vulnerability SFN-Alert rules SFN-Vulnerability-Alert category any
set profiles vulnerability SFN-Alert rules SFN-Vulnerability-Alert packet-capture disable
set shared log-settings profiles SFN-Log-Forwarding match-list "Syslog-Traffic-SFN" send-syslog syslog-SFN-5514
set shared log-settings profiles SFN-Log-Forwarding match-list "Syslog-Traffic-SFN" log-type traffic
set shared log-settings profiles SFN-Log-Forwarding match-list "Syslog-Traffic-SFN" filter "All Logs"
set shared log-settings profiles SFN-Log-Forwarding match-list "Syslog-Traffic-SFN" send-to-panorama no
set shared log-settings profiles SFN-Log-Forwarding match-list "Syslog-Threat-SFN" send-syslog syslog-SFN-5514
set shared log-settings profiles SFN-Log-Forwarding match-list "Syslog-Threat-SFN" log-type threat
set shared log-settings profiles SFN-Log-Forwarding match-list "Syslog-Threat-SFN" filter "All Logs"
set shared log-settings profiles SFN-Log-Forwarding match-list "Syslog-Threat-SFN" send-to-panorama no
set shared log-settings profiles SFN-Log-Forwarding match-list "Syslog-URL-SFN" send-syslog syslog-SFN-5514
set shared log-settings profiles SFN-Log-Forwarding match-list "Syslog-URL-SFN" log-type url
set shared log-settings profiles SFN-Log-Forwarding match-list "Syslog-URL-SFN" filter "( category eq malware ) or ( category eq command-and-control ) or (category eq hacking) or (category eq phishing)"
set shared log-settings profiles SFN-Log-Forwarding match-list "Syslog-URL-SFN" send-to-panorama no
set rulebase security rules "allow all" profile-setting profiles url-filtering SFN-Alert
set rulebase security rules "allow all" profile-setting profiles spyware SFN-Alert
set rulebase security rules "allow all" profile-setting profiles vulnerability SFN-Alert
set rulebase security rules "allow all" log-setting SFN-Log-Fowarding
set rulebase security rules SFN-Logging profile-setting profiles url-filtering SFN-Alert
set rulebase security rules SFN-Logging profile-setting profiles spyware SFN-Alert
set rulebase security rules SFN-Logging profile-setting profiles vulnerability SFN-Alert
set rulebase security rules SFN-Logging to untrust
set rulebase security rules SFN-Logging from trust
set rulebase security rules SFN-Logging source any
set rulebase security rules SFN-Logging destination any
set rulebase security rules SFN-Logging source-user any
set rulebase security rules SFN-Logging category any
set rulebase security rules SFN-Logging application any
set rulebase security rules SFN-Logging service any
set rulebase security rules SFN-Logging hip-profiles any
set rulebase security rules SFN-Logging action allow
set rulebase security rules SFN-Logging log-setting SFN-Log-Fowarding

```