# Create Instance Template
## Operation Steps
1.  Access [Instance Template Console](https://cns-console.jdcloud.com/host/launchtemplate/list) and enter the instance template list page. Or access [JD Cloud Console](https://console.jdcloud.com) Click navigation bar on the left **Elastic Compute** - **Virtual Machines** - **Instance Template** to enter the instance template list page.
2. Please note that instance templates cannot be used across regions when selecting a region, and the instance templates in the cn-south-1 region are not supported for creating VM instances. If the quota of the selected region is full, it can be increased by open ticket.
3. Click **Create** to enter the instance template creation page.
4. Set the instance template name and description.
5. Select Images:
	The image is divided into cloud system disk image and local system disk image, the former only supports to create the instance with the system disk of cloud disk, and the latter only supports the instance with the system disk of local disk.
	
	In addition, each type of image is divided into the following image types:
	
	* Public Image: It is provided and maintained by JD Cloud, which provides basic operating system, initialization components and some pre-installed software, and supports multiple released versions of Linux and Windows, thus it is available to all users.
	* Private Image: A customized image created based on the user's Virtual Machines. You can create the image for a machine deployed with businesses and quickly create multiple machines with the same configuration and software environment based on this image. You can share the private image with other JD Cloud users, and the shared image will be displayed in the shared image list of the target user in the same region. The private image supports deletion and basic information modification.
	* Shared Image: A customized image shared by other JD Cloud users through the image sharing function, the display sector is the same as the shared private image. The shared image can only be used to create a Virtual Machines, while the basic information modification and deletion cannot be performed. If the shared user cancels the sharing, the image will be automatically deleted from the list.
	* Marketplace Image: A image provided by the service providers in the cloud marketplace, integrating the running environment or software for different business scenarios to make it easy for users to quickly deploy their businesses.
	
	Users first using JD Cloud can choose the "Public Image" provided by JD Cloud. You can choose the corresponding system and the appropriate version as required. If you have created your own instance and configured the appropriate environment, you can create a private image for this instance, create a batch of instances based on this image with the same system and environment configuration, and share this private image with other JD Cloud users.


6. Type Selection: JD Cloud instance type supports customized selection: Users can select instance types and corresponding configurations according to different business scenarios, please refer to [Instance Type] for details().

		Note: If you plan to use the instance template to create an  Availability Group, you should select a second generation type, such as g.n2.large when creating the instance template.

7. Storage Selection: JD Cloud provides the cloud disk and local disk.
	
	Cloud Disk Service: Adopt a multi-backup and distributed storage mode with high data reliability;
	
	Local Disk: A storage device on a physical machine where the cloud server is located. It can obtain a lower delay, but there is a risk of single point data loss.

	Virtual Machines System Disk: Support local disk and cloud disk, where the local disk has 40GB for free, and the capacity cannot be changed. The cloud disk supports 40GB~500GB.

	Virtual Machines Data Disk: If the system disk is a local disk, it supports attaching 8 data disks. If the system disk is a cloud disk, it supports attaching 7 data disks. The data disk can be premium Hdd cloud disk and SSD cloud disk. After the cloud disk is attached to the Virtual Machines, it requires entering the Virtual Machines operating system to attach the cloud disk.

		Premium Hdd Cloud Disk as a Data Disk: Support range 20G~3000G

		SSD Cloud Disk as Data Disk: Support range 20G~1000G

	You can create an empty disk of the specified type and capacity with the instance, or you can create a data disk based on an existing cloud disk snapshot (the type of data disk created based on the snapshot does not support adjustment for the time being, and the capacity adjustment range is limited within the snapshot capacity). If the private image you choose contains preset data disk configuration information, then the data disk will be automatically configured according to the device mapping information in the image after selecting a private image. If you only want to use part of the default configuration of the image, you can also delete it (do not support replacing the snapshot on the default configuration, if you want to replace the snapshot, please select the snapshot after deleting the default configuration and adding a new disk separately). For the assignment rules of data disk device name, please refer to [ Assignment Rules](../Storage/Assign-Device-Name.md).

8. Select Network: <br> Select **Virtual Private Cloud** and **Subnet**. After selecting the subnet, you can determine the number of Virtual Machines that can be created under this subnet. If there is no subnet, you can create a new subnet through the quick portal and select it in the **Virtual Machines Network**. Please refer to the Virtual Private Cloud and subnet for details.        

	Select the corresponding created security group, the security group is required, you can select from the default security group or create it from a quick portal, please refer to [Security Group Creation](../Security-Group/Create-Security-Group.md) for details, and select in the "Network" configuration after creation.


9. Select Public Network Bandwidth:

	The EIP bandwidth provided by JD Cloud is charged by fixed bandwidth and by traffic, the former is charged according to the bandwidth upper limit set when purchasing, which is not related to the current real-time access to the public network bandwidth of your instance, and the latter is charged according to the actual traffic of your real-time access to the public network.
    Public network lines are divided into BGP and non-BGP, and BGP is provided for a faster and more efficient network access.        

	Bandwidth Range: 1Mbps~200Mbps, it can temporarily not be configured with EIP, and you could purchase the associate it after creating instance by instance template.

	The EIP bandwidth cost is independent of the instance cost. Please refer to [Public IP Price]() for detailed price information.
**Please note that the instance template service is free of charge. The following billing information is estimated when creating VM instance by instance template.**
	<table>
	   <tr>
	      <td >Instance Billing Methods</td>
	      <td >Public Network Bandwidth Billing Methods</td>
	      <td >Cost Estimation</td>
	   </tr>
	   <tr>
		   <td rowspan="2">Pay By Configuration  </td>
	      <td >By Fixed Bandwidth </td>
	      <td > The configuration cost includes instance type (CPU and memory configuration), the Cloud Disk Service (if configured) and the elastic IP bandwidth.</td>
	   </tr>
	   <tr>
	      <td >By Traffic </td>
	      <td > Public Network Traffic Cost + Configuration Cost. Among them, the configuration cost includes instance type (vCPU and memory configuration), the Cloud Disk Service (if configured) and the elastic IP configuration.           </td>
	   </tr>
	   <tr>
		   <td rowspan="2">Monthly Package  </td>
	      <td >By Fixed Bandwidth </td>
	      <td > The configuration cost includes instance type (vCPU and memory configuration), the Cloud Disk Service (if configured) and the elastic IP bandwidth.</td>
	   </tr>
	   <tr>
	      <td >By Traffic </td>
	      <td > Among them, the configuration cost includes instance type (CPU and memory configuration) cost, the cost of the Cloud Disk Service (if configured) and the elastic IP configuration.     </td>
	   </tr>
   </table>

10. Configure Login Information:

	The login user name of Windows system is administrator;  the login user name of Linux system is root;    
    
	You can select **Set Now** or **Set Later** during the login password setting, if you choose "Set Later", the system will send a random password by SMS and mail when using the starting template to create VM instance. We recommend that you could change the password through the console after VM instance creation.    
    
	For Linux system, you can also choose the key pair to login and associate the created key pair to conduct safer instance login verification. Please refer to [Key Pair](../Key-Pair/KeyPair-Overview) for detailed key pair creation and login help.


The instance template service is free, the information you have configured is displayed on the right side of the create page, and the estimated cost of creating an instance based on the starting template and your configuration is also displayed: Including monthly cost in the case of monthly package and hourly cost in the case of billing by configuration. It includes the cost of the Cloud Disk Service and the EIP cost with billing by fixed bandwidth, the EIP cost should be listed separately if you choose billing by traffic.

## Related Reference

[Public IP Price]()

[Instance Type]()

[Key Pair](../Key-Pair/KeyPair-Overview)

[Key Pair]()