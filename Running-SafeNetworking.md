As of version 2.0.3, SafeNetworking runs as a system service in Ubuntu.  You have the same commands for SafeNetworking as you would for any other service - i.e. systemctl and service. 

To view if SafeNetworking is running:
```
sudo service sfn status
```

To stop SafeNetworking:
```
sudo service sfn stop
```

To start SafeNetworking (this takes a minute):
```
sudo service sfn start
```

To restart SafeNetworking (this takes a minute)"
```
sudo service sfn restart
```

If, for some reason, SafeNetworking does not start automatically, you can use the 'journalctl' to view the logfile for system errors when trying to start SafeNetworking.  