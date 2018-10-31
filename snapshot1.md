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

================================

**Create VM using CLI**

	virt-install --virt-type=kvm --name cli --ram 2048 --vcpus=2 --os-variant=ubuntu14.04 --cdrom=/home/admin1/Downloads/ISO/ubuntu-14.04.5-desktop-amd64.iso --disk path=/var/lib/libvirt/images/cli.qcow2,size=30

================================

**Snapshots**
 
*To list snapshots*

	virsh snapshot-list --domain vm_name


*To create snapshot*

	virsh snapshot-create-as --domain vmname --name "snap1"

*Display the snapshot details*

	virsh snapshot-info --domain vmname --snapshotname snap1

*Restore the snapshot*

	 virsh snapshot-revert --domain vmname --snapshotname snap1 --running

*Delete the snapshot*

	virsh snapshot-delete --domain vmname --snapshotname snap1

===============================

**Clone**

*To create clone*

	virt-clone --original vmname --name clone1 --auto-clone

				OR

If you want to change the default location try this

	virt-clone --original ubuntu14.04desktop --name clone2 --file=/home/admin1/clone2.qcow2







