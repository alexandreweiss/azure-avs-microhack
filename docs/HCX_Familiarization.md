Challenge 2
"HCX Familiarisation"
---

# Introduction

In this challenge, you will perform the following tasks:

1.	Configure HCX Manager Appliance On-Prem
2.	Configure Site Pair
3.	Create Network Profile
4.	Create Compute Profile
5.	Deploy Service Mesh
6.	Extend Network 
7.	Perform Migration of a VM on an extended network
8.	Perform Migration of a VM on an un-extended network

Please carefully follow the instructions provided by your facilitator. Incorrectly deploying the HCX may result in multiple forthcoming steps not operating as expected.

Work with the instructor to ensure your VMware environment has the required permissions to access your AVS vCenter Server.

## Configure HCX Manager Appliance On-Prem

1.	Log in to the On Prem SDDC by login to your Azure jumpbox and by navigating to portal.azure.com. Log on to the jumpbox using the Bastian host and key in the username and password provided  within the AVS "Credentials&IP" document identified for your team

2.	Log on to your On-Prem vCenter using the "Credentials&IP" document

 ![](/Images/HCX/HCX_image3.png)

3.	Confirm that the vCenter server has hcx-manager deployed and powered on.

  ![](/Images/HCX/HCX_image4.png)

4.	Log on to the AVS private Cloud for your team in Azure Portal from where you will need to get a activation key for the HCX manager On-Prem

![](/Images/HCX/HCX_Image5.1.png)

5.	In the Azure VMware Solution portal, go to Manage > Add-ons > Migration using HCX > Connect with on-premise using HCX keys > Add > , specify the HCX Key Name (example as shown in the screenshot), and then select Add.

 ![](/Images/HCX/HCX_Image5.2.png)

6.	Use the admin credentials to sign in to the on-premises VMware HCX Manager at https://HCXManagerIP:9443. Use the "Credentials&IP" doc for this

### TIP
The admin user password is set during the VMware HCX Manager OVA file deployment.

7.	In Licensing, enter your key for HCX Advanced Key and select Activate.

![](/Images/HCX/HCX_image7.png)

### Important TIP
VMware HCX Manager must have open internet access or a proxy configured.

8.	In Datacentre Location, specify Chicago, Unted States of America and press continue

![](/Images/HCX/HCX_image8.png)

9.	In System Name, modify the name or accept the default and select Continue.

 ![](/Images/HCX/HCX_image9.png)

10.	Select Yes, Continue.

 ![](/Images/HCX/HCX_image10.png)

11.	In Connect your vCenter, provide the FQDN or IP address of your vCenter server and the appropriate credentials, and then select Continue. Use the "Credentials&IP" document for this

![](/Images/HCX/HCX_image11.png)

12. In Configure SSO/PSC, provide the FQDN or IP address of your Platform Services Controller (PSC), and then select Continue. In this case the the PSC is the same as the On-Prem vCenter server. Use the "Credentials&IP" document for the same

 ![](/Images/HCX/HCX_image12.png)

13. Verify that the information entered is correct and select Restart.

![](/Images/HCX/HCX_image13.png)

### Note
You'll experience a delay after restarting before being prompted for the next step.

After the services restart, you'll see vCenter showing as green on the screen that appears. Both vCenter and SSO must have the appropriate configuration parameters, which should be the same as the previous screen.

14.	Once HCX Appliance is restarted, log on to the HCX Manager UI – https://hcxmanagerIP:9443

15.	Go to Configuration -> vSphere Role Mapping -> replace System Administrator and Enterprise Administrator user groups with the following custom domain (instead of vsphere.local). 

Replace the domain name according to the group you have been assigned- microhack-one.zpod.io, Microhack-two.zpod.io or Microhack-three.zpod.io

 ![](/Images/HCX/HCX_image14.png)

## Configure Site Pairing

Now you're ready to add a site pairing, create a network and compute profile, and enable services such as migration and network extension.

## Add a site pairing
You can connect or pair the VMware HCX Cloud Manager in AVS with the VMware HCX Connector in your On-Prem datacenter.

