# cogbook_unity

## A openedx plugin package to create xblocks

### Installation Steps:

#### Step 1:
[CogBooks] provides a plugin package (Ex: cogbook_unity) to be installed in the open edX environment. Open the FTP client and connect to the server of the open edX platform of the institution.
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
Add cogbook_unity  to your INSTALLED_APPS in cms/common.py or cms.env.json
#### Step 6:
Add url(r'^cogbook_unity', include('cogbook_unity.urls')),  in your cms/urls.py
#### Step 7:
Apply the following changes to the file ```sh/edx/app/edxapp/edx-platform/openedx/core/djangoapps/oauth_dispatch/views.py``` in your environment

```sh
diff --git a/openedx/core/djangoapps/oauth_dispatch/views.py b/openedx/core/djangoapps/oauth_dispatch/views.py

old mode 100644

new mode 100755

index 10889ef..c96bbc5

--- a/openedx/core/djangoapps/oauth_dispatch/views.py

+++ b/openedx/core/djangoapps/oauth_dispatch/views.py

@@ -92,9 +92,8 @@ class AccessTokenView(_DispatchingView):

     def dispatch(self, request, *args, **kwargs):
     
         response = super(AccessTokenView, self).dispatch(request, *args, **kwargs)
         
-
         if response.status_code == 200 and request.POST.get('token_type', '').lower() == 'jwt':
         
-            expires_in, scopes, user = self._decompose_access_token_response(request, response)

+            expires_in, scopes, user, refresh_token = self._decompose_access_token_response(request, response)

             content = {
             
                 'access_token': JwtBuilder(user).build_token(scopes, expires_in),
                 
@@ -102,6 +101,8 @@ class AccessTokenView(_DispatchingView):

                 'token_type': 'JWT',
                 
                 'scope': ' '.join(scopes),
                 
             }
             
+            if refresh_token:

+                content['refresh_token'] = refresh_token

             response.content = json.dumps(content)
             
         return response
         
@@ -115,7 +116,11 @@ class AccessTokenView(_DispatchingView):

         user = access_token_obj.user
         
         scopes = scope.split(' ')
         
         expires_in = content['expires_in']
         
-        return expires_in, scopes, user

+        try:

+            refresh_token = content['refresh_token']

+            return expires_in, scopes, user, refresh_token

+        except:

+            return expires_in, scopes, user, None
```
#### Step 8:
Restart LMS and CMS using the below command. 
```sh
/edx/bin/supervisorct1 restart all
```



[CogBooks]: <https://www.cogbooks.com/>
