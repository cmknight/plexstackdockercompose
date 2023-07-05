# plexstackdockercompose
docker/podman compose starting point for plex/transmission-vpn/radarr/sonarr/prowlarr

Folder structure based on this documentation:
https://wiki.servarr.com/docker-guide#consistent-and-well-planned-paths

Using the following:
Radarr - https://github.com/linuxserver/docker-radarr
Sonarr - https://github.com/linuxserver/docker-sonarr
Prowlarr - https://github.com/linuxserver/docker-prowlarr
Plex - https://github.com/linuxserver/docker-plex
Transmission w/ vpn - https://haugene.github.io/docker-transmission-openvpn

I included empty directory structure, with .gitkeep in each folder to be able to keep empty folders in github

Basic Instructions:

I'm running on centos 8, using podman-compose.  Your mileage may vary.

1) extract contents of this repo to a folder
2) update docker-compose.yaml for the 
environment:
    - OPENVPN_PROVIDER=SURFSHARK
    - OPENVPN_CONFIG=us-atl.prod.surfshark.com_udp
    - OPENVPN_USERNAME=######
    - OPENVPN_PASSWORD=######
Update this to use your specific VPN settings.  Look to the https://haugene.github.io/docker-transmission-openvpn repository for information.
Since I use Surfshark, I put an example value for the OPENVPN_CONFIG option for that provider.

Also for this setting:
LOCAL_NETWORK
I have 10.0.0.0/16, because of how my router assigns IP addresses. Yours may be 192.168.0.0/24, depends on your network settings.
3) start using podman-compose up -d, or docker-compose up -d if you're using docker.  There are permission differences if you're using docker vs podman, I had to run as root user with podman which is less than ideal. I will probably figure out why, but the problem I had was:

When starting transmission-openvpn service, it fail because of an issue that adding mknod to capabilities might theoretically resolve. However, didn't seem too.

It's something related to this:

https://www.redhat.com/sysadmin/container-permission-denied-errors



Remaining issues:
* Can't seem to figure out how to get sonarr to remove the download after it copies to the media folders.  I believe it has something to do with seeding ratios/etc, but either way its currently not working for me.