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

### Virtual Machine Logging
- **Azure monitor**
  - Centralized way of monitoring applications that takes log files, alerts, metrics, from various services
    - Can run queries on it
- **Guest-level monitoring (Needs to be enabled)**
  - Virtual machine extension that is installed into the running VM and is able to pull out CPU utilization, disk, and network usage
  - Enables you to monitor the VM outside of the VM (using Azure Monitor or Application Insights)
  - Go to VM settings > Diagnostics settings > Enable guest-level monitoring
  - Once this is enabled, you can configure the type of information you want collected inside it.

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
- **Costs**
  - Batch accounts are free
  - You are charged for pools while the nodes are running, even if no jobs are scheduled. 
- **Summary steps using Azure CLI**
  - Login to azure account: ``az login``
  - Create a resource group
  - Create a storage account (optional)
  - Create a batch account (required)
  - Create a pool of compute nodes (VMs that are run)
  - Create a job 
    - A Batch job is a logical group for one or more tasks. A job includes settings common to the tasks, such as priority and the pool to run tasks on. Initially the job has no tasks.
  - Create tasks on the job 
- See Azure Batch examples on github: https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts, https://github.com/Azure-Samples/azure-batch-samples/tree/master/CSharp/GettingStarted/01_HelloWorld
- Can use Batch explorer to see insights on your batch 
- See Quotas and limits: https://docs.microsoft.com/en-us/azure/batch/batch-quota-limit
- Quick start using Azure CLI: https://docs.microsoft.com/en-us/azure/batch/quick-create-cli#code-try-0

## Containers
- Midway between an IaaS and PaaS
- A way to package code and deploy it to your environment
- Easy to get up and running (with environment replicated etc)

## Docker Containers
- Create own containers using docker
- Can create own containers and package it so that it is deployed in Kubernetes cluster
- Download docker: https://hub.docker.com/editions/community/docker-ce-desktop-windows or Docker toolbox https://docs.docker.com/toolbox/toolbox_install_windows/
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
  - View dashboard: ``az aks browse --resource-group myResourceGroup --name myAKSCluster``
  - See: https://docs.microsoft.com/en-us/azure/aks/kubernetes-dashboard
- Practise: https://github.com/Azure-Samples/aks-dotnet-manage-kubernetes-cluster
- Concepts: https://docs.microsoft.com/en-us/azure/aks/concepts-clusters-workloads
- **Monitor health and logs**
  - Go to azure portal > resource group > AKS cluster > Monitoring > Insights
  - Add filter and choose Namespace <All but kube-systems>

### Create Kubernetes cluster
- DNS name prefix - used for fully qualified domain name
- Node size defaults to Standard DS2 v2 (2 cpus, 7 GB memory), 
- Defaults to 3 nodes
- Service principal: the accounts in which the nodes are going to run
- Minimum nodes is 1
- Enabling virtual nodes allows you to deploy or burst out containers to nodes backed by serverless Azure Container Instances. This can provide fast burst scaling options beyond your defined cluster size.
- VM Scale sets are required if you are using auto scale etc.
- HTTP application routing
  - Makes applications deployed to cluster publicly accessible by DNS names for application endpoints. 
  - Creates DNS zone in your subscription.
  - Not recommended for production clusters. 
- **Enable Azure Dev Spaces on your AKS Cluster**
  - Benefits
    - Reduces burden and complexity of collaborating with team in a shared AKS cluster
    - Allows you to debug containers directly in AKS
    - Allows you to test and iteratively dev your microservice apps running in AKS 
  - Azure Portal > AKS cluster > Dev spaces
  - Connect your application to your dev space, you will need Kubernetes tools for Visual Studio. 
    - Follow URL below: https://docs.microsoft.com/en-us/azure/dev-spaces/quickstart-netcore-visualstudio?toc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Faks%2FTOC.json&bc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fbread%2Ftoc.json
- **Create using application code**
  - Get service principal / authenticate
  - Create a KubernetesCluster with region, resource group, principal, defining the agent pool (with virtual machines and tier of virtual machine) with DNS prefix
  - Can update kubernetesCluster to change number of virtual nodes etc...
- **Create using Azure cli**
  - Create a resource group
  - Create a azure kubernetes cluster: ``az aks create --resource-group kubernuts-rg --name myAKSClus
ter --node-count 1 --enable-addons monitoring --generate-ssh-keys``
  - Connect to the aks cluster: ``az aks get-credentials --resource-group kubernuts-rg --name m
