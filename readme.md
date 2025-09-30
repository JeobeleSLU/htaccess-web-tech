<!-- TODO Readme -->
# A tutorial on how to setup .htaccess step by step



## ***Important!*** make sure that httpd.config is configure
- navigate to the directory C:\wamp64\bin\apache\apache2.4.62.1\conf
- inside the httpd.config navigate to <Directory "${INSTALL_DIR}/www/"> and add these
    -   AllowOverride all  
    -   Require all granted

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

## Curl basics [[Reference](https://curl.se/docs/manpage.html)]
Curl is a command tool that sends http requests basic syntaxes 
``` cmd
curl [options] [url]
```
- To get the response headers

``` cmd
curl -I [url]
```
- To get the response headers + body
``` cmd
curl -i [url]
```
- To login if there are auth the colon (:) seperates from user and password  
``` cmd
curl -u user:password -i [url]
```
 - For byte ranges 0-10
 ``` cmd
 curl -r 0-10 -i [url]
 ```
- For adding headers 
``` cmd
curl -H "Accept-Encoding: *" -I [url] 
```


### To Check for Cache control 
<!-- TODO steps  -->
### To Check for Content Negotiation
<!-- TODO steps-->

### To check for range headers
<!-- TODO  curl commands -->

### To check for Etag matching
<!-- TODO  curl commands-->

### To check if the file is compressed

To verify if it is compressed
- compare the [Content-Length] and  [Content-Encoding]
- Accept the original file no compression 

```cmd
curl -u user:password -H "Accept-Encoding: identity" -I http://localhost/trial/pokemon.csv
```
- Accept compressed file

```cmd
    curl -u user:password -H "Accept-Encoding: gzip" -I http://localhost/trial/pokemon.csv
```
