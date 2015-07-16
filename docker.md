#Docker#

##Installing and setting up Docker##
Install the docker package - `sudo zypper install docker`

To automatically start the Docker daemon at boot time `sudo systemctl enable docker`

Start the Docker daemon - `sudo systemctl start docker`

The Docker daemon listens on a local socket which is accessible only by the _root_ user and by the members of the _docker_ group. The docker group is automatically created at package installation time. To allow a certain user to connect to the local Docker daemon - `sudo /usr/sbin/usermod -aG docker $USERNAME`

The user can communicate with the local Docker daemon upon next login. 

##Networking##
If you want your containers to be able to access the external network, you must enable the _ipv4 ip_forward_ rule. This can be done using YaST by browsing to _Network Devices › Network Settings › Routing_ menu and ensuring _Enable IPv4 Forwarding_ is checked.

This option cannot be changed when networking is handled by the Network Manager. In such cases the `/etc/sysconfig/SuSEfirewall2` file needs to be edited by hand to ensure the _FW_ROUTE_ flag is set to yes:

`FW_ROUTE="yes"`

##Getting Docker images##
Base Docker images can be obtained from the [Docker Hub][A]. The format is _$IMAGE:$TAG_:
`docker pull opensuse:13.2` or `docker pull opensuse:tumbleweed`

#Modifying a Docker image##
There are two ways to create a modified docker image:
1. Run a base image
`docker run -t -i opensuse:tumbleweed /bin/bash`
Make the desired changes.
`exit`
Commit the changes
`docker commit -m "added foo.bar" 432f3f67dece awafaa/tumbleweed:foo`

`-m` - is to specify the commit message
`432f3f67dece` - is the container id that you are basing on
`awafaa/tumbleweed` - is the new repository to save it to
`:foo` - is the new tag

[A]: https://registry.hub.docker.com/