#Saltstack#

##Installation##
Salt is in the OSS repo on openSUSE. Regular releases may have slightly older versions whilst Tumbleweed will always have the latest release. To get the latest version on >=13.1 add the `systemsmangement` repository.

To install:
`zypper in salt-master` This installs the management component, multiple masters can be installed on the network
`zypper in salt-minion` This installs the client component, you can (should?) install -minion on master nodes

##Master Configuration##
* Edit `/etc/salt/master`

Ports `4505` & `4506` need to be opened on the master firewall

To start the master service
Either `systemctl start salt-master` or `rcsalt-master start`
Enable the master service to start at boot `systemctl enable salt-master.service`

###WebUI###
The current WebUI is _Halite_, according to the project this has been deprecated in favour of SaltPad but I've not manged to get that up and running yet.

The default engine for Halite is CherryPy

To enable Halite you need to add a user that can login

      external_auth:  
        pam:  
          $USER:  
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

* If you do not wish to use ssl, use `tls: False`
* To bind to a specific IP instead of all available change `0.0.0.0` to whichever IP you want

You will need to change ownership of several files so that non-root users can use salt.

`chown -R $USER /etc/salt /var/cache/salt /var/log/salt /var/run/salt`

###Key Management###
Salt communication is encrypted. Minions generate keys and submit them to the master for acceptance, once the master accepts the keys communication can begin.

On the master check what keys have been submitted/accepted/rejected
`salt-key -L`

You can either bulk or individually accept or reject keys
Individually accept - `salt-key -a`
Bulk accept - `salt-key -A`
Individually reject - `salt-key -r`
Bulk reject - `salt-key -R`

##Minion Configuration##
* Edit `/etc/salt/minion`

The first thing to edit is the location of the _salt-master_, if the salt-master can not be found the minion service will fail to start.

    master: salt.example.com 

To start the minion service
Either `systemctl start salt-minion` or `rcsalt-minion start`
Enable the minion service to start at boot `systemctl enable salt-minion.service`

##Usage##
Regular commands used in Salt

###Adding a repo###
`sudo salt '*' pkg.mod_repo alias alias=$ALIAS url=$URL enabled=True autorefresh=True`

###Installing packages###
Installing a single package
`sudo salt '*' pkg.install $PACKAGE`

Installing multiple packages
`sudo salt '*' pkg.install pkgs='["$PACKAGE1", "$PACKAGE2", "$PACKAGE3", "$PACKAGE4"]'