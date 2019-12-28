# PiHole_Docker

# ISSUE AND WORKAROUND
The -dns setting is used to tell docker what DNS servers should be used for internal resolving. But because docker uses 127.0.0.11 (yes, two 11) for internal resolving, it somehow messes up 127.0.0.1. Probably a pihole bug, not sure.

The workaround is to bind mount a resolv.conf file. I created one as such:
echo -e "nameserver 127.0.0.1\nnameserver 192.168.88.118"\nnameserver 127.0.0.11 > overwrite_resolv.conf
We specify the above in our bind mount.

Clients are not showing in the network tab, but the correct IP addresses are showing in the query log.



# Sort out the network stuff first
Pull the docker container docker pull pihole/pihole:latest

sudo systemctl disable systemd-resolved.service
sudo systemctl stop systemd-resolved.service

### I dont have this file
sudo nano /etc/NetworkManager/NetworkManager.conf

Add dns=default under [main]

sudo mv /etc/resolv.conf /etc/resolv.conf.bak

### No need for the below either if you dont have it
sudo service network-manager restart

# Connect to the running pihole container
docker exec -ti pihole /bin/bash

# Later on create own network if you want
sudo docker network create -d macvlan --subnet=192.168.88.0/24 --gateway=192.168.88.1  -o parent=enp3s0 main_net