yAKSCluster``
  - Verify the connection to the cluster: ``kubectl get nodes``
  - A kubernetes manifest file (.yaml) specifies the desired state for a cluster and also specifies what container images to run.
  - Deploy the application by using the manifest file: ``kubectl apply -f azure-vote.yaml``
  - Test the application: ``kubectl get service azure-vote-front --watch``
  - See the external IP address, and navigate to it to test it.

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
  - See url to find your SP: https://serverfault.com/questions/896673/how-to-find-the-service-principal-assigned-to-a-newly-created-aks-cluster

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
- **Azure CLI**
  - Configure local git deployment: ``az webapp deployment user set --user-name <username> --password <password>``
    - You configure this deployment user only once. You can use it for all your Azure deployments.
    - It is different from your Azure subscription credentials
    - This deployment user is required for FTP and local Git deployment to a web app.
    - This is at the account level
  - Create a resource group: ``az group create --name <resourceGroup> --location "West Europe"``
  - Create an app service plan: ``az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE``
  - Create a web app: ``az webapp create --resource-group myResourceGroup --plan myAppSerivcePlan --name <app-name> --deployment-local-git``
  - Add an azure remote to your local git repository: ``git remote add azure <deploymentLocalGitUrl>`` and push it: ``git push azure master``
  - Browse to the Azure app: ``http://<app_name>.azurewebsites.net/swagger``
  - Enable CORS: ``az resource update --name web --resource-group myResourceGroup --namespace Microsoft.Web --resource-type config --parent sites/<app_name> --set properties.cors.allowedOrigins="['http://localhost:5000']" --api-version 2015-06-01``
  - https://docs.microsoft.com/en-gb/azure/app-service/app-service-web-tutorial-rest-api#deploy-app-to-azure
- **App Service CORS vs your CORS**
  - You can use your own CORS utlities instead of App Service CORS for more flexibility
    - More fine-grained e.g. specify different allowed origins for different routes or methods
    - Azure APp Service CORS lets you specify one set of accepted origins for all API routes and methods
  - Do not try to use App Service CORS and your own CORS code together. 
    - App Service CORS will take precedence over your own CORS
  - https://docs.microsoft.com/en-us/aspnet/core/security/cors?view=aspnetcore-2.2

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
- No additional cost; however the same resources are used, so creating a WebJob will sap some of the performance from the App Service
- You can run multiple WebJobs in an App Service.
- https://docs.microsoft.com/en-us/azure/app-service/webjobs-create
- https://microsoftlearning.github.io/AZ-203-DevelopingSolutionsforMicrosoftAzure/Instructions/Labs/AZ-203_02_lab.html
- See WebJobs vs Azure Functions: https://stackoverflow.com/questions/36610952/azure-webjobs-vs-azure-functions-how-to-choose

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
- **See API URL**
  - Your API app > Properties > URL 
- **Autoscaling**
  - To enable autoscale go to Your API app > Scale Out > Enable AutoScale
  - Enter a name, and scale mode (based on metric), add a scale rule with default values, and specify min instances and max instances and default instances > Save.
  - See: https://microsoftlearning.github.io/AZ-203-DevelopingSolutionsforMicrosoftAzure/Instructions/Labs/AZ-203_05_lab.html
- **Deployment web app using Azure CLI**
  - Using zip file: ``az webapp deployment source config-zip --resource-group MonitoredAssets --src api.zip --name smpapihai``
- **Load/Performance Tests**
  - Go to Web App > Performance test
  - Use Azure Monitor metrics after the performance test
    - Create charts in Azure Monitor based on the application insights

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
  - Lets you write stateful functions in a serverless environment.
    - Can have functions calling other async functions, chaining etc
  - In azure functions, see more templates, you will see the following:
    - **Durable Functions HTTP starter**
      - Executes an orchestrator when it receives a trigger
    - **Durable Functions activity**
      - Executes the tasks
    - **Durable Functions orchestrator**
      - Functions that invokes the other functions
  - See design patterns for Durable functions on Microsoft url: https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview
    - **Fan-out/fan-in**
      - Pattern of executing multiple concurrently then performing aggregation on the results.
    - **Chaining**
      - Pattern of executing in sequential order.
      - Often, function input requires the result of the previous function output etc
  - **Security**
    - System-assigned managed identity
    - Go to Your Function App > Platform features tab > Identity > Enable system-assigned managed identity 

## Application Insights
- Built-in logging for applications 
- **Instrumentation Key**
  - This is the key used by client applications to connect to Application Insights
  - Find it by going to: Your Application Insights > Properties > Instrument key
- **Application Insights for API Apps**
  - It is set automatically when building API app resource with Application insights
  - See APPINSIGHTS_INSTRUMENTATIONKEY in the application settings by doing the following > Your API App > Configuration > App settings > APPINSIGHTS_INSTRUMENTATIONKEY


### Function App Logging
- Turn on Application Insights to turn on logging for function apps.

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
- Developing for Table storage in Visual Studio, use the package WindowsAzure.Storage
- **TableBatchOperation**
  - Are for transactions (All or nothing)
  - Conditions for it to work:
    - All need to use same partition key
    - Limit of 2,000 operations per batch
    - Only one operation to the same entity is allowed

