gsm\_run\_shell (B210)

This doc is for gsm run multi USRP b200

== First of all you need to have admin privileges
    
    $ sudo su -

== Create Script
    
    # wget https://dl.dropbox.com/s/l6bappgovaz71z8/startUSRP -O /usr/local/src/startUSRP
    # echo "alias startUSRP='/usr/local/src/startUSRP'" >>  ~/.bashrc

== Give file the execute permission
    
    # chmod +x /usr/local/src/startUSRP

== Execute transceiver_usrp_with_serial
    
    # cd /usr/local/src/
    # ./startUSRP --device 0

== Replace transceiver file
    
    # cd /usr/local/src/public/openbts/trunk/apps/
    # rm transceiver
    # ln -s ../Transceiver52M/transceiver_usrp_with_serial ./transceiver 

== Execute (select first UHD Device, if you have more than one USRP board)
    
    # cd /usr/local/src/
    # ./startUSRP --device 0


