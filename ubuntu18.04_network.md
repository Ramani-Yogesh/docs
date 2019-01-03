================================================

   ##Ubuntu18.04_Networking

================================================

* **Install NetworkManager**

        sudo apt install network-manager

* **Open and edit the network configuration file**

        sudo vim /etc/netplan/50-cloud-init.yaml

*__For Dynamic IP:__*

    network:
        ethernets:
            enp0s3:
                    addresses: []
                    dhcp4: yes
        ethernets:
                enp0s8:
                        addresses: []
                        dhcp4: yes
        version: 2
        renderer: NetworkManager

*__For Static IP:__*

    network:
        ethernets:
            enp0s3:
                addresses: [192.168.56.6/24]
                gateway4: 192.168.1.1
                nameservers:
                addresses: [8.8.8.8,8.8.4.4]
                dhcp4: no
        ethernets:
                enp0s8:
                        addresses: []
                        dhcp4: yes
        version: 2
        renderer: NetworkManager
    

* **Restart the service**

        sudo netplan apply
        sudo /etc/init.d/networking restart 
        sudo /etc/init.d/network-manager restart

* **Check the IP**

        ifconfig
