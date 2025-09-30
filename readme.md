<!-- TODO Readme -->
# A tutorial on how to setup .htaccess step by step

## Setup Your wamp server under apache and enable these

- auth_basic_module
- headers_module
- deflate_module
- negotiation_module
- filter_module

### setup the password in the htpasswd
 - First navigate to C:\wamp64\bin\apache\apache2.4.62.1\bin


```CMD
cd C:\wamp64\bin\apache\apache2.4.62.1\bin
```
 - create a user in the using the following command -c for create the next is the path and last is the user
``` CMD
htpasswd -c "C:/wamp64/passwords/.htpasswd" [User]
```
- overwrites the user password / add a user
```CMD
htpasswd "C:/wamp64/passwords/.htpasswd" [User]
```

- Deletes a user
``` cmd
htpasswd -D "C:/wamp64/passwords/.htpasswd" [User]
```

### To Check for Cache control 
<!-- TODO steps  -->
### To Check for Content Negotiation
<!-- TODO steps-->

### To check for range headers
<!-- TODO  curl commands -->

### To check for Etag matching
<!-- TODO  curl commands-->

