#sharing file from linux to windows using samba service

This is the hands-on practice about how we transfer file from linux to windows(vice versa) 

Tools used:
    - VM linux
    - Windows Explorer

Service need to install:
    - SAMBA (smb.service)
    - nmb.service

/////////Lets start/////////////

We need to install 
        # samba service in linux by
            - sudo apt install samba
then,
        # start the service & enable the service (smb.service& nmb.service)

            - sudo systemctl start smb.service
            - sudo systemctl start nmb.service
       
        # now enable,
            - sudo systemctl enable smb.service
            - sudo systemctl enable nmb.service

$$$$ now we need to add the service in firewall $$$$

            - sudo firewall-cmd --add-service=samba --permanent

        # reload the firewall 
            - sudo firewall-cmd --reload
### now we complete install, start, enable, adding firewall ###

### next ###

        # making the directory to share 
            - sudo mkdir /sambafile
        # we need to change the 
            - chmod 777 sambafile
        # so only windows side can make a change if u dont want keep it default

        # then we need to create a new user to use the service by the user for security purpose

            - useradd john
        # make the user passwd for smb related 

            - smbpasswd -a john
              enter passwd

$$$$ now we create user, the specific user only access the windows side with the security credentials

### we move on to config a file into our system ###         

        # moving too etc/samba/smb.config and edit tha smb.config

            - vi etc/samba/smb.config

        # inside that we create into the last line 


[folder_samba]
        commment = practical pupose
        path = /sambafile
        writeable = yes
        browsable = yes
        create mask = 0664
        directory mask = 0755
        valid user = john

####end#####

        #restart the service 
            - systemctl restart smb.service
            - systemctl restart nmb.service

##### after that important step is need to change system security mode , default 'enforcing' we need to change 
##### permissive 

        # using 
            - sestatus to check the type 
            - setenforce 0   it will change the security mode

        # restart the smb.service 


$$$$$$$$$$$$$$$$$$ Going to windows side $$$$$$$$$$$$$$$$$$$$$$$$$

        # search the linux in Win+R and enter "\\IP"            (note: \\ only works)

        # then login using username and pasord we created before.......


&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& if windows shows network error whilw connecting to that ip &&

        # we have to manually start the samba service in windows 

            go to control panel > program > turn windows on or off > 
                   -  enable samba & cfts smb file
                   -  turn on windows discovery if asked

                     @@@@@@ restart pc @@@@@@

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ succesfull while using this method ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^



















