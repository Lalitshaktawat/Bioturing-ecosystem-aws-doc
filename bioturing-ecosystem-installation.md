<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/bbrowser_logo.png" class="lazy" width="30%"><br><br>

# <p> <span style="color:blue"> BioTuring Ecosystem </span> <span style="color:green"> Installation Technical Documentation </span> </p>
 
 
## Introduction

The BioTuring Ecosystem is a collection of web-based applications designed to assist scientists in analyzing single-cell RNA sequencing (scRNAseq) and spatial omics data.
This system supports proprietary and publicly accessible data. Users have the flexibility to access the ecosystem through the BioTuring cloud or deploy it on a local, on-premises server.

This documentation describes the technical requirements to deploy the ecosystem on a local, on-premises server and the high-level architecture designs of the ecosystem.

## System requirements

| **Resources** | **Basic recommendation**                                         | **Detail**                                                                                            |
|---------------|------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| CPU           | <strong style="color:red;">32 core / 64 cores </strong>          | This is basic requirement to start BioTuring Ecosystem and based on requirement.                      |
| RAM           | <strong style="color:red;">64 GB / 128 GB</strong>               | As above                                                                                              |
| HDD           | / partition can be 100 GB                                        | As above.                                                                                             |
|               | Data Volume : 500GB                                              | As above.                                                                                             |
| OS            | Any OS. Ubuntu 20.04 and above.                                  | BioTuring Ecosystem is more supportive with Linux OS. For better performance linux OS is recommended. |
| AWS Instance  | <strong style="color:red;"> One 16 GB NVIDIA GPU</strong>        | AWS **g5.8xlarge**                                                                                    |
| Platform      | Docker / Kubernetes                                              |                                                                                                       |

:bell: **NOTE**: Every analysis in the BioTuring Ecosystem is GPU-based. Therefore, the hosting server must have at least **one NVIDIA GPU** with Turing architecture or above.

| **Security**        |                                                                                                                  |
|---------------------|------------------------------------------------------------------------------------------------------------------|
|                     | The BioStudio platform uses HTTPS protocol to securely communicate over the network.                             |
|                     | All of the users need to be authenticated using a BioTuring account or the company's SSO to access the platform. |
|                     | We highly recommend setting up a private VPC network for IP restriction.                                         |
|                     | The data stays behind the company firewall.                                                                      |


| **Data visibility** |                                                                                                     |
|---------------------|-----------------------------------------------------------------------------------------------------|
|                     | Data can be uploaded to Personal Workspace or Data Sharing group.                                   |
|                     | In the Personal Workspace, only the owner can able to see and manipulate the data she/he uploaded.  |
|                     | In the Data Sharing group, only people in the group can able to see the data.                       |
|                     | In the Data Sharing group, only people with sufficient permissions can able to manipulate the data. |

## Network requirements

| **Protocol** | **Open Port for application** |
|--------------|:-----------------------------:|
| HTTP         |               **80**              |
| HTTPS        |              **443**              |

|        **Domain**        |                            **Explain**                           |
|:------------------------:|:----------------------------------------------------------------:|
| **54.203.5.109**         | This is the authentication server to check for license.          |
| **216.105.40.75**        | This is a BioTuring centralized server for accessing public data |
| **s3://talk2dataupdate** | This is a bucket to download new updates of the applications.    |

## Nvidia explore

Feel free to explore more detail related to Nvidia drivers.

