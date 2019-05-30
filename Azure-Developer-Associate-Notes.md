# Microsoft Certified: Azure Developer Associate Notes
- Exam AZ-203: Developing Solutions for Microsoft Azure


## Azure Learning Resources
- Azure Code Samples: https://azure.microsoft.com/en-us/resources/samples/?sort=0
- Official Azure Documentation: https://docs.microsoft.com/en-us/azure/
- Azure Citadel - Labs and Workshops: https://azurecitadel.github.io/labs/
- Azure Hands on Labs: https://www.microsoft.com/handsonlabs/SelfPacedLabs
- Official Microsoft Azure YouTube Channel: https://www.youtube.com/user/windowsazure
- Official Microsoft Developer YouTube Channel: https://www.youtube.com/channel/UCsMica-v34Irf9KVTh6xx-g
- Azure REST API Browser: https://docs.microsoft.com/en-us/rest/api/?view=Azure
- Azure Hands on labs for AZ-203: https://microsoftlearning.github.io/AZ-203-DevelopingSolutionsforMicrosoftAzure/

## Virtual Machines
- Can SSH or RDP into the VM to perform tasks
- Can use the VM for a server etc
- To allow for public internet access - enable the publi inbound ports i.e. 80, 443 if it is going to be a web server, or other ports for SSH or RDP (3389)
  - security concerns i.e. put some blocks on RDP, close ports after use etc
- Infrastructure that you are responsible for, i.e. security, updates, OS etc.
- Admin account is for RDP
- Enable Auto-shutdown when creating VM (useful)
- Can use a free domain instead of a IP address for ease of use
- Azure charges you for when you run your VM, as well as storage etc. If you stop your VM, charges will stop but you will still have to pay for Storage costs
- By default, Azure recommends using the default VM size of **Standard DS1**

### Encrypt a OS/Datadisk in VM
- Azure by default encrypts files using Secure Storage Encryption but does not encrypt the VM itself by default except the files (VHD is not encrypted, Go to Disks, look for OS disks and see that the Encryption is not enabled)
- Can use BitLocker to encrypt the Virtual Disk. The keys will be stored in Azure Key Vault
- Create a Key Vault if you don't have one, search for it
  - Stores secrets, application keys, certificates
  - To encrypt the key (needed to encrypt the VHD) using Azure Vault for a VM it has to exist in the same region as the VM
  - Use powershell to encrypt the VM (see microsoft links online)

### Connect to VM using RDP/SSH
- Go to Overtab of VM
- Go to Connect tab then you can see instructions for RDP or SSH
- **Troubleshooting RDP**
  - Restart machine in Azure Portal
  - Check the Firewall on the following:
    - Virtual Network/Subnets (Go to overview and click Virtual network/subnets, check for security groups)
    - Network security groups in Network Interface Card (Go to VM, Networking tab, Network interface card)
      - RDP should be an inbound port rule (port 3389), otherwise you will have to add the rule
    - Worse case is Recreate VM if firewall is on Windows...

## Azure Batch Services
- Allows to upload batch jobs and have azure manage the execution
  - Batched jobs: identical tasks that need to be run multiple times
  - Can run in parrallel etc
- Separate from Azure Subscription, need to create a batch account for the batch services
- See Azure Batch examples on github: https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts, https://github.com/Azure-Samples/azure-batch-samples/tree/master/CSharp/GettingStarted/01_HelloWorld
- Can use Batch explorer to see insights on your batch 

## Containers
- Midway between an IaaS and PaaS
- A way to package code and deploy it to your environment
- Easy to get up and running (with environment replicated etc)

## Docker Containers
- Create our own containers using docker
- Can create our own containers and package it so that it is deployed in Kubernetes cluster
- Download docker - https://hub.docker.com/editions/community/docker-ce-desktop-windows or Docker toolbox https://docs.docker.com/toolbox/toolbox_install_windows/
- docker-compose.yaml contains instructions on how to create the container
- Use command to create containers using the docker-compose file: ``docker-compose up -d``
- See the created containers: ``docker container ls``
- Docker runs (using native virtualisation and it does not require virtual box, or you can run it using virtual box) (which has an IP address).
  - See ip address: ``docker-machine ip Default``
  - Can now navigate to that ip address and the port to see the container running or use localhost (if not using virtualbox and using native virtualisation)
- Use command to stop containers: ``docker-compose down``
- Use command to see images: ``docker images``

### Azure Container Registry (ACR)
- Location where you can store container images for others to use
- Create a container registry in Azure
  - SKU: different types but all cost money (defaults to standard)
- To add docker image to register you tag the image with the ACR name for the registry: ``docker tag {image-name} {registry-name}:v1`` 
  - example: ``docker tag azure-vote-front azsj.azurecr.io/azure-vote-front:v1`` 
- Then push the image: ``docker push {registry-name}:v1`` i.e. ``docker push azsj.azurecr.io/azure-vote-front:v1``

