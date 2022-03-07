# cogbook_unity

## A openedx plugin package to create xblocks

### Installation Steps:

#### Step 1:
[CogBooks] provides a plugin package to be installed in the open edX environment. Open the FTP client and connect to the server of the open edX platform of the institution.
Note: LMS admin should have permissions to access the Open edX server via an FTP client.
#### Step 2:
Open the server in a terminal for the next steps. 

#### Step 3:
Login as the root user. [sudo su -]

#### Step 4:
Run the below command in edxapp environment
```sh
pip install git+https://github.com/cogbookspblc/cogbook_unity
```
#### Step 5:
Add ```cogbook_unity```  to your ```INSTALLED_APPS``` in ```cms/envs/common.py``` or ```/edx/etc/studio.yml```
#### Step 6:
Add ```url(r'^cogbook_unity', include('cogbook_unity.urls')),```  in your ```cms/urls.py```
#### Step 7:
Restart LMS and CMS using the below command. 
```sh
/edx/bin/supervisorct1 restart all
```


[CogBooks]: <https://www.cogbooks.com/>
