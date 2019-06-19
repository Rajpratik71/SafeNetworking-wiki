To verify that logstash is up, running and listening on the ports specified for SFN you can use the following *nix command to test:

```
nc -z -v -u [hostname/IP address] [port number]
```

Here is an example of a successful command to our THREAT port (5514)
```# nc -z -v -u 192.168.10.12 5514
Connection to 192.168.20.95 5514 port [udp/ntp] succeeded!```