## Azure Kubernetes Service (AKS)
- Azure Kubernetes is a managed Kubernetes service for Azure
- Originally created by Google
- Kubernetes container is a way to package your code and all the dependencies and everything needed to run. 
- Open source
- Supports multiple different cloud providers which allows deployment to Azure, AWS, Google Cloud etc
- Be aware that this is a cluster so there are multiple nodes - which may incurr higher fees
- The Yaml file is the settings and it tells Kubernetes where to get the code to deploy it to the cluster. It has two types of programs: back-end and front-end services.
- Install the Azure CLI to perform local tasks for azure using command line or you can use PowerShell
- **AKS Dashboard**
  - Only available for use locally
  - For local setup, download azure cli or use powerShell
  - View kubernetes dashboard in Azure Portal and copy the commands
- Practise: https://github.com/Azure-Samples/aks-dotnet-manage-kubernetes-cluster

### Create Kubernetes cluster
- DNS name prefix - used for fully qualified domain name
- Node size defaults to Standard DS2 v2 (2 cpus, 7 GB memory), 
- Defaults to 3 nodes
- Service principal: the accounts in which the nodes are going to run
- Minimum nodes is 3

### Deploy Kubernetes cluster
- Using cloud shell cli
- Use command to set up context and connect to cluster: ``az aks get-credentials --resource-group {resource-group-name} --name {cluster-name}``
- Use command to see the nodes: ``kubectl get nodes``
- Deploy code to cluster using a YAML file (see Kubernetes notes): ``kubectl apply -f {yaml-file-name}.yaml``
  - Provision of an external-ip address takes a few minutes
- See services running with the command: ``kubectl get service``
- See more at: https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough

### Deleting Kubernetes cluster
- When deleting the resource group of the Kubernetes cluster, it does not delete the Service Principal - you will need to do this separately
  - See url: https://docs.microsoft.com/en-us/azure/aks/kubernetes-service-principal#additional-considerations

## Azure Concepts
- **Resource group** 
  - A folder that stores resources that are related
  - A way of organising resources
- **Network Security Groups (NSG)**

## Azure Resource Management (ARM) Templates
- Json files that contain all configuration made to create resources
- Automation, can recreate it exactly as it was by redeploying
- Can modify the json files and deploy to create new resources with similar infrastructure i.e. changing location of data center etc
- Can Add to library if want it reusable by other projects, give it a name and save it to the library.
- Add to source control, and you will have a snapshot of the environment that is reproducable

## Azure App Services
- For web apps, create and package your code, then upload the code to Azure App Service (PaaS)
- Different to containers as they only support a set amount of languages
- Web apps only run if the user visits it, if you use web jobs it will run independently regardless of anyone visits it or not...
- WebJobs are a feature of web apps

### Create an Azure App Service
- public domain by default
- If choosing OS as windows, app insights is available, if choosing Linux, app insights is not available.
- If using publish as Docker Image, you can publish to Azure Container Registry or Kubernetes etc (not based on direct code)
- Application settings
  - ARR affinity: have cookies that go to the same server

#### Azure App Service Pricing Tiers
- Azure App Service plan is similar to a hosting plan, you can have multiple web apps on 1 plan
  - App plan should be closest to the users who are going to use it to reduce latency etc
- **Azure Compute Units (ACU)?**
    - Microsoft has come up with a number/term to describe the performance to compare with other plans
- **Note:** prices are just for 1 instance, if you scale the number of nodes then you multiple by the cost.   
- Dev/Test
  - F1 - Free, 60 mins a day compute time, no bonus features
  - D1 - Shared, custom domain
  - B1 - Basic, manual scale, custom domain/SSL
  - A series equivalent is comparing against a VM
- Production
  - S1 (standard 1), custom domain/SSL, auto scale, staging slots, daily backups, traffic manager (load balancer)
    - up to 5 instances
  - P1 (premium 1), same as above but more instances, more frequent backups, 
    - up to 20 instances
  - If you need premium plan, choose P1V2. It has same features but is cheaper than P1 and the others and has more memory and cpu.
- Isolated
  - Running on your own hardware, you are not sharing it with different tenants. Use this if you do not want different users runnning on the same hardware, there is no other traffic running on that machine or tenants slowing you down.
    - Similar to combination of IaaS and Paas, as you get features of PaaS (deployments, daily backups etc) but with the benefits of IaaS (running in your own machine) 
  - **IMPORTANT: On-top of the monthly fees presented, there is a $1000 monthly charge for the hardware** 
  - I1 - similar to production instances but running on own hardware

### Azure Web Jobs
- Web apps only run if the user visits it, if you use web jobs it will run independently regardless of anyone visits it or not...
- WebJobs are a feature of web apps, it runs as background processes
- WebJobs require a Web App 
- **Types:**
  - Continuous - starts with web app and runs continously
  - Trigger - manual (url web hook) or scheduled CRON
- Supports scripts and programs

### Enable Diagnostics logging
- In Azure App Service, go to Alerts, Metrics, Diagnostics logs
- Can turn on application logs for the internal account (locally) or to external account (blob storage etc)
- Turn on application logs but going to Diagnostics logs and creating a storage account, create a container within the storage account, can create multiple containers for each type of logs (for app itself, for machine, for web logging (IIS) etc)
- Go to the storage account to see the logs
- Can also go to the Log stream in the Web App to see real-time logs (file system application logs - this is a temporary as it turns itself off after 12 hours)

