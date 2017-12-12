# docker-template
### template for running ERDDAP, THREDDS, NGINX docker containers

1. Setup a ubuntu machine with plenty of RAM.  I used digitalocean to create a droplet with Ubuntu 16.04, 16GB RAM, 160GB disk.  This instance is $160/month, but I'm only going to use it for two days before destroying it, so about $11)

2. Set up a domain name.  I used "do.maltlab.com" because I own the maltlab.com domain.  I set up the custom nameservers for maltlab.com following the instructions on https://cloud.digitalocean.com/networking

3. ssh in to the machine as root. [Create user "rsignell" and add to sudo list](https://www.digitalocean.com/community/tutorials/how-to-create-a-sudo-user-on-ubuntu-quickstart).  
```
adduser rsignell
usermod -aG sudo rsignell
```
4. Run Julien's nice script for installing Docker and Docker-compose:
```
mkdir github
git clone https://github.com/Unidata/xsede-jetstream.git
cd xsede-jetstream
chmod +x docker-install.sh
./docker-install.sh -u rsignell
```
Note: the docker install script logs you off part way through, so you need to log back in and run again to complete.

5. Get Rich's configuration template for pycsw, thredds, erddap and nginx (with let's encrypt):
```
cd ~/github
git clone https://github.com/rsignell-usgs/docker-template.git
```
6. Edit the Let's encrypt script, replacing the [CERTS line](https://github.com/rsignell-usgs/docker-template/blob/master/nginx/do_get#L2) with your endpoint
```
cd docker-template
cd nginx
chmod +x do_get
vi do_get
```
7. After modifying the CERTS and EMAIL lines in do_get, run it:
```
./do_get
```
 