1.	Sign in to your on-premises vCenter, and under Home, select HCX.

2.	Under Infrastructure, select Site Pairing, and then select the Connect To Remote Site option (in the middle of the screen).

 ![](/Images/HCX/HCX_image15.png)

3.	Enter the Azure VMware Solution HCX Cloud Manager URL or IP address, username and password to intiate the site pairing. Use the "Credentials&IP" doc for the same

 ![](/Images/HCX/HCX_image16.png)

 ### Note

To successfully establish a site pair:
Your VMware HCX Connector must be able to route to your HCX Cloud Manager IP over port 443.

You'll see a screen showing that your VMware HCX Cloud Manager in Azure VMware Solution and your on-premises VMware HCX Connector are connected (paired).

![](/Images/HCX/HCX_image17.png)

## Create Network Profiles

VMware HCX Connector deploys a subset of virtual appliances (automated) that require multiple IP segments. When you create your network profiles, you use the IP segments that have been identified during the VMware HCX Network Segments pre-deployment preparation and planning stage.

### Note

Generally in a customer scenario we create multiple network profiles for the networks below

#### Management	
#### vMotion
#### Replication
#### Uplink


For this MicroHack, we will be using the same network profile for all the four networks

1.	Under Infrastructure, select Interconnect > Multi-Site Service Mesh > Network Profiles > Create Network Profile.

![](/Images/HCX/HCX_image18.png)

2.	For each network profile, select the network and port group, provide a name, and create the segment's IP pool. Then select Create. Please refer to the Credentials&IP document for the details for the IP addresses to be used
 
![](/Images/HCX/HCX_image19.png)

3.	Once done, the network profile created by you will be available to be used by the Interconnect and Network Extension appliances within the Service Mesh

![](/Images/HCX/HCX_image20.png)

## Create a Compute Profle

1.	Under Infrastructure, select Interconnect > Compute Profiles > Create Compute Profile.

![](/Images/HCX/HCX_image21.png)

5.	Enter a name for the profile and select Continue.

![](/Images/HCX/HCX_image22.png)

6.	Select the services to enable, such as migration, network extension, or disaster recovery, and uncheck the WAN Optimization, SRM and OS Assisted Migration and then select Continue.

![](/Images/HCX/HCX_image23.png)

### Note 
Generally the type of services greyed out will depend on the type of HCX licensing type used.  

7.	When you see the clusters in your on-premises datacenter, select Continue.

8.	From Select Datastore, select the datastore storage resource for deploying the VMware HCX Interconnect appliances. Then select Continue.

![](/Images/HCX/HCX_image25.png)

9.	From Select Management Network Profile, select the management network profile that you created in previous steps. Then select Continue.

![](/Images/HCX/HCX_image26.png)

10.	From Select Uplink Network Profile, select the uplink network profile you created in the previous procedure. Then select Continue.

![](/Images/HCX/HCX_image27.png)

11.	From Select vMotion Network Profile, select the vMotion network profile that you created in prior steps. Then select Continue.

![](/Images/HCX/HCX_image28.png)

12.	From Select vSphere Replication Network Profile, select the replication network profile that you created in prior steps. Then select Continue.

![](/Images/HCX/HCX_image29.png)

13.	From Select Distributed Switches for Network Extensions, select the switches that contain the virtual machines to be migrated to Azure VMware Solution on a layer-2 extended network. Then select Continue.

![](/Images/HCX/HCX_image30.png)

14.	Review the connection rules and select Continue.

![](/Images/HCX/HCX_image31.png)

15.	Select Finish to create the compute profile.

![](/Images/HCX/HCX_image32.png)
 
16.	One the On Prem Compute profile has been created the Compute profile will be listed as below

![](/Images/HCX/HCX_image33.png)


## Create a service mesh

Now it's time to configure a service mesh between on-premises and Azure VMware Solution private cloud.

### Note
To successfully establish a service mesh with Azure VMware Solution:
Ports UDP 500/4500 are open between your on-premises VMware HCX Connector 'uplink' network profile addresses and the Azure VMware Solution HCX Cloud 'uplink' network profile addresses.
Be sure to review the VMware HCX required ports.

