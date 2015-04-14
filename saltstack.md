#Saltstack#

##Installation##
Salt is in the OSS repo on openSUSE. Regular releases may have slightly older versions whilst Tumbleweed will always have the latest release. To get the latest version on >=13.1 add the `systemsmangement` repository.

To install:
`zypper in salt-master` This installs the management component, multiple masters can be installed on the network
`zypper in salt-minion` This installs the client component, you can (should?) install -minion on master nodes

##Configuration##
All configuration files are stored in `/etc/salt`

###Master###
* Edit `/etc/salt/master`

Ports `4505` & `4506` need to be opened on the -master host firewall

To start the master service
Either `systemctl start salt-master` or `rcsalt-master start`

####WebUI####
The current WebUI is _Halite_, according to the project this has been deprecated in favour of SaltPad but I've not manged to get that up and running yet.

The default engine for Halite is CherryPy

To enable Halite you need to add a user that can login

      external_auth:  
        pam:  
          mytestuser:  
            - .*  
            - '@runner'  
            - '@wheel'
      
Add the following to the bottom of the config file:

    halite:  
     level: 'debug'  
     server: 'cherrypy'  
     host: '0.0.0.0'  
     port: '8080'  
     cors: False  
     tls: True  
     certpath: '/etc/pki/tls/certs/localhost.crt'  
     keypath: '/etc/pki/tls/certs/localhost.key'  
     pempath: '/etc/pki/tls/certs/localhost.pem'  

If you do not wish to use ssl, set 
###Minion###
* Edit `/etc/salt/minion`

The first thing to edit is the location of the _salt-master_, if the salt-master can not be found the minion service will fail to start.

    master: salt.example.com 

To start the minion service
Either `systemctl start salt-minion` or `rcsalt-minion start`
