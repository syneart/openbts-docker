Docker container with N210/B210/B200-miniRequirementsDocker 1.9.1 (aufs)====================================---------------------------------------    # docker infoServer Version: 1.9.1<br>Storage Driver: aufs<br>Root Dir: /var/lib/docker/aufs<br>　Backing Filesystem: extfs<br>　Dirs: 31<br>　Dirperm1 Supported: true<br>Execution Driver: native-0.2<br>Logging Driver: json-file<br>Kernel Version: 4.2.0-19-generic<br>Operating System: Ubuntu 15.10<br>CPUs: 1<br>
---------------------------------------Ubuntu 14.04 LTS (or later) <br>OpenBTS 2.9 (c438a5a689b32539d1c4ce8dd607d0231451faa9)<br>gnuradio-3.7.8.1 (or later)<br>Asterisk-11.7<br>UHD-3.9.0 (or later)<br>Ortp-0.20.0---------------------------------------2016/12/29 update= Enable GPRS
>\> config GPRS.Enable 1 <br>>\> config GGSN.DNS 8.8.8.8 <br>    # apt install iptables <br>    # iptables-restore < /usr/local/src/openbts/apps/iptables.rules= DEBUG MESSAGE ENABLE IN CONTINTER    # apt install syslogd    # /bin/busybox syslogd -nSO /proc/1/fd/1 &= External Reference <br>DB: "TRX.Reference" '0' for Internal or '1' for External 10 MHz reference= OpenRegistration>\>  config Control.LUR.OpenRegistration 0= Asterisk Console   
    # asterisk -vvvvvrAfter docker import, you need to
