By default, SafeNetworking runs as a system service and logs directly to the safe-networking-sp/log/sfn.log file.  There may be times where SafeNetworking needs to be debugged and the console output provides more information than in the sfn.log file.  
This document covers the ability to run SafeNetworking in DEBUG mode **and** to capture the console logging in a file that can be used to debug the workflow(s).  You may have to have this run for a long period of time and it is suggested that you run it in a tmux session so that you may log out and back in and not have to worry about the SafeNetworking processes dying because your shell is gone.  

First, edit your .panrc file and make sure that the appropriate DEBUG settings are turned on under the Logging section:
```json
DEBUG = True
LOG_LEVEL = "DEBUG"
```
### NOTE: DO __NOT__ change DEBUG_MODE to True under the Application Specific heading.  Doing so will make SFN processes only 1 event at a time and the queue will get backlogged quickly.
<br/>

Next, you will need to run SFN in DEBUG mode for a while to gather data. 

To do this, ensure you have tmux installed:
```
sudo apt-get update && sudo apt-get install tmux
```
Since we are using the very basics of tmux, v2.1 or higher should suffice:
```
tmux -V
```
Because root owns the process of SafeNetworking (sfn) when it runs, it also writes the log files as root.  Therefore, to run in debug mode, you must change the permissions of the sfn.log file or you will get a permissions error
```
chmod 777 ~/safe-networking/log/sfn.log
```

Once you have tmux installed, you can now use it to run SafeNetworking and keep it running even when you kill your session.  So, in step 12 of the README the instructions would now look like this:
```
tmux new-session -s sfn
cd safe-networking
source env/bin/activate
python ./sfn > log/console-$(date +%Y-%m-%d_%H:%M).log 2>&1 &
```
This will run SafeNetworking in the background (ampersand "&" at the end of the python line) and the entire session will be in it's own shell that will remain running until either the system is shutdown or the tmux session is killed externally.

To exit the tmux session:
```
ctrl-b d
```
To return to that session:
```
tmux attach -t sfn
```

Other shortcuts and basic usage can be found here:  http://tmuxcheatsheet.com/



