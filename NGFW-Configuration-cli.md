### Command Line Syntax 
Use the following commands to configure the NGFW to forward all traffic and threat logs to your SFN instance.<br/>
### NOTE: Make sure to have the appropriate IP (line 1) and port (line 3) for your SFN implentation<br/>

```
set shared log-settings syslog syslog-SFN-5514 server SFN-server server <IP of you SFN server>
set shared log-settings syslog syslog-SFN-5514 server SFN-server transport TCP
set shared log-settings syslog syslog-SFN-5514 server SFN-server port 5514
set shared log-settings syslog syslog-SFN-5514 server SFN-server format BSD
set shared log-settings syslog syslog-SFN-5514 server SFN-server facility LOG_USER
set rulebase security rules "allow all" profile-setting profiles url-filtering SFN-Alert
set rulebase security rules "allow all" profile-setting profiles spyware SFN-Alert
set rulebase security rules "allow all" profile-setting profiles vulnerability SFN-Alert
set rulebase security rules "allow all" log-setting SFN-Log-Fowarding
set profiles url-filtering SFN-Alert credential-enforcement mode disabled
set profiles url-filtering SFN-Alert credential-enforcement log-severity medium
set profiles url-filtering SFN-Alert credential-enforcement allow credential-phish-test
set profiles url-filtering SFN-Alert safe-search-enforcement no
set profiles url-filtering SFN-Alert action block
set profiles url-filtering SFN-Alert alert [ abortion abused-drugs adult alcohol-and-tobacco auctions business-and-economy command-and-control computer-and-internet-info content-delivery-networks copyright-infringement dating dynamic-dns educational-institutions entertainment-and-arts extremism financial-services gambling games government hacking health-and-medicine home-and-garden hunting-and-fishing insufficient-content internet-communications-and-telephony internet-portals job-search legal malware military motor-vehicles music news not-resolved nudity online-storage-and-backup parked peer-to-peer personal-sites-and-blogs philosophy-and-political-advocacy phishing private-ip-addresses proxy-avoidance-and-anonymizers questionable real-estate recreation-and-hobbies reference-and-research religion search-engines sex-education shareware-and-freeware shopping social-networking society sports stock-advice-and-tools streaming-media swimsuits-and-intimate-apparel training-and-tools translation travel unknown weapons web-advertisements web-based-email web-hosting credential-phish-test ]
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

```