## Cosmos DB
- NoSQL
- One major advantage that Cosmos DB has over Storage Account Table Storage is that is guarantees very fast latency, resulting in a much quicker experience. (Sub 10ms latency guaranteed)
- Made to be built from ground up for global distribution and made for scaling for large number of transactions per second etc
- Multi-model database service (can choose different models which can choose different ways it can save data)
- CosmosDB required a CosmosDB account
- Has 99.999 SLA
- Billing is based on Reads/Writes per RU/s (request units a second) a month, and also for the storage (GB/month)
- Developing for Cosmos DB in Visual Studio, use the package corresponding to the API you chosen, i.e. for SQL API it is Microsoft.Azure.DocumentDb.
  - For querying, we would use CreateDocumentQuery<>() method etc
- **Cosmos Database**
  - Can store multiple collections
  - Provision database throughput option **(affects billing)**
    - Determines the throughput for the database (which affects billing!)
    - Note: collections also have a throughput which affects billing
- **Cosmos Collections**
  - Needs a database as it sits within a database
  - Partition key 
    - Determines how data is separated and split when the data gets too large
    - Should be wide range of values that is likely to have evenly distributed access patterns
  - Unique keys
    - Determines uniqueness on fields etc
- **Data Consistency**
  - In Azure Portal, Go to settings, default consistency
  - **Strong Consistency**
    - When data is written, it gets replicated to all other regions and everyone reading that data will have the most up to date information at that exact moment
    - Use when you need all data available in regions of the world available when it is written to
  - **Bounded Staleness Consistency**
    - Data is consistent, but has a maximum lag time which allows you to have a lag between the data being available in other regions
  - **Session Consistency** 
    - Default optin selected
    - Only consistent data depending on the session
    - Good for when application should be bound to the client
  - **Consistent Prefix Consistency**
    - Always displayed in correct order, but there may be lags between the other regions receiving the data (read throughput similar to eventual consistency)
    - Use when the order of the data matters, but the time that it is available is not important (slight delay - don't need the updated data straight away etc)
  - **Eventual Consistency**
    - No guarentee that the order will be correct when received
    - Just has a guarentee that the replicas within the group will eventually converge (received the data at some point)
    - Example, retweets, likes

### Create a Cosmos DB Account 
- **Multi-model options: Type of API**
  - Dictates the way the data is stored
  - The data is all stored in NoSQL still... but it has different APIs to access it differently (assumption) i.e. MongoDB, SQL, Graph etc
- **Geo-redundancy**
  - Replicates my data into different regions
- **Multi-region Writes**
  - Writes to multiple regions (for your replicates to be replicated properly).
  - Adds complexity in chance of data conflicts where you write to two different regions, and it needs to merge it etc.
- **Next steps**
  - Can now create a new database
    - Database can have many collections

## Azure SQL Database
- Database as a Service solution
- Azure provides SQL Database (where you are responsible for managing it i.e. performance) and a Azure SQL Managed instance (where Azure manages it for you i.e. scaling etc)
- Costs are cheaper compared to using a Virtual machine with SQL Server on it
  - **Only use SQL Server on a VM in Azure if you need to squeeze every performance or need to configure it to specific settings.**. In addition, it has extra features such as collation, and full text indexing that Azure SQL DB does not support.
- **Geo-replication**
  - Can set up geo-replication where you can replicate data to other regions
    - When doing this, you create a new server for this
  - Can program the applications to make them geographically aware to get data from the closest region
  - Can assign a region for **failover**
    - If primary region crashes, then the secondary region will become the primary until the other region recovers etc.
- **SQL Database Firewall**
  - Go to the Server level in Azure portal
  - Add white listing rules in the Firewall settings to allow other machines to access it i.e. home computer etc
    - Add IP range etc
- Can also connect database to Azure AD
  - Allows you to have service accounts for those applications etc
- Can perform queries by issuing SqlCommand() in Visual Studio using System.Data.SqlClinet etc

### Create a Azure SQL Database
- Source options
  - Can create from a backup or choose blank, or choose sample (AdventureWorks) with sample data already in it.
- Server
  - Create a new SQL Server (will be publicly accessible - so need security rules around this)
- Want to use SQL elastic pool?
  - This determine if you share resources with the other databases
  - This saves money as you are sharing all the resources for multiple databases in the pool
- **Pricing**
  - Two ways of purchasing:
    - Can use different plans (Basic, standard, premium)
    - Or can purchase more cores (Virtual Cores)
  - Pricing is based on performance
  - Can also use VCores, where you are adding CPUs to improve performance.
- Collation
  - Use default one for most situations

## Azure Blob Containers 
- Unstructered data, a container which you can throw files
- Billing is based on GB per month.
- **Access levels for Containers**
  - Private (no anonymouse access)
    - Always require authentication for access
  - Blob (Anonymous read access for blobs only)
    - Anonymous access for individual files i.e. for images in a website or text files and videos in a website
  - Container (anonymous read access for containers and blobs)
    - Anonymous access for whole container and list all files within it. 
- Containers do not have a real folder structure
  - Can create a folder strcutre but it just virtually creates the concept of the folder, just acts and looks like a folder
- Azure Portal doesn't have many options for moving blobs around etc.
  - Can use **AzCopy program** (Command line tool) to do this
  - ``Azure Login`` to login to your azure account.
- **Acquire and Break Leases**
  - **Acquire Lease**: Locks the blob, so you cannot write, update, or delete unless you have the lease ID.
  - Leases allow you to prevent modifications to Azure blobs.
- **Storage Access Tiers**
  - Blobs can have hot or cool access levels
    - Hot = access in an isntant but costs more money
    - Cool = have to keep file for at least 30 days but storage becomes cheaper and access becomes more expensive! i.e. good for storing items where you would only need to access it in emergency
    - Archive = a lot cheaper, making it more inaccessible, delay in accessing it. Push a request and Azure lets you know when it's available. Stored for at least 180 days.

## Azure Queues
- Queues are for messages
- Queue naming convention has to be lowercase
- Create a queue: ``CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();``
  - Get the Queue: ``CloudQueue myQueue = queueClient.GetQueueReference("thisisaqueue");``
- Insert into queue: ``CloudQueueMessage message = new CloudQueueMessage("message content");``
  - Add it into the queue: ``myQueue.AddMessage(message);``
- Peek 
  - peek at the message in the queue: ``myqueue.PeekMessage();``
- Take message off the queue
  - take it off the queue: ``myQueue.GetMessage();``

## Azure Active Directory (AD)
- Azure Active Directory is the default answer when it comes to identity and authentication within the cloud
- Different to on-premises Active Directory 
  - Can use Azure AD Connect to sync your on-premises AD to your Azure AD
  - Can register applications in Azure AD to give users a Single Sign On (SSO) experience 
- Azure AD can also allow you to create users that are only available in Azure AD and the user won't be synced back to your on-prem AD
- Can also use third-party websites such as Facebook, Goolge etc to be used to sign in with their credentials.
- **When you create an Azure Active Directory - it is like a whole new Azure account for your organisation.**
- **Register an app for Azure AD**
  1. Create a web app and deploy it so you have the URL.
  2. Register a new app in the Azure Portal, using the signin URL as the url for the web app
  3. Once registered, retrieve the App/Client ID and put it in your application code as well as the redirect Uri
- **Multi-tenant Azure AD**
  - Affects only the ability to grant access, it doesn't affect any other access.
  1. Go to the registered app
  2. Go to Settings > properties, scroll to the bottom where it says Multi-tenanted and set it
- **OWIN**
  - Use this for authentication with Azure AD in the development code
  - OWIN is Open Web Interface for .NET. It is a standard for web apps and web servers to talk to each other.
  - Using the package, there is a security context inside of that so we can use it to call the challenge etc.
- **Multi-factor authentication**
  - Azure AD has built in multi-authentication available
    - Can choose text message, phone call etc
  - Requires Premium license: https://docs.microsoft.com/en-us/azure/active-directory/authentication/concept-mfa-howitworks

### Azure Access Control
- **Role Based Access Control (RBAC)**
  - Grant users the **LEAST** amount of permissions that they need to get their work done (Give them minimum privileges)
  - **To check the access users have (IAM) do the following**
    - Go to your Azure subscription, click Access control (IAM), go to Role assignments
      - All Azure resources should have Access control tab (IAM), so you can see Role assignments etc
    - Assign roles to users based on what they need
      - **Roles**
        - Owner = full access and can grant other people permissions etc
        - Contributor = access to everything, but can't give access to anyone else
        - Reader = read only account
        - Many more...
    - Can also create custom roles if need be
- **Shared Access Signatures (SAS)**
  - Another way of giving people access to storage resources is by using SAS
  - **Access keys**
    - Anyone who has access keys has full access to your account
    - Microsoft gives you two keys:
      - Reason is for recylcing keys and regenerating keys without any downtime
      - If there was only 1 key, then makes it hard to keep application secure when you regenerate it as you will need to copy the key into your application and redeploy etc.
  - **Shared Access Signatures (SAS)**
    - Allows you to restrict the access based on a token
    - Sign the SAS with a key, which will generate the SAS tokens you can use
      - The signature and the combination of the key can validate the values
      - Once it's generated the SAS token, it's immediately forgotten (it doesn't store it).
        - It uses the signature to validate the contents
    - **Can't revoke the SAS token without regenerating the Access Key**
      - You cannot revoke the SAS token because Azure does not keep a list of the tokens and uses the signature to validate the token
      - **Regenerate the access key used to sign the SAS token to revoke the SAS token**. All SAS tokens that were signed with the key become invalid!

### Create an Azure Active Directory
- Organisation name = Your organisation name
- Initial domain name = the domain of the URL
- When you create an Azure Active Directory - it is like a whole new Azure account for your organisation.

## Data Encryption and Storage Accounts
- Storage accounts are encrypted by default at rest (Storage Service Encryption), Microsoft manages the keys
- You cannot turn off encryption, but you can manage your own keys by selecting Use your own key in Encryption settings for Storage account.
  - When managing your own keys, you can select to use Azure Key Vault to select the key (you are now control of the keys)
  - If you do this, you need to grant the function access to the key
- **Encryption in Transit**
  - Once Microsoft has stored your file, it's encrypted on the hard disk, then somebody needs to read the file, the file gets unencrypted and if that file is going to get transferred from the storage account to a virtual machine, application, or outside of Azure, then it is unencrypted
    - Go to Storage account settings, then Configuration. Ensure "Secure transfer required" is Enabled. This makes it only accessible by https method. Note: Azure storage doesn't support https for custom domain names (challenges there if you want to do this).
    - If anyone needs to access the data, it gets unencrypted, but is transferred over the encrypted channel of Https

## Data Encryption and SQL Databases
- **Server level database encryption***
  - Go to server level of the SQL Server database settings > Security > Transaparent data encryption
  - Our data is being stored in an encrypted manner, but it's transparent to us (it's encrypted behind the scenes and gets unencrypted when you are doing select statements)
  - Can also use own key from Azure Key Vault (similar to Data Encryption in Storage account)
- **Database level encryption**
  - From the server level, select SQL databases, and select a database, you will now see another set of settings > go to Transparent data encryption
  - You can turn off encrypted at this level, might want to disable encruption if it's already encrypted at another level
- **Note:** There is a systems generated master database which cannot be encrypted because the keys that you use to encrypt your data are stored here.
  - You can see the master database by going to resource group of the SQL database etc, then click "Show hidden types".

## Azure Key Vault 
- Azure key vault is accessible from multiple places such as ARM templates, application code etc.
- Secrets etc can be disabled if there is a security issue 
- **Access policies**
  - Restrict access to your secrets to very specific people
- **Purpose is to store Keys, Secrets, and Certificates**
  - **Keys**
    - Private and public key pairs etc
  - **Secrets**
    - Secrets application needs to store
    - Could be third-party API keys
    - Create a secret to store it
      - Azure Key Vault also stores multiple versions for you if the secret  keeps changing etc
      - Secret has a Secret Identifier which is a URL that enables you to use it 
      - Then use the using Microsoft.Azure.KeyVault in your application code, create a KeyVaultClient and do a request for the secret.
        - The application will be authenticated, which means Azure will use the Service Principal and will know who you are
        - This means the programmers will not see the secret but will have access to it and can use it.
  - **Certificates**
    - Used for signing things such as SSL and HTTPS
    - Can generate/import certificates etc (you would use a Certificate authority to get a certificate etc)
- **Pricing tiers**
  - A1 Standard
    - cheap monthly cost
  - P1 Premium
    - has Hsm backed keys which is a hardware security solution that uses the hardware element to generating the randomness etc
- Virtual Network Access can be used to restrict access
- **Create a secret**
  - Create a server-assigned managed service identity for your function app and then give that identity the appropriate permissions to get the value of a secret in the Key Vault.
  - Go to Secrets and create a new one
  - After it has been created, record the Secret identifier to use later
  - Configure Key Vault access policy to only allow certain application to use the secret
    - Go to Access Policies > Create a new access policy > Set secret permissions to GET > Set principal to your application to use the secret > Save
  - To refer to the secret use the following in the Application settings in the Portal: ``@Microsoft.KeyVault(SecretUri=<Secret Identifier>)``
  - https://microsoftlearning.github.io/AZ-203-DevelopingSolutionsforMicrosoftAzure/Instructions/Labs/AZ-203_04_lab.html#exercise-1-create-azure-resources 

## Scaling of Azure App Services
- **Scale up** = Moving pricing tier plans either up or down
  - This is seamless, doing this actually doesn't bring down your site
- **Scale out** = Adding/removing instances (adding/removing copies that are running in the same service plan)
  - You are charged for these instances
- **Manual Scaling of Azure App Service**
  - This is available in Basic tier and up
  - It means you manually configure how many instances you want in your App Service plan
- **Automatic Scaling of Azure App Service**
  - Only available in production tiers and up
  - Can add rules to determine when to scale up or down
    - Can choose scale based on metric (service plan, length of storage queue, service bus, application insights etc)
    - Or choose to scale to a specific instance count

## Scaling of Virtual Machines (Virtual Machine Scale Sets)
- **Use Virtual Machine Scale Sets**
  - Provides a load balancer, a group of identical virtual machines running in their own type of availability set
  - Can set the scalability rules for when to scale up and when to scale down (can set by schedule or performance etc)
  - Support up to 1000 virtual machine instances
- Deploy as low priority option
  - Microsoft gives you those machines when they're available, when they're not then Microsoft takes them away from you 
- Virtual machine sets have a type of availability set that are called **Placement groups.**
  - Up to 100 virtual machines can be in a placement group. This is to ensure they are distributed so if something goes wrong, they all don't go down together etc.
- **Autoscale**
  - Can choose min and max number of VMs
  - Can choose the rules based on CPU threshold etc
  - Can also set this up based on time (scheduling)
- **Scaling a single virtual machine**
 - If not using Virtual Machine Scale sets, then you can do the below:
 - **Resize of the VM**
   - Go to Settings > Size > then choose a new virtual machine size
   - By switching VM size, it is disruptive operation so do it at least disruptive time possible 
 - **Create new identical VM with larger size**
   - Then you can start directing traffic to the new VM
   - Can create load balancer to direct the traffic to two or more VMs (simplier and makes more sense)
 - **Can set up automation**
   - using ARM templates and scripting
   - Setup automation that detects an alert when CPU reaches a certain threshold then fires off a script that does it all in PowerShell
     - Powershell creates a new virtual machien and sets it up with everything it needs and adds it to the load balancer

## Transient Faults (Add Resiliency to Applications)
- **Cloud disadvantage**
  - Service downtime or slowdowns (slightly more likely to have these)
  - Applications don't get to complete their execution every time i.e. when scaling down 
    - This is called a transient fault
- **Transient Faults**
  - Temporary one-time errors such as network error, slowdown of a service, or timeout issues to database or I/O storage etc
- **Handling Transient Faults**
  - **Expect that API calls will not always work and handle errors gracefully**
  1. Retry/back-off logic
    - If some stage of the process fails, retry multiple times.
    - If it keeps failing either rollback all the applied changes back to initial state, or save the state and continue where you left off.
  2. Use queues, databases, and other messages systems (to decouple applications)
    - Do not have such a tightly coupled application. i.e. an application directly calling a third party API and expecting it to be there is tightly coupled, if the third party is down then your application fails. Instead use a queue as a middle tier between the two to handle this.
    - Can also put in a dead letter queue or poison queue if messages aren't working multiple times that will notify support etc.

## Backend Caching
- **Redis Cache**
  - Is meant to be a temporary data store by default
    - Do not put data into redis that you could not potentially lose
    - Typically for back-end caching
  - In memory Cache, doesn't actually write anything to disk
  - Guaranteed to be low latency and designed for it.
  - Put redis cache location as close to your application as possible
  - **Pricing**
    - Basic (C0-C6)
      - Shared infrastructure, No SLA, 
    - Standard (C0-C6)
      - Shared infrastructure, 99.9% SLA, SSL etc
    - Premium (P1-P5)
      - Has extra features (Redis cluster, 99.9% SLA, can store session onto disk for layer etc)
      - Can use Availability zones allowing you to put it in specific data center for Redis to be running in
      - Can use Virtual Network, Redis cluster etc
- **Reading and Writing to Redis in .NET**
  - Use StackExchange.Redis NuGet package (this is the industry standard for walking with Redis, the microsoft pacakage has less installs and less documentation and examples)
  - Get cache by doing similar: ``IDatabase cache = lazyConnection.Value.GetDatabase();``
    - Use commands to read ``cache.StringGet("someString")`` and to write ``cache.StringSet("someString", "someValue")``
    - Supports storing strings, geo positions (lat/long etc), hashes, lists, sorted sets etc
- **Security**
  - For Basic/Standard tiers you do not assign it a network, and it is publicly available on the internet using the right access keys
  - For Premium tier you can add it to a virtual network etc
    - This allows you to be a Network Security Group which can do the following:
      - Make it internal only network
      - Packet filtering and routing
      - Needs it's own subnet I think
      - You have to do the reddis cache on time of set up for this as you cannot move the reddis cache etc

## Frontend Caching
- **Content Delivery Network (CDN)**
  - Azure has a CDN
  - Sits in front of your website and will take any of the static files (anything that doesn't change from call to call), audios, videos, images, CSS, javaScript, HTML etc and will store them on another web server, often closer to your users
  - **Create a CDN**
    - CDN runs in global scale so you don't choose a location for it
      - The CDN is a global service (even though the resource group has a local region)
    - **Create a CDN profile***
      - Give it a name etc
      - **Pricing Tiers**
        - There are three companies that provide CDN to azure: Verizon, Akamai, and Microsoft
          - Same price, but features are different
    - **Create a CDN endpoint**
      - Provides the URL to access the files 
- **Ways to update files in the CDN**
  - For example, you need to change javaScript files in your CDN to fix a bug
  - You can do it in the following ways:
   1. **Purge your CDN**
   2. **Put a version number in the file name**
     - You will also need to update the references to reference the correct versions in the CDN
     - This way avoids having to purge your CDN

## Azure Monitor
- Central dashboard for all of your hardware infrastructure and reporting services coming into a single location
- Can add queries/conditions to create graphs for monitoring and pinning to your dashboard etc
- Azure monitor has a concept of Workspaces where all the logs can be sent to etc.
- Azure monitor is the recommended approach for a centralized solution to monitor all of the visibility of your applications, virtual machines, all other computer options, networking and infrastructure.

## Azure Logic Apps
- Logic app is the connector. IFTTT (if this then that) - same type of logic where you can connect anything that has an API with anything else that has an API
- Workflow program that is used to connect different components or do integration (Azure equivalent to SSIS in database world)
- All logic apps have to have a trigger
    - Http trigger
    - Monitoring location, message queue, new tweet etc
    - Timer trigger
    - More...
- Logic app designer
  - Point and click
  - Has multiple templates
- Logic apps can be used for complex functions with 10-20 steps and so on.
  - Logic apps can call azure functions for custom functions
  - Since azure functions are meant to be small pieces of code, you can string together multiple azure functions in Logic apps

## Azure Search
- Provides search capability within your own solution
- Application surfaces the data you care about, builds the index based on prioritizations you specify. 
- **Pricing Tiers**
  - Free
    - Shared resources, 3 indexes, 50 mb, no scaling
  - Basic
    - Dedicated resources, up to 3 search units, up to 3 replicas (Load balancing), 15 indexes, 2 GB
  - Standard S-S3
    - Up to 12 replicas
      - Full copies of your index
      - provides the high availability
    - Has up to 12 partitions
      - Areas within a serach engine that do the work
      - This is where it does the rebuilding of the index to give near real time results
    - Pricing is per unit so if you add more replicas or partitions then the price goes up with it.
- **Indexes**
  - Retrievable
    - Can be displayed on search results
  - Filterable
    - filter
  - Sortable
    - sort
  - Facetable (facet filter)
    - Used for searching based on grouped results (like returning buckets). For example, ``facet=price,values:100|1000|10000`` puts prices in 3 groups, where the prices are up to but not including those values. e.g. first group = all under 100 etc.
    - Used for self-directed filtering on query results, where your application offers UI controls for scoping search to search groups
    - i.e. rate ranges from 1-4 are retrievable and then you want to also filter based on that
    - Once they have seen the results, be able to filter all the results on another field
    - https://docs.microsoft.com/en-us/azure/search/search-filters-facets
  - Searchable
    - Search this field
- **Notes**
  - When adding next steps that rely on previous step attributes, add dynamic fields! Ensure that you click the dynamic button and select the fields.

## API Management (APIM)
- Your API's should be protected by a front-end so access is controlled for the API. Can have front-end protect multiple backend APIs
  - Without a front-end for your API anyone can make calls to your API and do thousands of requests per second or unintentionally cause a DDoS attack or affect your production environment
- APIM = API frontend that's a developer portal and management tool that connects into your back-end APIs
  - Can create API documentation
  - Allow them to register and apply, so you can approve them
  - Can put limits, throttles and quotas on your APIs
    - **Throttling** = controls usage of APIs by consumers during a given period. Can be used to limit number of API requests per day/week/month.
    - **Rate limiting** = defines the rate that consumers can access APIs. Determines the speed at which consumer can access APIs and is calculated in real time.
  - Can see who is calling your APIs
  - Monitor health of your APIs and identify errors
  - Run reports
- **Creating APIM Service**
  - Organisation name = the name users will see when they go to your portal/site
  - **Pricing Tiers (No free tiers)**
    - Developer
    - Basic
    - Standard
    - Premium
    - Consumption (serverless)
      - New tier
      - 1,000,000 calls free (public preview pricing - could change)
  - Creating this gives you a website at the URL
  - You will then need to configure it to point to your backend APIs
- **Configure API Management**
  - See Quickstart in Azure Portal
  - Create new subscriptions (new products) for each group you want to give access to. This allows you to restrict users from accessing your API
   - New products = new groups/subscriptions
     - Can give different access levels and so on
  - Go to APIs to configure APIM frontend to point to our backend APIs
    - Can add rules and policies (i.e. inbound or outbound processing policies such as adding headers on response, or filters any requests before they can go to backend)
    - Can add inbound policy
      - Filter IP addresses
      - Limit call rate
      - Mock responses
      - Set query params
      - CORS
      - and more...
- **Link Azure Search to Api Management**
  - Assumes you've already created your azure search with index in a database
  - Create a new APIM (20 mins to create it)
  - Add new API 
    - Enter the Web service URL as the backend service of the Azure Search
  - Add api-key header (value is Azure Search key) to All Operations and set api-version query parameter as inbound to All Operations
  - Create new GET operation to the API
    - Add query parameter for search
  - Test the operation in APIM
  - Add outbound set-header operations to delete unnecessary headers by putting in header name and selecting "delete". Leave the value as blank
  - Add custom outbound rule to the GET operation scope by adding the following appropriate place:
    - ``<outbound>
    <base />
    <set-body>
    @{ 
        var response = context.Response.Body.As<JObject>();
        return response.Property("value").Value.ToString();
    }
    </set-body>
</outbound>``
  - Test the operation in APIM
  - Tutorial here: https://microsoftlearning.github.io/AZ-203-DevelopingSolutionsforMicrosoftAzure/Instructions/Labs/AZ-203_06_lab.html#exercise-1-creating-an-azure-search-service-in-the-portal
- See options for an API Gateway: https://docs.microsoft.com/en-us/azure/architecture/microservices/design/gateway

## Event Grid and Event Hub
- The Service Bus is the enterprise grade queue system in Azure
- The second way for applications to communicate to each other are events.
- **Events vs Messages**
  - Something occurring but does not contain a lot of information, typically smaller pieces of information i.e. email alert which leads to to go and read the full email etc, whereas messages are fuller pieces of information (don't need to go off to get more information) i.e. email itself that contains the body etc
- **Event Grid (for events inside of Azure)**
  - Allows different Azure services to be triggered and picked up by other services
  - A connector between Azure services that create events and things that are available to do the work and listen to events. The Event Grid sits in-between.
- **Event Hub (for events outside of Azure)**
  - Different types of Event Hubs such as IoT Hub, Irregular Hub etc
  - Allows for external sources to push events into Azure
  - Listens for events outside of Azure (can be millions of requests per second etc) and can push events into Azure using Event Grid, or push it into a database or Blob, or a type of receiver etc.

## Azure Service Fabric
- For microservices 
- For hyperscale
- Used for applications needed for high performacne across hundreds of servers etc
- You can use Azure Service Fabric (software) in multiple different environments, Azure, private cloud, AWS etc
- It is like an abstraction running above a VM, which you can deploy your code to it will take care of scaling.
  - Many microservices per VM
  - High microservices density
  - Fast deployment and upgrades
  - Fast scaling microservices across the cluster
- Your Servers are just resources which you can use any of them when you need it, whereas with VMs you might have hot and cold VMs that are not used and need to be spun up to be used it.
- Has built in features with it i.e. Self healing, Stateful, Stateless, Load balancer, monitoring, application failover etc
- https://stackoverflow.com/questions/48415057/difference-between-kubernetes-and-service-fabric

## Azure Service Bus
- Can have two different models: 
  - **Queue Model**
  - **Topic Model**
    - Publish/Subscribe model
    - Within a topic model you push a message into a topic and that gets sent to all subscriptions to that
      - 1 to many model
      - i.e. twitter 
- Add the WindowsAzure.ServiceBus package in the code etc
- **Queue Model**
  - See all the access keys in Shared Access Policies > RootManageSharedAccessKey
    - Contains all the keys and connection strings
  - Development code
    - Create a queue client: ``QueueClient.CreateFromConnectionString(conn, queueName);``
    - Create a message: ``var msg = new BrokeredMessage("message 1")``
    - Send the message: ``client.Send(msg);``
    - Read message: ``client.OnMessage(message => { Console.WriteLine(message); });``
- **Topic Model**
  - Like a queue but multiple subscriptions will get the message
  - Development code
    - Create a namespace manager: ``var nsm = NamespaceManager.CreateFromConnectionString(conn);``
    - Check if subscription exists, if not, create it: ``if (!nsm.SubscriptionExists(topic, subscriptionName) nsm.CreateSubscription(topic, subscriptionName);``
    - Create a topic client: ``var client = TopicClient.CreateFromConnectionString(conn, topic);``
    - Create a message: ``var msg = new BrokeredMessage("message 1")``
    - Send the message: ``client.Send(msg);``
    - Subscribe to a topic: ``var subscriptionClient = SubscriptionClient.CreateFromConnectionString(conn, topic, subscriptionName);``
    - Read message: ``client.OnMessage(message => { Console.WriteLine(message); }, options);`` - options is optional
      - If options AutoComplete is set to false then you have to manually handle what happens
        - removes it from the queue: ``message.Complete();``
        - Does not remove it from the queue: ``message.Abandon();``
- **Pricing Tiers**
  - Basic
    - Queues only
    - Shared servers/environment
    - $0.05 USD million/month
  - Standard
    - Queues and Topics
    - Shared servers/environment
    - $10.00/12.5million/month
  - Premium
    - Queues and topics
    - Dedicated servers/environment
    - $668 month
- **Azure Service Bus Relay**
  - Allows your on-premises services (WCF services) to connect to ServiceBus namespace. 
  - Any application or web application/mobile application that can connect to your public Azure Service Bus can communicate to any service inside your network 
  - Exposes private serivces within your network securely

## Internal and External Load Balancer
- Two types of load balancers:
  - **Public Load Balancer**
    - Has a public IP address
    - Open to the internet, requests from internet hit the load balancer and distributes traffic to VMs inside your network
    - Filter traffic etc
  - **Internal Load Balancer**
    - Is for running between your internal networks
    - Load balance things that are not public facing 
- Backend pools
  - Are the backend services that are responsible for accepting traffic
- Probes
  - Health of the VMs to see if they are working
- Load Balancing Rules
  - Specifies what to do when traffic comes in
  - Can configure the ports that are open and redirected to back-end port etc
  - Should not enabled session persistence (if any of the servers go down - you lose your persistence etc)

## Azure Application Gateway
- Only does web based load balacing (HTTP/HTTPS etc)
- Type of Load balancer only applicable to Web 
- Has security features 
  - SSL offloading so your servers do not have to do SSL and the Gateway handles this
  - Web Application Firewall (WAF) 
  - Cookie-based session affinity (sticky cookies)
  - URL-based content routing, allows to have multiple websites behind the gateway
- Costs money