### Mobile Apps - Notification Hub
- Allows app developers to easily send notifications to phones
  - Has options to connect to Apple, Google, Windows, Windows phone, Amazon Kindle, and Baidu
  - Use SDK to connect and send them.
- Uses workspaces
- **Pricing**
  - F1 - Free tier
    - 1 million pushes, 500 active devices
  - B1 - Basic
    - 10 million pushes, 200k active devices
  - S1 - Standard
    - 10 million pushes, 10 million active devices
    - extra features: bulk operations, Rich telemetry, Scheduled pushes, multi-tenancy (separate classes of applications)
- **Set up notifications**
  - Create a mobile app in Azure (or web app)
  - Go to Mobile after it is created > Data Connection,
  - Choose storage, (Azure SQL or Storage account)
  - Allow Azure service to access server (needed if mobile app or web app)
  - Go to easy tables in tables > add a table which can be used for notifications
  - Go to Push settings, connect it to notification hub

### API Apps
- Web apps that do not have a user interface
- Same as Web App under the scenes (just different image, and name)
- Distinguishing features for API Apps
  - **Cross-Origin Resource Sharing (CORS)**
    - List of Domains that can use your API
    - Security standard
  - **API definition**
    - No UI so you need to know how to interface with it by using API definition
    - Use Swagger, and put the URL as the API definition

### Azure Function Apps
- **Hosting plan:** have option to run in App Service plan or as a serverless compute
- If you create Azure function in Azure Portal, then you cannot use it in IDE/Visual Studio, and vice versa. If you create azure function using Visual Studio and publish it, it uses a dll so you cannot see the code in the portal.
- **Serverless Consumption plan**
  - Only charged for when it runs
  - Also Total CPU usage charge as well
  - Get free 1 million executions
- **Function**
  - Intended to be short pieces of code
  - Self contained
  - Types: Webhook + API, Timer, Trigger, queue triggers, Event Hub, SendGrid, EventGrid etc
  - All functions must have a trigger and **ONE** trigger
- **Bindings**
  - Bindings have triggers, Inputs, Outputs
  - Integrates to other services through bindings
- **Durable Functions**
  - Used for complex chains of functions, similar to a logic app
    - Can have functions calling other async functions, chaining etc
  - In azure functions, see more templates, you will see the following:
    - **Durable Functions HTTP starter**
      - Executes an orchestrator when it receives a trigger
    - **Durable Functions activity**
      - Executes the tasks
    - **Durable Functions orchestrator**
      - Functions that invokes the other functions
  - See design patterns for Durable functions on Microsoft url: https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview

## Azure Storage Account
- **Storage accounts are publicly available (open to internet) unless you tie them to a specific network using network endpoint**
- Storage accounts can have blobs, queues, tables, files, each will have their own unique URL
- Can use blobs/files etc as a Static website 
- Managed storage vs Storage account (in terms of pricing)
  - Managed Storage is provisioned to a size, and you pay for that size whether or not you use it at all.
  - You Pay storage account per GB, which is what you use. Storage accounts have a fixed maximum size.
  - Storage Accounts have operation charges, priced per 10,000 operations.

### Creating a Storage account
- Storage accounts are publicly accessible by default using security keys etc
- Choose location close to your users
- **Standard storage**
  - Choose Storage V2 (Recommended)
    - ARM support 
  - Storage V1 uses the classic ASM model
  - Blob storage (only block blobs)
- **Replication**
  - Locally-redundant storage
    - Data kept in 3 total locations within a data center, (1 availability zone)
  - Zone-redundant storage
    - High availability/durability, data is stored in 3 copies stored in 3 availability zones
  - Geo-redundant storage
    - 6 copies and stored in another region, to maximise availability
  - Read access geo-redundant storage
    - Stored in different regions, accessed Read-only
- **Access tier**
  - Cool
    - Keep data atleast 30 days
    - Higher cost to access, lower cost to store
    - Good for back-ups and data that you are not going to access frequently
  - Hot
    - Data is available whenever needed
    - Standard read/write, prices, and performance
- **Virtual networks**
  - Default is all networks but select "Selected network" for more security 

### Azure Table Storage
- Relational data
- Partition key: determines how the data is partitioned/divided/split if the data is large.
- Querying database: Use TableQuery (similar to LINQ), then use Table.Execute

## Cosmos DB
- NoSQL
- Made to be built from ground up for global distribution and made for scaling for large number of transactions per second etc
- Multi-model database service (can choose different models which can choose different ways it can save data)
- CosmosDB required a CosmosDB account

### Create a Cosmos DB Account 
- **Multi-model options: Type of API**
  - Dictates the way the data is stored
  - The data is all stored in NoSQL still... but it has different APIs to access it differently i.e. MongoDB, SQL, Graph etc
- **Geo-redundancy**
  - Replicates my data into different regions
- **Multi-region Writes**
  - Writes to multiple regions (for your replicates to be replicated properly).
  - Adds complexity in chance of data conflicts where you write to two different regions, and it needs to merge it etc.