[NVIDIA CUDA Toolkit version 11.7](https://developer.nvidia.com/cuda-11-7-0-download-archive?target_os=Linux)

[NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)

:point_right: **Please contact** :email: [support@bioturing.com](mailto:support@bioturing.com) to get the **token** for your company.

## Reverse Proxy

BioTuring ecosystem is compatible with Nginx, load balancers (LB), and various types of proxies, allowing seamless access to it.

# Installation

## BioTuring ecosystem server setup for Docker engine

The BioTuring ecosystem is designed to seamlessly support all cloud service providers, on-premises private servers, and diverse infrastructure setups, guaranteeing compatibility across a multitude of architectures.

:bell: Before creating the instance, we'll make sure the network setup is prepared. For the purpose of illustration, we'll be using AWS cloud services for this installation.

:large_orange_diamond: Logging into your AWS account and ensuring that the necessary permissions are added to your Role-Based Access Control (RBAC) policy for creating instances

### Create a VPC

:high_brightness: Switch to VPC service and click on **Create VPC** button.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/crv.png" class="lazy" width="100%"><br><br>

:high_brightness: Fill all necessary values.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/crv-1.png" class="lazy" width="100%"><br><br>

:high_brightness: Click on **Create VPC**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/crv-2.png" class="lazy" width="100%"><br><br>

### Create Subnets

:high_brightness: Switch to Subnet service and click on **Create subnet** button.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/subnet.png" class="lazy" width="100%"><br><br>

:high_brightness: After completing all required fields, proceed by clicking the **Create subnet** button.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/subnet1.png" class="lazy" width="100%"><br><br>

:bell: **NOTE**: Router table is auto created during subnet creation. We only need to update subnet association. 

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/subnet2.png" class="lazy" width="100%"><br><br>

:large_orange_diamond: **Subnet associations**

:high_brightness: Switch to Router table tab and click on **Subnet associations**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/esa.png" class="lazy" width="100%"><br><br>


:high_brightness: Click on **Explicit subnet associations associations** -- **Edit subnet associations**
:high_brightness: Select **Available subnets** -- **save associations**
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/sa.png" class="lazy" width="100%"><br><br>

:high_brightness: All your subnets are associated with a route table.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/sadone.png" class="lazy" width="100%"><br><br>

### Internet Gateway

:high_brightness: Switch to Internet Gateway and click to **Create internet gateway**

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/ig.png" class="lazy" width="100%"><br><br>

:high_brightness: Fill all the values and click on **Create internet gateway button**

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/cig.png" class="lazy" width="100%"><br><br>

:bell: **NOTE**: Once internet gateway ready. We must **attache** this to VPC by clicking **Action** button.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/avpc.png" class="lazy" width="100%"><br><br>

:high_brightness: Select correct VPC and click to **Attach  internet gateway** button.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/aavpc.png" class="lazy" width="100%"><br><br>

:high_brightness: **VPC attached**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/vpcaa.png" class="lazy" width="100%"><br><br>

:high_brightness: Time to work with Router table to add routes.
:high_brightness: Switch to **Router tables** then click to **Edit routes**.
:high_brightness: Click to **Add route** then click to **Save changes**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/edrt.png" class="lazy" width="100%"><br><br>

:high_brightness: Router table has been updated.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/edrt.png" class="lazy" width="100%"><br><br>

### Security Group

:high_brightness: The same way, Kindly create a security group by allowing inbound rules based on your requirement.

:high_brightness: Switch to **Security Group**.
:high_brightness: Click to **Create security group**.
:high_brightness: Following provides all the necessary values and click to **Create security group**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/sg1.png" class="lazy" width="100%"><br><br>

### instance creation

:high_brightness: Switch to EC2 service and select **instance** tab from left menu.

:high_brightness: Click to **Launch instances**

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/li.png" class="lazy" width="100%"><br><br>

:high_brightness: Provide **name** and select **Operating system**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/ino.png" class="lazy" width="100%"><br><br>

:high_brightness: Select **AMI** and **instance type**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/ino1.png" class="lazy" width="100%"><br><br>

:high_brightness: Create **Key Pair** and saved it on safe location.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/ckp.png" class="lazy" width="100%"><br><br>

:high_brightness: **Edit** network setting and select appropriate values.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/editn.png" class="lazy" width="100%"><br><br>

:high_brightness: **Configure Storage**
:high_brightness: Click to **Launch instance**
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/congs.png" class="lazy" width="100%"><br><br>

:high_brightness: Get connection string. By clicking **connect** button.
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/connect.png" class="lazy" width="100%"><br><br>

:high_brightness: Get **ssh** connection string.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/ssh-conn.png" class="lazy" width="100%"><br><br>

:large_orange_diamond: Connect to the server using SSH key pair to perform installation.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/ssh-conn1.png" class="lazy" width="100%"><br><br>

### Folder structure

:bell: **NOTE**: Before starting the installation, please ensure that the below requirements have been fulfilled and create the path as specified below.

| **Item**                       | **Note**                                       | **Size**     |
|--------------------------------|------------------------------------------------|--------------|
| BioTuring ecosystem token      | To access our product.                         |              |
| Application Domain             | Access BioTuring ecosystem on Browser.         |              |
| /bioturing_ecosystem           | Create default directories to store user data. | 1TB or above |
| /config/ssl                    | Used to store SSL certificate                  |              |

:high_brightness: BioTuring ecosystem **folder structure**.

| **Folder**                     | **Details**          |
|--------------------------------|----------------------|
| /bioturing_ecosystem/app_data  | Application Data     |
| /bioturing_ecosystem/user_data | User Data            |
| /bioturing_ecosystem/script_log| Script execution log |
| /config/ssl                    | SSL Certificate      |

:high_brightness: Make sure that `/dev/shm` size is at least half of physical memory.

To change the configuration for `/dev/shm`, add one line to `/etc/fstab`. 
For example, if the system has 128 GB of physical memory:

```R
# tmpfs /dev/shm tmpfs defaults,size=<Half number of physical memory>g 0 0

tmpfs /dev/shm tmpfs defaults,size=64g 0 0
```

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/shm-e.png" class="lazy" width="100%"><br><br>

Run the command below to make the change immediately:

```R
sudo mount -o remount /dev/shm
```

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/rmount.png" class="lazy" width="100%"><br><br>


:high_brightness: To establish default directories for storing user and application data, it's strongly advised to employ persistent storage. In the commands provided below, we utilize an empty EBS volume as an example.

Kindly use `fdisk -l` or `lsblk` command to find exact partition that needs to be mount for bioturing ecosystem.

```R
sudo mkfs -t ext4 /dev/nvme2n1
sudo mkdir /bioturing_ecosystem
sudo mount /dev/nvme2n1 /data
sudo mkdir -p /bioturing_ecosystem/app_data
sudo mkdir -p /bioturing_ecosystem/user_data
sudo mkdir -p /bioturing_ecosystem/script_log
```

:high_brightness: Generate an SSL certificate comprising two files, precisely named as `tls.crt` and `tls.key`, and place them within the `/config/ss`l directory. 

For example:

```R
sudo mkdir -p /config/ssl
sudo mv tls.crt /config/ssl
sudo mv tls.key /config/ssl
```

:high_brightness: Kindly include a mount point entry into the `/etc/fstab` file.

 list `UUID` of your partitions with the `lsblk` command, run `lsblk` as follows:

```R
sudo lsblk -f
```

Put entry into the `/etc/fstab`

```R
UUID="<Device UUID number>"     /bioturing_ecosystem   ext4    defaults   0   0

--i.e.--

UUID="XXXXXX-XXXXXXX-XXXX-XXXXX"     /bioturing_ecosystem   xfs    defaults   0   0

--OR--

<Storage Device> <Mounted Volume> <File System Type> defaults   0   0

--i.e.--
/dev/nvme2n1 /data ext4    defaults   0   0
```

### BioTuring installation start

:high_brightness: Once all configurations are in place, please proceed to execute the following script.

:bell: Make sure all folder is in place before start execution installation script.

:high_brightness: Download BioTuring ecosystem script.

```R
cd /bioturing_ecosystem
wget https://github.com/bioturing/installation/archive/refs/tags/v2.0.54.tar.gz
```

:high_brightness: Uncompressed .gz file.

```R
tar xvf v2.0.54.tar.gz
```

:high_brightness: Switch to the installation folder.

```R
cd installation-2.0.54/
```

:high_brightness: Execute installation script.

```R
bash install.bioturing.ecosystem.sh
```

:high_brightness: Just follow script execution steps by providing necessary values.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/inst.png" class="lazy" width="100%"><br><br>

:high_brightness: Absolutely, ensuring the proper configuration for **HTTP_PROXY**, **HTTPS_PROXY**, and **NO_PROXY** settings is crucial for network connectivity and security within an infrastructure.

**HTTP_PROXY** and **HTTPS_PROXY** are environment variables used to define proxy settings for HTTP and HTTPS connections, respectively. These variables typically contain the URL of the proxy server.

Here are some examples of how you might set these variables:

```R
export HTTP_PROXY=http://proxy.example.com:8080
export HTTPS_PROXY=https://secure-proxy.example.com:8443
```

**NO_PROXY** is another environment variable used to specify hosts or domains that should not be accessed via the proxy. It's a comma-separated list of addresses or domains. For instance:

```R
export NO_PROXY=localhost,127.0.0.1,internal.example.com
```

Ensure that these values are configured correctly based on your network infrastructure. Adjust them to match your specific proxy server settings and network requirements.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/proxy.png" class="lazy" width="100%"><br><br>

:high_brightness: In the step of **shm size** : This number is half the available physical memory.

```R
shm size (please input shm size. This value is half of physical memory that we did for /dev/shm : 1/2 of available physical memory ): 1/2 of available physical memory
shm_size is : 1/2 of available physical memory in gb
```

:high_brightness: Kindly select right choice based on your infrastructure.

:bell: **NOTE**: If you are using Nginx proxy , Load balancer or any other proxy. Kindly select `y`. By default that value is `n` 

```R
Are you using any Proxy / Loadbalancer that will have SSL [y/n] :
```

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/port.png" class="lazy" width="100%"><br><br>

:high_brightness: Steps for SSO Domain setup involve configuring the domain access for BioTuring applications. By default, all domains are permitted access. However, if you have specific domains you'd like to authorize, kindly furnish the domain names. In case of multiple domains, please list them using a comma as a separator.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/ssod.png" class="lazy" width="100%"><br><br>

:high_brightness: WORKER COUNT, This is by default value number 4 and it's depend on server capabilities. Number 4 would be fine.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/wcount.png" class="lazy" width="100%"><br><br>

:high_brightness: Install Self-Signed CA Certificate steps choice depend on your infrastructure. We can select `n`.

```R
Install Self-Signed CA Certificate [y, n]: n
```

:high_brightness: Step to install CUDA Toolkit. Please press `y`. It will try to install cuda toolkit automatically.
:bell: **NOTE**: In case of failure. We need to install it manually and execute this script again.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/cuda.png" class="lazy" width="100%"><br><br>

:high_brightness: Accept license agreement.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/accept-l.png" class="lazy" width="100%"><br><br>

:high_brightness: Use down arrow key to select install and press enter key.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/insc.png" class="lazy" width="100%"><br><br>

:high_brightness: Cuda toolkit install successfully.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/cudo-done.png" class="lazy" width="100%"><br><br>

:high_brightness: Press `y` for NVIDIA Docker 2 installation.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/dc2.png" class="lazy" width="100%"><br><br>

:high_brightness: Provide BBrowserX's VERSION

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/dcv.png" class="lazy" width="100%"><br><br>

:high_brightness: Wait for a while to start BioTuring ecosystem container service.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/run.png" class="lazy" width="100%"><br><br>

:high_brightness: Kindly access Bioturing ecosystem URL using web browser.

```R
https://<your bioturing domain>
```

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_docker_install_img/brow.png" class="lazy" width="100%"><br><br>

## BioTuring ecosystem EKS setup

:high_brightness: Switch to Elastic Kubernetes Service (Amazon EKS) and click on **Create** under **Add Cluster**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/add_cluster.png" class="lazy" width="100%"><br><br>

:high_brightness: Provide cluster **name**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/eksn.png" class="lazy" width="100%"><br><br>

:high_brightness: Create EKS Cluster role. Feel free to explore link, given below.

:link:[Creating the Amazon EKS cluster role](https://docs.aws.amazon.com/eks/latest/userguide/service_IAM_role.html#create-service-role)

:high_brightness: Switch to IAM and select role.
:high_brightness: Click to **Create role**.


<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/crole.png" class="lazy" width="100%"><br><br>

:high_brightness: Select right trusted entity type (**AWS Service**) and use case (**EKS - Cluster**).

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/ucase.png" class="lazy" width="100%"><br><br>

:high_brightness: Click on **next** to review permission policy.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/policy_review.png" class="lazy" width="100%"><br><br>

:high_brightness: Provide **Role name** and Click to **Create role**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/eks_rname.png" class="lazy" width="100%"><br><br>

:high_brightness: Role has been created that can be used now attach with ESK cluster.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/rcreated.png" class="lazy" width="100%"><br><br>

:high_brightness: Select **Cluster service role** and **Cluster access**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/caccess.png" class="lazy" width="100%"><br><br>

:high_brightness: Click on **next**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/eksapi.png" class="lazy" width="100%"><br><br>

:high_brightness: **Configure specific network**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/network-selection.png" class="lazy" width="100%"><br><br>

:one: Switch to VPC console and create a one VPC for EKS Cluster.

:high_brightness: Click to **Create VPC**.
:high_brightness: Fill all necessary values.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/evpc.png" class="lazy" width="100%"><br><br>

:high_brightness: Click to **Create VPC**.
:high_brightness: VPC has been created.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/vpc-done.png" class="lazy" width="100%"><br><br>

:high_brightness: Select right VPC and attach to cluster.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/vpcselect.png" class="lazy" width="100%"><br><br>

:two: Switch to Subnets console and **Create subnet** under specific VPC.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/csubnet.png" class="lazy" width="100%"><br><br>

:high_brightness: Select specific **VPC ID** and provide **Subnet name**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/subnet1.png" class="lazy" width="100%"><br><br>

:high_brightness: Fill all requited values and click to **Create subnet**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/subnet2.png" class="lazy" width="100%"><br><br>

:bell: **NOTE**: We must have two subnet on different AZ. So I created two subnet at the same time.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/subnet-II.png" class="lazy" width="100%"><br><br>

:bell: **NOTE**: When we create a subnet under a specific VPC, it automatically associates with the same route table. Since my aim to access it via the public interface, I'll create an Internet Gateway and attach it to this route table. Please review your infrastructure to determine the access

:large_orange_diamond: **Internet gateway**

:high_brightness: Switch to Internet gateway and fill all necessary values then click to **Create internet gateway**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/igway.png" class="lazy" width="100%"><br><br>

:high_brightness: Now attach to a VPC to enable the VPC to communicate with the internet.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/avpc1.png" class="lazy" width="100%"><br><br>

:high_brightness: Select specific VPC and **Attach internet gateway**

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/aig1.png" class="lazy" width="100%"><br><br>

:high_brightness: Now, time to work with Router Table.
:high_brightness: Switch to Router table and **Edit routes**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/rt1.png" class="lazy" width="100%"><br><br>


:high_brightness: Click to **Add route** after filling all required values.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/addr1.png" class="lazy" width="100%"><br><br>

:high_brightness: Route table updated successfully.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/rtus.png" class="lazy" width="100%"><br><br>

:high_brightness: Click to **Subnet associations** that follow that route.
:high_brightness: Click to **Edit subnet associations**.
:high_brightness: Click to **Available subnets**.
:high_brightness: Click to **Save associations**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/subnetasso.png" class="lazy" width="100%"><br><br>

:high_brightness: All your subnets are associated with a route table.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/subnetassociationupdate.png" class="lazy" width="100%"><br><br>

:three: Security group

:high_brightness: Switch to Security Group console.
:high_brightness: Click to **Create Security Group**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/sg12.png" class="lazy" width="100%"><br><br>

:high_brightness: Please add inbound rule based on your infrastructure.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/ibrule.png" class="lazy" width="100%"><br><br>

:high_brightness: Click to **Create Security Group**.
:high_brightness: Security group has been created.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/sginrule.png" class="lazy" width="100%"><br><br>

:high_brightness: Network setting completed.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/netcom.png" class="lazy" width="100%"><br><br>

:high_brightness: Click to **Next**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/nnext.png" class="lazy" width="100%"><br><br>

:high_brightness: Select **Control plane logging**.
:high_brightness: Click to **Next**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/cplog.png" class="lazy" width="100%"><br><br>

:high_brightness: Select add-ons based on your requirement and click to **Next**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/addon.png" class="lazy" width="100%"><br><br>

:high_brightness: Review the setting and click to **Create**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/revc.png" class="lazy" width="100%"><br><br>

:high_brightness: Wait until cluster became ready.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/cluster_ready.png" class="lazy" width="100%"><br><br>

:high_brightness: Cluster creation completed.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/cluster-creation-done.png" class="lazy" width="100%"><br><br>

:large_orange_diamond: Time to create node group.

:high_brightness: Select Compute tab and click to **Add node group**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/nodeg.png" class="lazy" width="100%"><br><br>

:high_brightness: Please provide Name for node group and create specific role with proper permission for Node IAM role.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/ngc.png" class="lazy" width="100%"><br><br>

:high_brightness: Switch to **IAM console**.
:high_brightness: Click to **Create role**.
:high_brightness: On the **Use case**. Please select **EC2**, because node group is associated with EC2 instance.
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/nrole-s.png" class="lazy" width="100%"><br><br>

:high_brightness: Click to **next** to add specific permission.

```R
AmazonEKS_CNI_Policy
AmazonEKSWorkerNodePolicy
AmazonEC2ContainerRegistryReadOnly
```

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/addp.png" class="lazy" width="100%"><br><br>

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/addp12.png" class="lazy" width="100%"><br><br>

:high_brightness: Provide the **Role details**.
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/nrole.png" class="lazy" width="100%"><br><br>

:high_brightness: Click to **Create role**.
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/nrole1.png" class="lazy" width="100%"><br><br>

:high_brightness: All three permission policy has been attached to this role.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/rolecreated-1.png" class="lazy" width="100%"><br><br>

:high_brightness: Configure **node group**.
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/ndrole-added.png" class="lazy" width="100%"><br><br>

:high_brightness: Configure **Node group compute configuration**  based in your requirement.

:bell: **NOTE**: As BioTuring ecosystem required GPU supported instance. Kindly select GPU supported instance.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/compute_selection.png" class="lazy" width="100%"><br><br>


:high_brightness: Configure **Node group scaling configuration**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/ngscal.png" class="lazy" width="100%"><br><br>

:high_brightness: Configure **Specify networking**.
:high_brightness: Configure **remote access to nodes**.
:high_brightness: Click to **Next**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/eksnodessh.png" class="lazy" width="100%"><br><br>

:high_brightness: Configure **Node group network configuration**.
:high_brightness: Click to **Create**. 
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/ndreview-create.png" class="lazy" width="100%"><br><br>

:high_brightness: Node Group creation will take time. So please wait for a while.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/wait-ng.png" class="lazy" width="100%"><br><br>


:bell: **NOTE**: if you are planing to have remote access of node. Kindly make sure that enable public IP allocation on your Subnet.

```R
Enable auto-assign public IPv4 address = Y
```

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/enable-public-ip.png" class="lazy" width="100%"><br><br>

:high_brightness: Access EKS cluster. 

There are many ways to access EKS cluster. Kindly configure based your choice.

```R
aws eks --region ca-central-1  describe-cluster --name BioTuring-ecosystem-EKS  --query cluster.status --profile eks-access

aws eks update-kubeconfig --region ca-central-1 --name BioTuring-ecosystem-EKS --profile eks-access
```

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/add-profile-access-eks.png" class="lazy" width="100%"><br><br>

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/eks-ac.png" class="lazy" width="100%"><br><br>

:large_orange_diamond: Time to Amazon EBS CSI Driver to cluster.

Amazon EKS supports native Elastic Block Storage (EBS) with the Amazon EBS Container Storage Interface (CSI) driver for Kubernetes. Using this driver allows Kubernetes pods to directly read from and write to EBS volumes.

There is specif step that needs to be followed to add CSI driver.

:high_brightness: Switch to your EKS cluster.
:high_brightness: Click to **Add-on**.
:high_brightness: Select **Amazon EBS CSI Driver**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/ebsd.png" class="lazy" width="100%"><br><br>

:high_brightness: Click to **Next**

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/ebs12.png" class="lazy" width="100%"><br><br>

:high_brightness: Click to **Create**

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/ebs1234.png" class="lazy" width="100%"><br><br>

:high_brightness: **Creating an IAM OIDC provider for your cluster**.

:link: [OIDC](https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html)

It is essential that cluster would have OpenID connect.

:high_brightness: Following steps needs be followed up.

**To create an IAM OIDC identity provider for your cluster with the AWS Management Console
**

```R

1. Open the Amazon EKS console at https://console.aws.amazon.com/eks/home#/clusters.

2. In the left pane, select Clusters, and then select the name of your cluster on the Clusters page.

3. In the Details section on the Overview tab, note the value of the OpenID Connect provider URL.

4. Open the IAM console at https://console.aws.amazon.com/iam/.

5. In the left navigation pane, choose Identity Providers under Access management. If a Provider is listed that matches the URL for your cluster, then you already have a provider for your cluster. If a provider isn't listed that matches the URL for your cluster, then you must create one.

6. To create a provider, choose Add provider.

7. For Provider type, select OpenID Connect.

8. For Provider URL, enter the OIDC provider URL for your cluster, and then choose Get thumbprint.

9. For Audience, enter sts.amazonaws.com and choose Add provider.

```

:high_brightness: Copy **OpenID Connect provider URL**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/openid.png" class="lazy" width="100%"><br><br>

:high_brightness: Open the **IAM** console

:high_brightness: In the left navigation pane, choose **Identity Providers** under Access management

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/idpro.png" class="lazy" width="100%"><br><br>

:high_brightness: Choose **Add provider**.

:high_brightness: Configure provider using **Provider URL** by selecting **OpenID connect**.

:high_brightness: Click to **Get thumbprint** for verification.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/idp123A.png" class="lazy" width="100%"><br><br>

:high_brightness: Choose **Add provider**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/idp-created.png" class="lazy" width="100%"><br><br>

:large_orange_diamond: Now time to assign role.

:high_brightness: Create Amazon EBS CSI driver IAM role.

:link: [CSI driver IAM role](https://docs.aws.amazon.com/eks/latest/userguide/csi-iam-role.html) 

:high_brightness: Switch to IAM console.
:high_brightness: Select Roles.
:high_brightness: Click to Create role.
:high_brightness: Select Web identity.
:high_brightness: Select Identity provider that we created earlier.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/wid-1.png" class="lazy" width="100%"><br><br>

:high_brightness: Fill all the values and click to **next**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/webid34.png" class="lazy" width="100%"><br><br>

:high_brightness: Add correct permission policy `AmazonEBSCSIDriverPolicy`.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/csi-addp.png" class="lazy" width="100%"><br><br>

:high_brightness: After providing all necessary values. Click to **Create role**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/csi-role12.png" class="lazy" width="100%"><br><br>

:high_brightness: Edit trust policy for this role.
:high_brightness: Select that role and click to Edit trust policy.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/trustp.png" class="lazy" width="100%"><br><br>

Add a comma to the end of the previous line, and then add the following line after the previous line. Replace region-code with the AWS Region that your cluster is in. Replace EXAMPLED539D4633E53DE1B71EXAMPLE with your cluster's OIDC provider ID.

```R
"oidc.eks.region-code.amazonaws.com/id/EXAMPLED539D4633E53DE1B71EXAMPLE:sub": "system:serviceaccount:kube-system:ebs-csi-controller-sa"
```

:high_brightness: Prepare configuration script ready and update trust policy.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/pupdate.png" class="lazy" width="100%"><br><br>

:high_brightness: Policy updated.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/pupdated.png" class="lazy" width="100%"><br><br>

:high_brightness: Make sure user should have full permission.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/uperm.png" class="lazy" width="100%"><br><br>

:large_orange_diamond: Update EBS CSI Driver fro IAM Role.

:high_brightness: On the Add-on. Click to Edit and select service account and click to **Save changes**.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/ebsdupdate.png" class="lazy" width="100%"><br><br>

:high_brightness: IAM role has been added.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/iamadded.png" class="lazy" width="100%"><br><br>

:high_brightness: Do the verification of CSI deriver and Storage class.

```R
PS C:\Users\pc\Desktop\lalit_machine\BioTuring-ecosystem-documents\test>  kubectl get pods -n kube-system                         
NAME                                  READY   STATUS    RESTARTS   AGE
aws-node-7xzsv                        2/2     Running   0          82m
coredns-7bf5f58bbd-gz87w              1/1     Running   0          39h
coredns-7bf5f58bbd-q4f7k              1/1     Running   0          39h
ebs-csi-controller-84c66cd5c8-l4hgv   6/6     Running   0          76m
ebs-csi-controller-84c66cd5c8-zhhv2   6/6     Running   0          76m
ebs-csi-node-lnttw                    3/3     Running   0          82m
eks-pod-identity-agent-fjlmk          1/1     Running   0          82m
kube-proxy-x8hdn                      1/1     Running   0          82m
PS C:\Users\pc\Desktop\lalit_machine\BioTuring-ecosystem-documents\test> kubectl get storageclass
NAME            PROVISIONER             RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
gp2 (default)   kubernetes.io/aws-ebs   Delete          WaitForFirstConsumer   false                  45h
```

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/csideriver-confirm.png" class="lazy" width="100%"><br><br>

:high_brightness: Add Helm repository and download BioTuring ecosystem Helm Chart.

```R
helm repo add bioturing https://registry.bioturing.com/charts/
helm repo list
helm search repo -l 
helm pull bioturing/ecosystem       
```

:high_brightness: As we added nvidia supported instance. Nvidia would be auto installed.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/nvidia-test.png" class="lazy" width="100%"><br><br>

:bell: **NOTE**: We must enable k8s-device-plugin by following steps below.

:link: [K8s device plugin](https://github.com/NVIDIA/k8s-device-plugin)
:link: [Contianet toolkit installation](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installing-with-yum-or-dnf)
:link: [GPU Operator](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/getting-started.html)
:link:[GPU test](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/microsoft-aks.html)

:high_brightness: By default GPU plugin was not allocated.

```R
PS C:\Users\pc\Desktop\lalit_machine\BioTuring-ecosystem-documents> kubectl get nodes "-o=custom-columns=NAME:.metadata.name,GPU:.status.allocatable.nvidia\.com/gpu"
NAME                                          GPU
ip-10-0-0-171.ca-central-1.compute.internal   <none>
```

:high_brightness: Following steps to enable GPU.

```R
[root@ip-10-0-0-171 ~]# vi /etc/containerd/config.toml


[root@ip-10-0-0-171 ~]# cat /etc/containerd/config.toml
root = "/var/lib/containerd"
state = "/run/containerd"

[grpc]
address = "/run/containerd/containerd.sock"

[plugins.cri]
sandbox_image = "602401143452.dkr.ecr.ca-central-1.amazonaws.com/eks/pause:3.5"

[plugins.cri.registry]
config_path = "/etc/containerd/certs.d:/etc/docker/certs.d"

[plugins.cri.containerd.default_runtime]
privileged_without_host_devices = false
runtime_engine = ""
runtime_root = ""
runtime_type = "io.containerd.runtime.v1.linux"

[plugins.cri.containerd.default_runtime.options]
Runtime = "/etc/docker-runtimes.d/nvidia"
SystemdCgroup = true

[plugins.cri.containerd.runtimes.nvidia]
privileged_without_host_devices = false
runtime_engine = ""
runtime_root = ""
runtime_type = "io.containerd.runtime.v1.linux"

[plugins.cri.containerd.runtimes.nvidia.options]
Runtime = "/etc/docker-runtimes.d/nvidia"

version = 2
[plugins]
  [plugins."io.containerd.grpc.v1.cri"]
    [plugins."io.containerd.grpc.v1.cri".containerd]
      default_runtime_name = "nvidia"

      [plugins."io.containerd.grpc.v1.cri".containerd.runtimes]
        [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.nvidia]
          privileged_without_host_devices = false
          runtime_engine = ""
          runtime_root = ""
          runtime_type = "io.containerd.runc.v2"
          [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.nvidia.options]
            BinaryName = "/usr/bin/nvidia-container-runtime"
==============================
You can try this also
==============================
privileged_without_host_devices = false
base_runtime_spec = ""
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
    SystemdCgroup = true
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.nvidia]
    privileged_without_host_devices = false
    runtime_engine = ""
    runtime_root = ""
    runtime_type = "io.containerd.runc.v1"
    [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.nvidia.options]
    BinaryName = "/usr/bin/nvidia-container-runtime"
    SystemdCgroup = true
[plugins."io.containerd.grpc.v1.cri".cni]
bin_dir = "/opt/cni/bin"
conf_dir = "/etc/cni/net.d"

[root@ip-10-0-0-171 ~]#

[root@ip-10-0-0-171 ~]#  systemctl restart containerd

[root@ip-10-0-0-171 ~]# nvidia-container-cli --load-kmods info
NVRM version:   535.54.03
CUDA version:   12.2

Device Index:   0
Device Minor:   0
Model:          NVIDIA A10G
Brand:          Unknown
GPU UUID:       GPU-a3a8e501-1bf2-8a8b-995c-82d75b7f5aea
Bus Location:   00000000:00:1e.0
Architecture:   8.6
[root@ip-10-0-0-171 ~]# 

```

:high_brightness: Create **nvidia device plugin daemonset**.

```R
# kubectl create -f https://raw.githubusercontent.com/NVIDIA/k8s-device-plugin/v0.14.3/nvidia-device-plugin.yml

PS C:\Users\pc\Desktop\lalit_machine> kubectl create -f https://raw.githubusercontent.com/NVIDIA/k8s-device-plugin/v0.14.3/nvidia-device-plugin.yml  
daemonset.apps/nvidia-device-plugin-daemonset created
PS C:\Users\pc\Desktop\lalit_machine> kubectl get nodes "-o=custom-columns=NAME:.metadata.name,GPU:.status.allocatable.nvidia\.com/gpu"
NAME                                          GPU
ip-10-0-0-171.ca-central-1.compute.internal   1
PS C:\Users\pc\Desktop\lalit_machine>
```

:high_brightness: **GPU Operator**

	:link:[GPU Operator release note](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/release-notes.html)
	:link:[GPU Operator documents](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/getting-started.html)
	:link:[GPU precompiled drivers](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/precompiled-drivers.html)

```R

On Cluster side:
===============

# helm install --wait --generate-name -n gpu-operator --create-namespace nvidia/gpu-operator
NAME: gpu-operator-1704204555
LAST DEPLOYED: Tue Jan  2 21:09:23 2024
NAMESPACE: gpu-operator
STATUS: deployed
REVISION: 1
TEST SUITE: None

kubectl get pods -n gpu-operator
NAME                                                              READY   STATUS      RESTARTS   AGE
gpu-feature-discovery-b2whg                                       1/1     Running     0          5m11s
gpu-operator-1704204555-node-feature-discovery-gc-7467b77c8scdg   1/1     Running     0          5m21s
gpu-operator-1704204555-node-feature-discovery-master-59c6xzvgl   1/1     Running     0          5m21s
gpu-operator-1704204555-node-feature-discovery-worker-h2cwf       1/1     Running     0          5m22s
gpu-operator-5d47875775-xcxq9                                     1/1     Running     0          5m21s
nvidia-container-toolkit-daemonset-zlkdb                          1/1     Running     0          5m12s
nvidia-cuda-validator-fbm7t                                       0/1     Completed   0          5m
nvidia-dcgm-exporter-2hrwz                                        1/1     Running     0          5m11s
nvidia-device-plugin-daemonset-q7s4g                              1/1     Running     0          5m12s
nvidia-operator-validator-l984q                                   1/1     Running     0          5m12s

#kubectl get runtimeclass -A                             
NAME     HANDLER   AGE
nvidia   nvidia    7m22s

============================
-- It is depend on OS and nvidia-smi that which version of toolkit needs to be installed. --

[root@ip-10-0-0-26 ~]# nvidia-smi
Wed Jan  3 08:07:50 2024
+---------------------------------------------------------------------------------------+
| NVIDIA-SMI 535.129.03             Driver Version: 535.129.03   CUDA Version: 12.2     |
|-----------------------------------------+----------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |         Memory-Usage | GPU-Util  Compute M. |
|                                         |                      |               MIG M. |
|=========================================+======================+======================|
|   0  NVIDIA A10G                    On  | 00000000:00:1E.0 Off |                    0 |
|  0%   19C    P8               9W / 300W |      4MiB / 23028MiB |      0%      Default |
|                                         |                      |                  N/A |
+-----------------------------------------+----------------------+----------------------+

============================

PS C:\Users\pc\Desktop\lalit_machine\BioTuring-ecosystem-documents> helm install --wait --generate-name -n gpu-operator --create-namespace nvidia/gpu-operator --set toolkit.version=v1.14.3-ubi8
NAME: gpu-operator-1704269900
LAST DEPLOYED: Wed Jan  3 15:18:26 2024
NAMESPACE: gpu-operator
STATUS: deployed
REVISION: 1
TEST SUITE: None
PS C:\Users\pc\Desktop\lalit_machine\BioTuring-ecosystem-documents> kubectl get pods -n gpu-operator      
NAME                                                              READY   STATUS      RESTARTS   AGE
gpu-feature-discovery-8xpqp                                       1/1     Running     0          67s
gpu-operator-1704269900-node-feature-discovery-gc-56788784qxqg4   1/1     Running     0          76s
gpu-operator-1704269900-node-feature-discovery-master-5c7d5pbzc   1/1     Running     0          76s
gpu-operator-1704269900-node-feature-discovery-worker-jpgkf       1/1     Running     0          77s
gpu-operator-574f794446-w6g6g                                     1/1     Running     0          76s
nvidia-container-toolkit-daemonset-dlp46                          1/1     Running     0          67s
nvidia-cuda-validator-lk4lx                                       0/1     Completed   0          55s
nvidia-dcgm-exporter-l7ctn                                        1/1     Running     0          67s
nvidia-device-plugin-daemonset-fh9tw                              1/1     Running     0          67s
nvidia-operator-validator-bjhdz                                   1/1     Running     0          67s

nvidia-smi.yaml
===============
apiVersion: v1
kind: Pod
metadata:
  name: nvidia-smi
spec:
  restartPolicy: OnFailure
  containers:
  - name: nvidia-smi
    image: nvidia/cuda
    args:
    - "nvidia-smi"
    resources:
      limits:
        nvidia.com/gpu: 1


# kubectl apply -f nvidia-smi.yaml
# kubectl logs nvidia-smi         

```

:high_brightness: **Deployment**

```R

C:\Users\pc\Desktop\lalit_machine\BioTuring-ecosystem-documents\BioStudio-K8s-chart\biocolab-1.0.70\biocolab> kubectl create ns bioturing-ecosystem
namespace/bioturing-ecosystem created

C:\Users\pc\Desktop\lalit_machine\BioTuring-ecosystem-documents\BioStudio-K8s-chart\biocolab-1.0.70\biocolab> kubectl create ns biostudio
namespace/biostudio created

C:\Users\pc\Desktop\lalit_machine\BioTuring-ecosystem-documents\BioStudio-K8s-chart\biocolab-1.0.70\biocolab> cd C:\Users\pc\Desktop\lalit_machine\BioTuring-ecosystem-documents\BioStudio-K8s-chart\biocolab-1.0.70\biocolab

C:\Users\pc\Desktop\lalit_machine\BioTuring-ecosystem-documents\BioStudio-K8s-chart\biocolab-1.0.70\biocolab> helm  install --namespace=biostudio --values=values.yaml --wait=true biostudio .\biocolab-1.0.70.tgz
NAME: biostudio
LAST DEPLOYED: Thu Jan  4 00:09:43 2024
NAMESPACE: biostudio
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:

C:\Users\pc\Desktop\lalit_machine\BioTuring-ecosystem-documents\BioStudio-K8s-chart\biocolab-1.0.70\biocolab> cd C:\Users\pc\Desktop\lalit_machine\BioTuring-ecosystem-documents\Bioturing-ecosystem-K8s-chart\ecosystem

C:\Users\pc\Desktop\lalit_machine\BioTuring-ecosystem-documents\Bioturing-ecosystem-K8s-chart\ecosystem> helm  install --namespace=bioturing-ecosystem --values=values.yaml --version=2.0.1 --wait=true bioturing-ecosystem .\ecosystem-1.0.28.tgz
NAME: bioturing-ecosystem
LAST DEPLOYED: Thu Jan  4 00:11:42 2024
NAMESPACE: bioturing-ecosystem
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:

C:\Users\pc\Desktop\lalit_machine\BioTuring-ecosystem-documents\Bioturing-ecosystem-K8s-chart\ecosystem> 
```

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/bioturing_ecosystem_k8s_install_img/pods.png" class="lazy" width="100%"><br><br>

## SSO Setup

**SSO Setup URL**: `https://Your-Domain/sso_register/`

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/sso-login.png" class="lazy" width="100%"><br><br>

:high_brightness: Click to ADD button.

### SSO set up with OKTA SAML.
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/callbackurl.png" class="lazy" width="100%"><br><br>

:high_brightness: Copy Callback URL and login with OKTA and configure application.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/g-info.png" class="lazy" width="100%"><br><br>

:high_brightness: Configure SSO.

Single sign-on URL = Callback URL
Audience URI (SP Entity ID) = Company Name

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/saml.png" class="lazy" width="100%"><br><br>

:high_brightness: Click to next.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/cnext.png" class="lazy" width="100%"><br><br>

:high_brightness: Click to Finish.

:high_brightness: Copy metadata or select View SAML setup instruction.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/metadata.png" class="lazy" width="100%"><br><br>

:high_brightness: Fill all appropriate values.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/fillSSO.png" class="lazy" width="100%"><br><br>

Identity Provider Issuer = Issuer
Identity Provider Single Sign-On URL = SSO URL
Certificate = X.509 Certificate

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/filled.png" class="lazy" width="100%"><br><br>

:high_brightness: Click to ADD on BioTuring ecosystem SSO setting.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/ssodone.png" class="lazy" width="100%"><br><br>

:bell: **NOTE**: Kindly restart the POD / Container once configured SSO to update setting with ecosystem DB.

:high_brightness: Assign user to the Application and try to login.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/ssolo.png" class="lazy" width="100%"><br><br>

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/ssodiv.png" class="lazy" width="100%"><br><br>

:high_brightness: Follow the steps to login.

### SSO Configuration with PingID.

:high_brightness: login to PingID.
:high_brightness: Create new application.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/adda.png" class="lazy" width="100%"><br><br>

:high_brightness: Fill Application Name, Description.
:high_brightness: Select SAML Application.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/fill-app.png" class="lazy" width="100%"><br><br>

:high_brightness: Click continue.

:high_brightness: Select Manually Enter.

ACS URLs: It would be callback URL.

Entity ID: That can be change and take it from Application `/sso_register` 

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/sso-setting-b.png" class="lazy" width="100%"><br><br>

:high_brightness: Filed require field.
:high_brightness: Click to Save.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/add-appli.png" class="lazy" width="100%"><br><br>

:high_brightness: Set Attributes for email Address for mapping.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/sete.png" class="lazy" width="100%"><br><br>

:high_brightness: Create user to access this application.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/create-user.png" class="lazy" width="100%"><br><br>

:high_brightness: Create Group and add users to that group.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/createGroup.png" class="lazy" width="100%"><br><br>

:high_brightness: Turn it ON.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/statuson.png" class="lazy" width="100%"><br><br>

:high_brightness: Add access.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/addaccess.png" class="lazy" width="100%"><br><br>


:high_brightness: Set up SP using IdP configuration.

:high_brightness: Download Metadata URL.

:bell: **Note**: We only need three element that would be configured with SP.

Issuer: That you can use <Yourdomian>/entity
SSO URL: SingleSignOnService
Certificate: This is signing certificate

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/spsetadd.png" class="lazy" width="100%"><br><br>

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/setup-final.png" class="lazy" width="100%"><br><br>

:high_brightness: **Restart** the application from backend.

```R
# docker restart bioturing-ecosystem
bioturing-ecosystem 
```

:high_brightness: Wait for a while to restart every components.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/tlog.png" class="lazy" width="100%"><br><br>

:high_brightness: It will prompt for the PingID user credential.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/pingc.png" class="lazy" width="100%"><br><br>

:high_brightness: Login success.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/successlogin.png" class="lazy" width="100%"><br><br>

### SSO set up with Microsoft Azure SAML.

BioTuring ecosystem can setup easily with AZURE using SAML protocol by following given steos below.

:high_brightness: Login to **Azure portal**.
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/az1.png" class="lazy" width="100%"><br><br>

:high_brightness: Select Enterprises application.
:high_brightness: Click to new application.
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/az2.png" class="lazy" width="100%"><br><br>

:high_brightness: Click to your own application.
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/az3.png" class="lazy" width="100%"><br><br>

:high_brightness: Type **application name**.
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/az4.png" class="lazy" width="100%"><br><br>

:high_brightness: Click to **2. Set uo single sign on**
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/az5.png" class="lazy" width="100%"><br><br>

:high_brightness: Click to SAML.
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/az6.png" class="lazy" width="100%"><br><br>

:high_brightness: Login to BioTuring ecosystem to get **callback** URL.
:high_brightness: `your bioturing ecosystem domain`/sso_register
:high_brightness: It will prompt admin credential.
:high_brightness: You can change company ID. Based on company ID the callback URL would be changed.
:high_brightness: Copy the cap back URL.
:high_brightness: Click to **Edit** on **Azure portal** to update Basic SAML Configuration.
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/az7.png" class="lazy" width="100%"><br><br>

:high_brightness: Put callback URL to Identifier (Entity ID).
:high_brightness: Put callback URL to Reply URL.
:high_brightness: Click to save button.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/az9.png" class="lazy" width="100%"><br><br>

:high_brightness: Configuration has been done. 
:high_brightness: Please collect Application ID, Which you can get it Application property configuration, Login URL and Certificate (Base 64).

:high_brightness: Click to Users and groups.
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/az12.png" class="lazy" width="40%"><br><br>

:high_brightness: Add user.
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/az13.png" class="lazy" width="100%"><br><br>

:high_brightness: How to collect Application ID.
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/az14.png" class="lazy" width="100%"><br><br>

:high_brightness: Configure Bioturing ecosystem SSO. By clicking add button.
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/az15.png" class="lazy" width="100%"><br><br>

:high_brightness: We can view SSO configuration by clicking **VIEW** button.
:high_brightness: Use **DELETE** button to delete this configuration.

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/az18.png" class="lazy" width="100%"><br><br>

:high_brightness: **Restart** the container / pod.
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/az20.png" class="lazy" width="100%"><br><br>

:high_brightness: Follow steps to log and auth. with Azure SSO.
<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/az22.png" class="lazy" width="100%"><br><br>

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/az23.png" class="lazy" width="100%"><br><br>

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/az24.png" class="lazy" width="100%"><br><br>

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/az25.png" class="lazy" width="100%"><br><br>

<br><img alt="Bioturing ecosystem" src="https://cdn.bioturing.com/documentation/SSO_Ecosystem_img/az26.png" class="lazy" width="100%"><br><br>


## Nginx sample config

```R
server {
    listen 0.0.0.0:80;
    server_name <Your Domain> www.<Your Domain>;
    return 301 https://<Your Domain>$request_uri;
}
server {
    listen 0.0.0.0:443 ssl http2;
    server_name <Your Domain> www.<Your Domain>;
    if ($host = 'www.<Your Domain>' ) {
        rewrite  ^/(.*)$  https://<Your Domain>/$1  permanent;
    }
    ssl_certificate <Certificate file path>/tls.crt;
    ssl_certificate_key <Certificate key file path>/tls.key;
    ssl_session_timeout 1d;
    ssl_session_cache shared:SSL:20m;
    ssl_prefer_server_ciphers on;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH;
    ignore_invalid_headers off;
    client_max_body_size 0;
    client_body_timeout 1d;
    proxy_buffering off;
    proxy_read_timeout 1d;
    proxy_connect_timeout 1d;
    proxy_send_timeout 1d;
    location / {
        proxy_pass http://127.0.0.1:3000/;
        proxy_intercept_errors on;
        error_page 404 /404_not_found;
    }
}

--- Just an example ---

# vi test-lalit-bioturing-ecosystem.bioturing.com
# ln -s /etc/nginx/sites-available/test-lalit-bioturing-ecosystem.bioturing.com /etc/nginx/sites-enabled/
# ls -l /etc/nginx/sites-enabled/
# nginx -t
```

Should you require any additional support or detailed assistance, please feel free to reach out to our dedicated support team at support@bioturing.com. They'll be more than happy to provide you with the necessary guidance and information.

# Trouble Shooting

:high_brightness: Issue : Cuda driver installation failed on REDHAT server.

**Issue:**
```R
ERROR: Unable to find the kernel source tree for the currently running kernel.  Please make sure you have installed the kernel source files for your kernel and that they are properly configured; on Red Hat Linux systems, for example, be sure you have the 'kernel-source' or 'kernel-devel' RPM installed.  If you know the correct kernel source files are installed, you may specify the kernel source path with the '--kernel-source-path' command line option.
ERROR: Installation has failed.  Please see the file '/var/log/nvidia-installer.log' for details.  You may find suggestions on fixing installation problems in the README available on the Linux driver download page at www.nvidia.com.
```

**Resolution:**

```R
[root@ip-172-31-3-207 bioturing_ecosystem]#  dnf install kernel-headers
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

Last metadata expiration check: 1:04:12 ago on Tue 26 Dec 2023 05:41:07 AM UTC.
Package kernel-headers-5.14.0-362.13.1.el9_3.x86_64 is already installed.
Dependencies resolved.
Nothing to do.
Complete!
[root@ip-172-31-3-207 bioturing_ecosystem]# dnf install kernel-devel
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.


Last metadata expiration check: 1:04:18 ago on Tue 26 Dec 2023 05:41:07 AM UTC.
Package kernel-devel-5.14.0-362.13.1.el9_3.x86_64 is already installed.
Dependencies resolved.
Nothing to do.
Complete!
[root@ip-172-31-3-207 bioturing_ecosystem]# 
[root@ip-172-31-3-207 bioturing_ecosystem]# rpm -qa | grep -E "kernel-devel|kernel-headers"
kernel-headers-5.14.0-362.13.1.el9_3.x86_64
kernel-devel-5.14.0-362.13.1.el9_3.x86_64
[root@ip-172-31-3-207 bioturing_ecosystem]# reboot

```

**Sample Nginx config -- from REDHAT**

```R

Below config is just for your reference. Kindly replace domain and other related values based on your requirement.

[root@ip-172-31-3-207 nginx]# cat nginx.conf
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 4096;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;

##    server {
#        listen       80;
#        listen       [::]:80;
##        server_name  _;
#        root         /usr/share/nginx/html;
#
#        # Load configuration files for the default server block.
#        include /etc/nginx/default.d/*.conf;

#        error_page 404 /404.html;
#        location = /404.html {
#        }

 #       error_page 500 502 503 504 /50x.html;
 #       location = /50x.html {
 #       }
 #   }

# Settings for a TLS enabled server.
#
#    server {
#        listen       443 ssl http2;
#        listen       [::]:443 ssl http2;
#        server_name  _;
#        root         /usr/share/nginx/html;

#        ssl_certificate "/etc/pki/nginx/server.crt";
#        ssl_certificate_key "/etc/pki/nginx/private/server.key";
#        ssl_session_cache shared:SSL:1m;
#        ssl_session_timeout  10m;
#        ssl_ciphers PROFILE=SYSTEM;
#        ssl_prefer_server_ciphers on;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

#        error_page 404 /404.html;
#            location = /40x.html {
#        }

#        error_page 500 502 503 504 /50x.html;
#            location = /50x.html {
#        }
#    }


server {
    listen 0.0.0.0:80;
    server_name test-lalit-bioturing-ecosystem.bioturing.com www.test-lalit-bioturing-ecosystem.bioturing.com;
    return 301 https://test-lalit-bioturing-ecosystem.bioturing.com$request_uri;
}
server {
    listen 0.0.0.0:443 ssl http2;
    # listen 0.0.0.0:443;
    server_name test-lalit-bioturing-ecosystem.bioturing.com www.test-lalit-bioturing-ecosystem.bioturing.com;
    if ($host = 'www.test-lalit-bioturing-ecosystem.bioturing.com' ) {
        rewrite  ^/(.*)$  https://test-lalit-bioturing-ecosystem.bioturing.com/$1  permanent;
    }
    ssl_certificate /config/ssl/tls.crt;
    ssl_certificate_key /config/ssl/tls.key;
    ssl_session_timeout 1d;
    ssl_session_cache shared:SSL:20m;
    ssl_prefer_server_ciphers on;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH;
    ignore_invalid_headers off;
    client_max_body_size 0;
    client_body_timeout 1d;
    proxy_buffering off;
    proxy_read_timeout 1d;
    proxy_connect_timeout 1d;
    proxy_send_timeout 1d;
    location / {
        proxy_pass http://127.0.0.1:3000/;
        proxy_intercept_errors on;
        error_page 404 /404_not_found;
    }
}

}

[root@ip-172-31-3-207 nginx]#
```

## Nginx Configuration

[vhost setup](https://serverspace.io/support/help/nginx-virtual-hosts-on-ubuntu-20-04/)

```R
# In case of we are not having SSL

server {
    listen 0.0.0.0:80;
    server_name <Your Domain>.com www.<Your Domain>.com;
    #return 301 https:<Your Domain>.com$request_uri;

    ignore_invalid_headers off;
    client_max_body_size 0;
    client_body_timeout 1d;
    proxy_buffering off;
    proxy_read_timeout 1d;
    proxy_connect_timeout 1d;
    proxy_send_timeout 1d;
    location / {
        proxy_pass http://127.0.0.1:8081;
        proxy_http_version 1.1;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Host $http_host;
        add_header X-Host $host;
        proxy_set_header X-Forwarded-Host $http_host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
    }
    location ~ /\.ht {
        deny all;
    }
}

server {
    listen 0.0.0.0:443;
#       ssl http2;
    server_name <Your Domain>.com www.<Your Domain>.com;
    if ($host = 'www.<Your Domain>.com' ) {
        rewrite  ^/(.*)$  https://<Your Domain>.com/$1  permanent;
    }
    #ssl_certificate /etc/ssl/certs/testdomain.pem;
    #ssl_certificate_key /etc/pki/tls/private/testdomain.key;
    #ssl_session_timeout 1d;
    #ssl_session_cache shared:SSL:20m;
    #ssl_prefer_server_ciphers on;
    #ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    #ssl_ciphers EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH;
    ignore_invalid_headers off;
    client_max_body_size 0;
    client_body_timeout 1d;
    proxy_buffering off;
    proxy_read_timeout 1d;
    proxy_connect_timeout 1d;
    proxy_send_timeout 1d;
    location / {
        proxy_pass http://127.0.0.1:8081;
        proxy_http_version 1.1;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Host $http_host;
        add_header X-Host $host;
        proxy_set_header X-Forwarded-Host $http_host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
    }
    location ~ /\.ht {
        deny all;
    }
}
```

**Issue : To restart Nginx service**

**Solution**

```R
[root@ip-172-31-3-207 nginx]# setenforce 0
```