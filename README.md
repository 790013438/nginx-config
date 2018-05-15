nginx-config
==============
## Installing NGINX(安装)
1. Open terminal window and open the sources.list file using the command
```
sudo nano /etc/apt/sources.list
```
2. You can add the Nginx repository links at the bottom of the file. Scroll down to
the very bottom of the file and add the two lines below :
```
deb http://nginx.org/packages/ubuntu/ trusty nginx
deb-src http://nginx.org/packages/ubuntu/ trusty nginx
```
3. Save the file.
4. Now you can download the package lists from the repositories and update
them with the information for the newest versions of the packages and their
dependencies. You can do that by typing the following command:
```
sudo apt-get update
```
5. You will get the following error regarding the missing signature key. It is
happening because gpg is trying to sign the nginx release and check its signature.
But the signing key is missing on the server and hence gpg is not able to validate
the nginx package:
Reading package lists... Done
W: GPG error: http://nginx.org trusty Release: The following signatures couldn't
be verified because the public key is not available: NO_PUBKEY ABF5BD827BD9BF62
6. Download and add the nginx signature key using the command below:
```
wget http://nginx.org/keys/nginx_signing.key
sudo apt-key add nginx_signing.key
```
7. Now try re-synchronizing the package index from the sources:
```
sudo apt-get update
```
8. Now that the package list is updated and indexed, you can install Nginx:
```
sudo apt-get install nginx
```
 9. You can verify Nginx installed the version:
```
nginx –v
```

You can get the complete list of all Nginx configuration details and its version by
using the –V command option with nginx . Here is a sample output of the command:
```
nginx –V
```

It has a mine.types and fastcgi_params file that contains all
the mime types that are enabled on the web server and fastcgi configuration details.
All these default configurations enable the Nginx server to start:

```
ls –F /etc/nginx/
```
