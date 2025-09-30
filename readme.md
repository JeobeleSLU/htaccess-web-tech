<!-- TODO Readme -->
# A tutorial on how to setup .htaccess step by step #

## ***Important!*** make sure that httpd.config is configure ##
- navigate to the directory C:\wamp64\bin\apache\apache2.4.62.1\conf
- inside the httpd.config navigate to <Directory "${INSTALL_DIR}/www/"> and add these
    -   AllowOverride all  
    -   Require all granted

## Inside your C:\wamp64\www ##
- clone the project
```git
git clone https://github.com/JeobeleSLU/htaccess-web-tech.git
```

## Setup Your wamp server under apache and enable these ##

- auth_basic_module
- headers_module
- deflate_module
- negotiation_module
- filter_module


## Curl basics [[Reference](https://curl.se/docs/manpage.html)] ##
Curl is a command tool that sends http requests 
- basic command syntax
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

## Authentication ##

On .htaccess check for the line if it exists
```.htaccess

<FilesMatch "index">
    AuthType Basic
    AuthName "Restricted Area"
    AuthUserFile C:/wamp64/.htpasswd
    Require valid-user
</FilesMatch>

```
### setup the password in the htpasswd ###
 - First navigate to C:\wamp64\bin\apache\apache2.4.62.1\bin


```CMD
cd C:\wamp64\bin\apache\apache2.4.62.1\bin
```
 - create a user in the using the following command -c for create the next is the path and last is the user
``` CMD
htpasswd -c "C:/wamp64/.htpasswd" [User]
```
- overwrites the user password / add a user
```CMD
htpasswd "C:/wamp64/.htpasswd" [User]
```

- Deletes a user
``` cmd
htpasswd -D "C:/wamp64/passwords/.htpasswd" [User]
```

### Check if authentication is working ###
 **curl**
    - check for HTTP/1.1 401 Unauthorized
```cmd
curl -I -u wrong:wrong http://localhost/trial/
````
- To authenticate 
```cmd
curl -I -U correct-user:correct-password http://localhost/trial/
```

**Browser**
    - Open a Browser
    - navigate to http://localhost/trial/
    - Enter Username and password
## Cache control ##
On .htaccess check for the line 
```htaccess
<IfModule mod_headers.c>
    <FilesMatch "\.(html|json|xml)$">
        Header set Cache-Control "no-cache, no-store, must revalidate, private" 
    </Filesmatch>
</IfModule>
```
### Check if cache control is working  ###

**curl**
```cmd
curl -I -u user:passowrd http://localhost/trial/
```
- Browser
    - Open a browser obv in incognito
    - f12 to and network tabs
    - navigate to http://localhost/trial/
    - sign in 
    - On the network tab headers - cache control

## Compression ##
- Check if this line exists in the .htaccess and the deflate_module is enabled

```.htaccess
<IfModule mod_deflate.c>
    SetOutputFilter DEFLATE
    # Regex for anything that is already zip / compressed so dont zip it ok 
    SetEnvIfNoCase Request_URI \
        \.(?:gif|jpe?g|png|pdf|exe|t?gz|zip|bz2|sit|rar)$ no-gzip dont-vary
</IfModule>
```

To verify if it is compressed
 ***Note if the picture is needed use the picture .html under resource ***

 **Curl**
- compare the [Content-Length] and [Content-Encoding]
- Accept the original file no compression 

```cmd
curl -u user:password -H "Accept-Encoding: identity" -I http://localhost/trial/pokemon
```
- Accept compressed file

```cmd
    curl -u user:password -H "Accept-Encoding: gzip" -I http://localhost/trial/pokemon
```
**Browser**
    - Open a browser
    - open devtools
    - Observe Network tab
    - navigate to http://localhost/trial/pokemon
    - check header section Accept-Encoding and content-length


## Conditional Request ##
 - 304 will be sent if the resource is cached and it's not modified (same e-tag)
 - 200 will be sent if the resource is modified
 - if modified since will tell the server "only send if newer than X date”
 - if none matchh will tell the server "only send if ETag is different"

### curl ###
- **idk if this is even possible to check 304 with curl**
- Curl command with if modified since
```cmd
curl -I -H "If-Modified-Since: Tue, 30 Sep 2025 11:30:00 GMT" http://localhost/trial/data.html
```
- curl command with if none match
``` cmd
curl -I -H "If-None-Match: \"etag-here\"" http://localhost/trial/data.html
```
### Browser ###
- Open a browser
- open devtools
- on devtools check on the network tab 
- navigate to the http://localhost/trial/data
- click on the data
- check the headers 


## Content Negotiation ##
- check if the .htaccess has this line
    - If this errors check for the module if its disabled negotiation_module
```.htaccess
Options +MultiViews
```
 ### Curl ###
 - Check for content type header

```cmd
    curl -I http://localhost/trial/data
```


 - delete what's in the content type if its json delete the json then run the curl agan



- ### Browser ###
- Navigate to  http://localhost/trial/data
- now Delete the html file from the www folder
- then delete the another file if its xml then delete it etc


## Range headers ##

### Curl ###
- run the curl command

``` cmd
REM this would request bytes from 0-4, 10-171
curl -i -H "Range: bytes=0-4,10-171" http://localhost/trial/data.html
```

### Browser ###
- idk how to do this in here
