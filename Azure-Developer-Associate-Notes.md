# Microsoft Certified: Azure Developer Associate Notes
- Exam AZ-203 

## Virtual Machines
- Can SSH or RDP into the VM to perform tasks
- Can use the VM for a server etc
- To allow for public internet access - enable the public inbound ports i.e. 80, 443 if it is going to be a web server, or other ports for SSH or RDP (3389)
  - security concerns i.e. put some blocks on RDP, close ports after use etc
- Infrastructure that you are responsible for, i.e. security, updates, OS etc.
- Admin account is for RDP
- Enable Auto-shutdown when creating VM (useful)
- Can use a free domain instead of a IP address for ease of use

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

## Azure Resource Management (ARM) Tempaltes
- Json files that contain all configuration made to create resources
- Automation so you can recreate it if run

