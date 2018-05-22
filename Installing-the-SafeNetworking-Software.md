#### NOTE: The following is done in the home directory of the user that is going to run/manage SafeNetworking.  All instructions after this step (tuning, configuration, troubleshooting) will refer to the safe-networking-sp directory with an assumption that YOU know where it is.  
### 1. Clone repo
```git clone https://www.github.com/PaloAltoNetworks/safe-networking.git```
<br/><br/>
### 2. Change into repo directory
```cd safe-networking```
<br/><br/>
### 3. Create python 3.6 virtualenv
```python3.6 -m venv env```
<br/><br/>
### 4. Activate virtualenv
```source env/bin/activate```
<br/><br/>
### 5. Download required libraries
```pip install -r requirements.txt```
<br/><br/>
### 6. Deactivate the virutalenv (we will return to it later)
```deactivate```
<br/><br/>

### Next, you will need to [configure SafeNetworking](https://github.com/PaloAltoNetworks/safe-networking/wiki/Configuring-SafeNetworking)