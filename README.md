# docker-template
### template for running ERDDAP, THREDDS, NGINX docker containers

1. Setup a ubuntu or centos machine with plenty of RAM.  I used digitalocean to create a droplet with Ubuntu 16.04, 16GB RAM, 160GB disk.  This instance is $160/month, but I'm only going to use it for two days before destroying it, so about $11)

2. Set up your endpoint name.  I used "do.maltlab.com" because I own the maltlab.com domain.  I set up the custom nameservers for maltlab.com following the instructions on https://cloud.digitalocean.com/networking. Let's say your endpoint is "mythredds.com".

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
Note: the docker install script logs you off so that changes can take effect, so you need to log back in.

5. Get Rich's configuration template for pycsw, thredds, erddap and nginx (with let's encrypt):
```
cd ~/github
git clone https://github.com/rsignell-usgs/docker-template.git
mv docker-template /opt/docker
```
6. Edit the Let's encrypt script, replacing the [CERTS line](https://github.com/rsignell-usgs/docker-template/blob/master/nginx/do_get#L2) with your endpoint
```
cd /opt/docker
cd nginx
chmod +x do_get
vi do_get
```
7. After modifying the CERTS and EMAIL lines in do_get, run it:
```
./do_get
```
8. Grep for all the places to change the domain name
```
cd /opt/docker
grep 'do.maltlab.com' -r *
```
and edit those, specifying your endpoint (e.g. "mythredds.com") 

9. Edit docker-compose.yml and make sure it looks okay.
```
cd /opt/docker
vi docker-compose.yml
```
10. Fire up the containers. 
```
cd /opt/docker
docker-compose up -d
```
11. login to the pycsw container and run `pycsw_setup`:
```
docker exec -it pycsw bash
pycsw_setup
exit
```
12. Grep for all the tomcat-user passwords to change:
```
cd /opt/docker
grep 'changeme' -r *
```
and edit the `tomcat-users.xml` files to enter your SHA1 passwords (can use http://www.sha1-online.com/ to generate)

13. Bounce the Docker containers
```
cd /opt/docker
docker-compose down
docker-compose up -d
```