1.	Under Infrastructure, select Interconnect > Service Mesh > Create Service Mesh.

![](/Images/HCX/HCX_image34.png)

2.	Review the sites that are pre-populated, and then select Continue.

![](/Images/HCX/HCX_image35.png)

### Note
If this is your first service mesh configuration, you won't need to modify this screen

3.	Select the source and remote compute profiles from the drop-down lists, and then select Continue.

The selections define the resources where VMs can consume VMware HCX services.

![](/Images/HCX/HCX_image36.png)

4.	Review services that will be enabled, and then select Continue.

![](/Images/HCX/HCX_image37.png)

5.	In Advanced Configuration - Override Uplink Network profiles, select Continue.

![](/Images/HCX/HCX_image38.png)

### Note
Uplink network profiles connect to the network through which the remote site's interconnect appliances can be reached

6.	In Advanced Configuration - Network Extension Appliance Scale Out, review and select Continue.

![](/Images/HCX/HCX_image39.png)

### Note 
You can have up to eight VLANs per appliance, but you can deploy another appliance to add another eight VLANs. You must also have IP space to account for the more appliances, and it's one IP per appliance. For more information, see VMware HCX Configuration Limits.

7. In Advanced Configuration - Traffic Engineering, do not select the Application Path Resiliency and Traffic Flow Conditioning, and then select Continue.

![](/Images/HCX/HCX_image40.png)

8.	Review the topology preview and select Continue.

![](/Images/HCX/HCX_image41.png)

9.	Enter the name for this HCX-Microhack-ServiceMesh and select Finish to complete.

![](/Images/HCX/HCX_image42.png)

9.	Select View Tasks to monitor the deployment.
 
When the service mesh deployment finishes successfully, you'll see the services as green.

10.	Verify the service mesh's health by checking the appliance status.

![](/Images/HCX/HCX_image43.png)

11.	Select Interconnect > Appliances.

![](/Images/HCX/HCX_image44.png)

## Extend Network
In this step you will extend any the on-premises environment to Azure VMware Solution.

1.	Under Services, select Network Extension > Create a Network Extension.

2.	Select each of the networks you want to extend to Azure VMware Solution, and then select Next

3.	Enter the on-premises gateway IP for each of the networks you're extending, and then select Submit.

![](/Images/HCX/HCX_image45.png)

The IP address to be used and extended is defined in the IP address / Login document

It takes a few minutes for the network extension to finish. When it does, you see the status change to Extension complete.

## Next steps
If the HCX interconnect tunnel status is UP and green, you can migrate and protect Azure VMware Solution VMs by using VMware HCX. Azure VMware Solution supports workload migrations (with or without a network extension). You can still migrate workloads in your vSphere environment, along with on-premises creation of networks and deployment of VMs onto those networks.


## Migrate a VM

1.	To migrate a virtual machine from and On Prem Environment to AVS, sign in to your on-premises vCenter, and under Home, select HCX.

2.	Under Services, select Migration, and then select the Migrate

![](/Images/HCX/HCX_image46.png)

3.	One the Workload Mobility window is opened, ensure your site pairing is available from On Prem to AVS. 

4.	Select mhack-tinycore1 as a VM that will be migrated from On-Prem to AVS and press Add 

![](/Images/HCX/HCX_image47.png)

5.	Once the virtual machine is added, select the transfer and placement parameters for the virtual machine post migration to AVS and then press validate
 
![](/Images/HCX/HCX_image48.png)

6.	Once the transfer and placement validation of the virtual machine has gone through, press go for the migration of the virtual machine


![](/Images/HCX/HCX_image49.png)
 
7.	Once the VM is migrated into AVS, check the IP address of the VM. 

Note : 

As the VM that was migrated was on a extended network, the IP address of the VM has not changed; however if the VM that was migrated was not on an extended network, then the IP address of the VM would have changed. 


This concludes the HCX familiarisation for AVS!!