1.    Rebuild Asterisk, skip make samples    
         # ./build.sh         # cd asterisk-11.7.0/         # ./configure && make menuselect && make && make install && make config2.    Rebuild OpenBTS

         # autoreconf -i && ./configure --with-uhd && make3.    startUSRP=first of all you need to have admin privileges  
    
    $ sudo su -
    =setup Docker    
    # curl -sSL https://get.docker.com | sh    # docker run -i -t --name="openbts_usrp_gsm" --privileged -v /dev/bus/usb:/dev/bus/usb ubuntu:14.04 /bin/bash    # apt update    # apt upgrade -y    # apt install vim -y\<Enter Running Container>=Give terimal some color    
    # sed -i 's/#force_color_prompt=yes/force_color_prompt=yes/g' '/root/.bashrc'=UHD    
    $ apt install -y libboost-all-dev libusb-1.0-0-dev python-mako doxygen python-docutils cmake build-essential wget python-requests~Download UHD-(3.9.0)    
    # cd /usr/local/src    # wget https://github.com/EttusResearch/uhd/archive/release_003_009_000.tar.gz    # tar zxvf release_003_009_000.tar.gz    # cd uhd-release_003_009_000/host/    # mkdir build    # cd build    # cmake ../    # make    # make test    # make install    # ldconfig=Download uhd images    
    # /usr/local/lib/uhd/utils/uhd_images_downloader.py=Test    
    # uhd_usrp_probe=GNURadio    
    # apt install -y libfontconfig1-dev libxrender-dev libpulse-dev swig g++ \
    automake autoconf libtool python-dev libfftw3-dev libcppunit-dev \
    libboost-all-dev libusb-dev libusb-1.0-0-dev libsdl1.2-dev python-wxgtk2.8 \
    git-core guile-1.8-dev libqt4-dev python-numpy ccache libgsl0-dev \
    python-cheetah python-lxml doxygen qt4-dev-tools libqwt5-qt4-dev \
    libqwtplot3d-qt4-dev pyqt4-dev-tools python-qwt5-qt4 cmake git-core wget \
    libxi-dev python-docutils gtk2-engines-pixbuf r-base-dev python-tk \
    libasound2-dev python-gtk2
    ~ Download Volk  (1.1.1)    
    # cd /usr/local/src    # wget https://github.com/gnuradio/volk/archive/v1.1.1.tar.gz    # tar zxvf v1.1.1.tar.gz    # cd volk-1.1.1/    # mkdir build    # cd build    # cmake ../    # make    # make install    # ldconfig~ Download GNURadio  (3.7.8.1)    
    # cd /usr/local/src    # wget https://github.com/gnuradio/gnuradio/archive/v3.7.8.1.tar.gz    # tar zxvf v3.7.8.1.tar.gz    # cp -r volk-1.1.1/* ./gnuradio-3.7.8.1/volk/    # cd gnuradio-3.7.8.1/    # mkdir build    # cd build    # cmake ../    # make    # make test    # make install    # ldconfig=liba53    
    # cd /usr/local/src    # git clone https://github.com/RangeNetworks/liba53    # cd liba53    # make     # make install    # make installtest=google-coredumper (1.2.1)    
    # cd /usr/local/src    # git clone https://github.com/RangeNetworks/libcoredumper    # cd libcoredumper/    # ./build.sh    # cd coredumper-1.2.1    # make install    # ldconfig=ortp (0.20.0)    
    # cd /usr/local/src    # wget https://github.com/BelledonneCommunications/ortp/archive/0.20.0.tar.gz    # tar zxvf 0.20.0.tar.gz    # cd ortp-0.20.0/    # autoreconf -i    # ./configure    # make     # make install=openBTS (2.9) [c438a5a689b32539d1c4ce8dd607d0231451faa9]    
    # cd /usr/local/src    # git clone --recursive https://github.com/RangeNetworks/openbts    # cd openbts    # ./NodeManager/install_libzmq.sh    # autoreconf -i    # ./configure --with-uhd    # apt install libsqlite3-dev sqlite3    # make    # cd apps/    # ln -s ../Transceiver52M/transceiver .    # mkdir /etc/OpenBTS    # sqlite3 -init ./OpenBTS.example.sql /etc/OpenBTS/OpenBTS.db ".quit"=subscriberRegistry (Registr)    
    # cd /usr/local/src    # git clone --recursive https://github.com/RangeNetworks/subscriberRegistry    # cd subscriberRegistry/    # apt install libosip2-dev    # autoreconf -i    # ./configure    # make    # sqlite3 -init ./apps/sipauthserve.example.sql /etc/OpenBTS/sipauthserve.db ".quit"    # mkdir -p /var/lib/asterisk/sqlite3dir=Smqueue (Receive SMS)    
    # cd /usr/local/src    # git clone --recursive https://github.com/RangeNetworks/smqueue    # cd smqueue/    # autoreconf -i    # ./configure    # make    # sqlite3 -init ./smqueue/smqueue.example.sql /etc/OpenBTS/smqueue.db ".quit"    # mkdir -p /var/lib/OpenBTS/=fuser    
    # apt install psmisc=Asterisk (11.7.0)    
    # cd /usr/local/src    # git clone https://github.com/RangeNetworks/asterisk    # cd asterisk/    # apt install unixodbc-dev libssl-dev libsrtp0-dev libsqlite0-dev libsqliteodbc    # ./build.sh    # cd asterisk-11.7.0/    # ./configure && make menuselect && make && make install && make config && make samples=SCRIPT     
    # vi /usr/local/src/startUSRP    # chmod +x /usr/local/src/startUSRP    # alias startUSRP='/usr/local/src/startUSRP'    # echo alias startUSRP='/usr/local/src/startUSRP' >> ~/.bashrc    # startUSRP=================================Extra<br>=usrp ip (N210) :    
    $ sudo '/home/rb507-1/Downloads/UHD-Mirror-release_003_004_002/host/utils/usrp2_recovery.py' --ifc=eth0 --new-ip=192.168.10.2
        $ uhd_find_devices --args addr=192.168.10.2    $ nohup----commit docker's container
	sudo docker commit -m="ORAL" -a="SyA" ec07a43b236a usrp/openbts_oral