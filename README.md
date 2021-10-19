# Part 6: TBN


[Tutorial Menu](https://github.com/pslucas0212/RedHat-Satellite-VM-Provisioning-to-vSphere-Tutorial)  

### Steps to update
1. Infrastructure -> capsule add org and location this makes cotent source
2. done - hosts -> operating systems
3. Create Host Group
4. Create compute profile - see instrutions below
5. Administer -> Settings -> Destroyed associated VM on host delete -> change to yes
6. Example deployment

### Integrating the VMWare Resources with Satellite. 

We are going pre-define hardware settings for a virtual machine in Satellite by creating a compute profile.  On the Satellite Console chose Infrastructure -> Compute Profiles.

![Infrastructure -> Compute Profiles](/images/sat44.png)

On the Compute Profiles page, click the blue Create Compute Profile button.  

![Click the blue Create Compute Profile button](/images/sat45.png)

On the Compute Profiles > Create Compute Profile, enter cp-vmware-small for the Name and click the blue Submit button.  

![Name Compute Profile](/images/sat46.png)  

On the Compute Profiles > cp-vmware-small page click the cr-vcenter link to define the new compute profile cp-vmware-small.  

![Click cr-vcenter link](/images/sat47.png)

We will now define the configuration of the RHEL VM that will be provisioned to VMWare from Satellite.  We will mostly accept default values.  Configuration changes are listed in the followng table.  

Name | Value
---- | -----
Cluster | LabCluster
Guest OS | Red Hat Enterprise Linux 8 (64 bit)
Virtual H/W version | 14 (EXSi 6.7)
Image | img-rhel8-prem-server
Create SCSI controller | VMware Paravirtual
Datastore | LabDatastore
Size(GB) | 20 GB
Thin Provision | Uncheck
NIC type | VNXNET3
Network | VM Network

The next two screen shots show what your configuration should look like.  Click the blue Submit button when you have finished with the configuration.  You will be returned to the  Compute Profiles > cp-vmware-small page.  

![cr-vmware-small config](/images/sat48.png)
![cr-vmware-small config continued](/images/sat49.png)

We finish up this session by creating a hosgroup. 

Host Group Tab:
Name | Value
---- | -----
Name | hg-rhel8-prem-server
Lifecycle Environment | le-ops-rhel8-prem-server
Content View | cv-rhel8-prem-server
Deploy On | cr-vcenter
Compute Profile | cp-vmware-small

Networkt Tab:
Name | Value
---- | -----
Domain | example.com
IPv4 Subnet | cn-operations-department

Operating System Tab:
Name | Value
---- | -----
Architecture | x86_64
Operating System | RedHt 8.3
Password | Passw0rd!

Check the Locations and Organziations tab to make sure that moline is set for Locations and Operations Department is set for Organizations.  

Set the activation key to ak-ops-rhel8-prem-server


```
# hammer hostgroup create \
--name hg-rhel8-prem-server \
--operatingsystem "Red Hat 8.3" \
--subnet sn-operations-department \
--domain example.com \
--architecture x86_64 \
--compute-profile cp-vmware-small \

```

## References  
[Installing Satellite Server from a Connected Network](https://access.redhat.com/documentation/en-us/red_hat_satellite/6.9/html/installing_satellite_server_from_a_connected_network/index)   
[Simple Content Access](https://access.redhat.com/articles/simple-content-access)  
[Provisioning VMWare using userdata via Satellite 6.3-6.6](https://access.redhat.com/blogs/1169563/posts/3640721)  
[Understanding Red Hat Content Delivery Network Repositories and their usage with Satellite 6](https://access.redhat.com/articles/1586183). 
[CHAPTER 11. PROVISIONING VIRTUAL MACHINES IN VMWARE VSPHERE](https://access.redhat.com/documentation/en-us/red_hat_satellite/6.9/html/provisioning_guide/provisioning_virtual_machines_in_vmware_vsphere#Provisioning_Virtual_Machines_in_VMware_vSphere-Creating_a_VMware_vSphere_User)  
[What user permissions/roles are required for the VMware vCenter user account to provision from Satellite 6.x?](https://access.redhat.com/solutions/1339483)

