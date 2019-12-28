# PiHole_Docker
pihole in docker
# ISSUE AND WORKAROUND
The -dns setting is used to tell docker what DNS servers should be used for internal resolving. But because docker uses 127.0.0.11 (yes, two 11) for internal resolving, it somehow messes up 127.0.0.1. Probably a pihole bug, not sure.
The workaround is to bind mount a resolv.conf file. I created one as such:
echo -e "nameserver 127.0.0.1\nnameserver 192.168.88.118" > overwrite_resolv.conf
Specify the above file in your bind mount

Clients are not showing in the network tab, but the correct IP addresses are showing in the query log
