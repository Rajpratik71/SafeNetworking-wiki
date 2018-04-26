Sometimes during an eval (or for other reasons) the API key may need to be changed.  

After obtaining the new key, stop SafeNetworking on the server:

```
sudo service sfn stop
```

Once it is stopped, edit the .panrc file:

```
vi ~/.panrc
```

Find the AUTOFOCUS_API_KEY variable (it should be towards the bottom of the file) and replace the value with the new key.  Save the file and start SafeNetworking back up.

```
sudo service sfn start
```

#### Optional
To verify the new key is working, tail the log/sfn.log file and make sure you are not getting errors about the API key being incorrect.
