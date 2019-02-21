###Nagios-NRPE Installation in Ubuntu 18.04


1. Add the below line to **/etc/apt/sources.list** file

        sudo  vim /etc/apt/sources.list

        -------
        -------
        deb http://in.archive.ubuntu.com/ubuntu/ bionic main universe
        
2. Update the package list

        sudo apt-get update
        
3. Now you can install the nagios-nrpe

        sudo apt-get install nagios-nrpe-server nagios-plugins
        
4. Check the plugins are installed

        ls /usr/lib/nagios/plugins/
