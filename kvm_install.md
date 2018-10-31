##_Bridge Configuration_

* **Install bridge**

       apt install bridge-utils

* open and add the following details to interface file

**For Static IP**

    vim /etc/network/interfaces

    # Bridge Information #
    auto br0
    iface br0 inet static
    bridge_ports eno1 

    # Bride IP #
    address 192.168.12.7
    netmask 255.255.255.0
    network 192.168.12.0
    broadcast 192.168.12.255
    gateway 192.168.12.2
    dns-nameservers 192.168.12.2
    dns-nameservers 8.8.8.8

Save and close the file

**For Dynamic IP**

    vim /etc/network/interfaces

    # Bridge Information #
    auto br0
    iface br0 inet dhcp
    bridge_ports eno1 

* Restart the network service

      service networking restart 
      service network-manager restart

		OR

      reboot

* Verify

      ifconfig
    
    OR 
     
      ip a s

=======================================================

**KVM Installation**

    apt-get install -y qemu-kvm qemu-system  virt-manager virt-viewer libvirt-bin

* Open virtual machine manager

      virt-manager &
      
=======================================================

#Virsh Commands

*To list all vm*
    
     virsh list --all

*To list running vm*

     virsh list

*To view the vm details*

     virsh dominfo test

*To check the status of vm*

     virsh domstate test

*To check domain id*

     virsh domuuid test

*Start the vm*

     virsh start vm_name	

*Shutdown the vm*

      virsh shutdown vm_name

*Restart the vm*

     virsh reboot vm_name
 
*To pause vm*

    virsh suspend vm_name

*To resume vm*

     virsh resume vm_name


