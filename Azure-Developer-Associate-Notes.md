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

