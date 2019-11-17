# Microsoft Certified: Azure Administrator Associate Notes
- Exam AZ-103: [Microsoft Azure Administrator](https://docs.microsoft.com/en-us/learn/certifications/azure-administrator?wt.mc_id=learningredirect_certs-web-wwl&tab=tab-learning-paths) ([skills measured outline](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3VwUF))

## Exam study resources
- https://ravikirans.com/az-103-study-guide/
- https://absolute-sharepoint.com/az-103-study-guide-microsoft-azure-administrator
- https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE2PjDI
- https://www.whizlabs.com/blog/azure-certifications-path/
- https://www.udemy.com/course/the-complete-walkthrough-of-microsoft-azure-services/

## Microsoft Learning Module Notes
- **Securing Data**
  - Cloud secruity is a shared responsibility
    - IaaS: still have to patch/secure OS & software and secure network.
      - Security advantage of having outsourced conern over protecting physical network
    - PaaS: outsources a lot of security concerns. Security for OS and software is outsourced. Can still control stuff in Azure Portal.
    - SaaS: outsources everything.
  - You always retain responsibility for securing the following:
    - Data
    - Endpoints
    - Accounts
    - Access management
  - **Defense in depth (Layered approach)**
    - Strategy to slow the advance of an attack trying to get unauthorized access to info.
    - Each layer provides protection, if one is breached then there is another layer as a safe net etc.
      - Can provide alert telemtry that can be acted upon 
    - Microsoft applies a layered approach to security, both in physical data centers and across Azure services
  - **Layers:**
    - **Data**
      - Almost all attackers are after this (DB, disk inside VM, etc)
    - **Application**
      - Store sensitive application in a secure storage medium
    - **Compute**
      - Secure access to VMs
      - Implement endpoint protection
      - Keep systems patched and current
    - **Networking**
      - Secure outbound/inbound internet access by limiting to only what is needed
      - Limit communication between resources
      - Deny by default
      - Implement secure connectivity to on-premises networks.
    - **Perimeter**
      - Use DDoS protection to filter large-scale attacks
      - Use perimeter firewalls to identify and alert malicious attacks against network
      - Protect from network-based attacks against your resources
    - **Identity and Access**
      - Control access to infrastructure and change control
      - Use SSO and multi-factor auth
      - Audit events & changes
    - **Physical security**
      - Controlling access to hardware within data center is first line of defence
- References:
  - https://docs.microsoft.com/en-gb/learn/modules/intro-to-security-in-azure/2-shared-responsibility
  
- **Azure Security Center**
  - Monitoring service for threat protection across all services in Azure, and on-premises. 
  - **Can do the following:**
    - Security recommendations
    - Monitor security settings and apply required security to new services as they come online 
    - Monitor all services and perform automatic security assessments to identify potential vulnerabilities
    - Use ML to detect and block malware from being installed
    - Identify potential inbound attacks
    - JIT access control for ports
  - **Available tiers**
    - Free - Limited to assessments and recommendations of Azure resources only.
    - Standard - full suite of security related services: continous monitoring, threat detction, JIT access control for ports etc
  - References: 
    - https://docs.microsoft.com/en-gb/learn/modules/intro-to-security-in-azure/2a-azure-security-center


- **Identity and access**
 - **Authentication and Authorization**
   - **Authentication**
     - Process of who the user is.
     - Involves act of challenging a party for legitimate credentials, and provides the basis for creating a security principal for identity and access control use. 
     - Establishes if they are who they say they are.
   - **Authorization**
     - Establishing what level of access an authenticated person or service has.
     - Specifies data they're allowed to access and their permissions.
  - Azure Active Directory (Azure AD)
    - Cloud-based identity service
    - Supports synchronizing with on-premises AD.
    - Provides:
      - Authentication
      - SSO
      - Application management (using Azure AD application Proxy, SSO, my apps protal, SaaS apps)
      - B2B identity services (guests, how users sign up, their profiles etc)
        - **Providing identities to services**
          - Credentials typically stored in config files. Anyone with access to these will be able to access these credentials and risk exposure.
          - Azure AD addresses this by using: service principals and managed identities for Azure services.
      - Device Management
  - References:
   - https://docs.microsoft.com/en-gb/learn/modules/intro-to-security-in-azure/3-identity-and-access


### Azure: Providing Identities to Services 
- **Service principals**
  - **Identity** = a thing that can be authenticated (person, application, service, or servers etc)
  - **Principal** = identity acting with certain roles or claims. Groups are often considered principals because they have rights assigned.
  - **Service principal** = identity used by a service or application. And like other identities, it can be **assigned roles**.
  - Creation of service principals can be tedious, making them difficult to maintain etc. Managed identities for Azure services are much easier and will do most of the work for you.
- **Managed Identities for Azure Services**
  - Managed identity can be created instantly for any Azure service that supports it.
  - **Managed identity** = creates account on organisations specific AD (on AD tenant). Azure infrastructure will automatically take care of authenticating the service and managing the account. **Can use account like any other Azure AD account, including securely letting the authenticated service access other Azure resources.**
- **Role-based access control**
  - Roles are sets of permissions, like "Read-only" or "Contributor", that users can be granted to access an Azure service instance.
  - Identities are mapped to roles directly or through group membership
  - Roles assigned at a higher scope, like an entire subscription, are inherited by child scopes, like service instances i.e resource groups and resources inside the RG etc.
- **Privileged Identity Management**
  - Azure AD Privileged Identity Management (PIM) is an additional, paid-for offering
  - Provides **oversight of role assignments, self-service and just-in-time role activation and Azure AD and Azure resource access reviews**.
- **Why separate them all?**
  - Separating security principals, access permissions, and resources provides simple access management and fine-grained control.
  - Can ensure minimum necessary permissions are granted.

### Encryption
- For most, data is most valuable asset.
- **Encryption serves as last and strongest line of defense in a layered security strategy.**
- Encryption = **making data unreadable and unusable to unauthorized viewers. (Needs to be decrypted to be read requiring a secret key)**
  - **Symmetric encryption**
    - Same key to encrypt and decrypt
  - **Asymmetric encryption**
    - public key and private key pair
    - Either key can encrypt but you need **BOTH keys to decrypt**.
- **Encryption is typically approached in two ways**
  1. Encryption at rest
  2. Encryption in transit
- **Encryption at rest**
  - At rest = data stored on a physical medium
  - Ensures data is unreadable without the keys and secrets needed to decrypt it
  - The actual data encrypted could vary in its content, usage, and importance etc.
- **Encryption in transit**
  - Data in transit = data actively moving from one location to another, such as across internet or through private network. 
  - Secure transfer can be handled by several different layers:
    1. Encrypt data at application layer prior to sending it over network. Only the receiver has the secret key that can decrypt the data to a usable form. HTTPS is an example of application layer in transit encryption.
    2. Can set up secure channel, like VPN, at network layer, to transmit data between two systems.
- **Encryption on Azure**
  - **Encrypt raw storage**
    - **Azure Storage Service Encryption** for data at rest. 
      - Automatically encrypts your data before persisting it to Azure Managed Disks, Azure Blob storage, Azure files, or azure queue storage, and decrypts the data before retrieval.
      - Handling of encryption, encryption at rest, decryption and key management in Storage Service Encryption is transparent to applications using the services.
   - **Encrypt virtual machine disks**
     - Azure Storage Service Encryption provides low-level encryption protection for physical disk, but how to ensure virutal hard disks (VHDs) of VMs are protected?
     - **Azure Disk Encryption**
       - Encrypts your Windows and Linuz IaaS feature VM disks. 
       - Leverages industry-standard BitLocker feature of Windows and dm-crypt feature of Linux
       - Integrated with Azure Key Vault to help control disk encryption keys and secrets.
  - **Encrypt databases**
    - **Transparent data encryption (TDE)**
      - **Provides encryption at rest by default for DB etc**
      - Performs real time encryption/decryption of the DB, backups, and transaction log files **at rest (without requiring changes to the application).**
      - TDE is enabled for all new Azure SQL DB instances **by default**.
      - TDE Encrypts storage of entire database using symmetric key (called DB encryption key). By default, Azure provides unique encryption key per logical SQL Server instance and handles all the details.
      - Bring your own key (BYOK) is also supported with Azure Key Vault.
  - **Encrypt secrets**
    - **Azure Key Vault**
      - Centralized cloud service for storing application secrets
      - Provides secure access, permissions control, and access logging capabilities. 
      - Can be used to store API keys, secrets, certificates
      - **Benefits**
        - Centralized application secrets
        - Securely stored secrets and keys
        - Monitor access and use
        - Simplified admin of application secrets (easy to renew certs, scale up and replicate content and use standard cert management tools etc)
        - Integrate with other Azure services (storage accounts, container registries, event hubs etc)     
- References: 
  - https://docs.microsoft.com/en-gb/learn/modules/intro-to-security-in-azure/4-encryption

### **Azure Certificates**
- Transport Layer Security (TLS) is the basis for encryption of website data in transit.
- Uses certificates to encrypt/decrypt data
- However, these certificates have a lifecycle requiring admin management.
- Websites having expired TLS certs open security vulnerabilities.
- **Certs can be signed by trusted cert authority, or be self-signed**
  - Self-signed cert is signed by its own creator (therefore, not trusted by default)
    - Most browser can ignore this problem
    - However, **you should only use self-signed certificates when developing and testing your cloud services.**
    - These certificates can contain a private or public key and have a thumbprint that provides a means to identify a certification in an unambiguous way.
    - This thumbprint is used in the Azure configuration file to identify which certification a cloud service should use.
- **Types of certificates**
  - Certs are used in Azure for two primary purposes:
    1. **Service certificates** are used for **cloud services**
    2. **Management certificates** are used for **authenticating** with the management API.
  - **Service certificates**
    - Attached to cloud services
    - Enable secure communication to and from the service
    - i.e. if you deloy web role, you want to supply a cert that can authenticate an exposed HTTPS endpoint. Services which are defined in your service definition, are automatically deployed to the VM that is running an instance of your role.
    - Upload service certificates to Azure using Portal or using classic deployment model.
      - Service certs are associated with a specific cloud service
      - They are assigned to a deployment in the service definition file.
    - Can managed service certificates separately from your services and can have different people managing them.
      - For example, a developer could upload a service package that refers to a certificate that an IT manager has previously uploaded to Azure. An IT manager can manage and renew that certificate (changing the configuration of the service) without needing to upload a new service package. Updating without a new service package is possible because the logical name, store name, and location of the certificate is in the service definition file, while the certificate thumbprint is specified in the service configuration file.
      - To update the certificate, it's only necessary to upload a new certificate and change the thumbprint value in the service configuration file.
  - **Management certificates**
    - Allows you to authenticate with the classic deployment model.
    - Many programs & tools (i.e. VS or Azure SDK) use these certs to automate configuration and deployment of various Azure Services. **However, these are not really related to cloud services.**
  - **Using Azure Key Vault with certificates**
    - **Automating cert management helps reduce or eliminate error prone task of manual cert management**
    - Can store certs in Azure Key Vault, however, Key Vault provides additional features above and beyond typical certificate management:
      - Can create certs in Key Vault or import existing certs
      - Securely store + manage certs without interaction with prviate key material
      - Create policy that directs Key Vault to manage life-cycle of a cert.
      - Provide contact info for notification about life-cycle events of expiration and renewal of cert
      - Automatically renew certs with selected issuers - Key Vsault partner x509 cert providers/cert authorities
- References:
  - https://docs.microsoft.com/en-gb/learn/modules/intro-to-security-in-azure/4a-certificates

### Protect your network
- **A layered approach to network security (Azure)**
  - **Internet protection**
    - Only allow inbound and outbond communication where necessary
    - Identify all resources that are allowing inbound network traffic of any type, then ensure they are restricted to only the ports and protocols required.
    - **Azure Security Center** allows you to see this information.
      - Identifies internet-facing resources with no network security groups associated
      - Identifies resources that are not secured by a firewall.
  - **Azure Firewall**
    - Service that grants server access based on originating IP address of each address. 
    - You create firewall rules that specify ranges of IP addresses. These generally also include specific network protocol and port info.
    - **To provide inbound protection at the perimeter, you have several choices**
      - **Azure Firewall**
        - Fully stateful firewall as a service with built-in high availability and unrestricted cloud scalability
        - Provides inbound protection for non-HTTP/S protocols (i.e RDP, SSH, FTP etc)
        - Provides outbound, network-level protection for all ports and protocols, and application-level protection for outbound HTTP/S.
      - **Azure Application Gateway**
        - **Load balancer** that includes a **Web Application Firewall (WAF)**, providing protection from known vulnerabilities in websites. (Designed to protect HTTP traffic)
      - **Network virtual appliances (NVAs)**
        - Ideal options for non-HTTP services or advanced configurations, and are similar to hardware firewall appliances.
  - **Distributed Denial of Service (DDoS) protection**
    - **Azure DDoS Protection**
      - Protects Azure applications by monitoring traffic at the Azure network edge before it can impact service's availability. 
      - Within a few minutes of attack detection, you are notified using Azure Monitor metrics.
      - **Tiers**
        - **Basic**
          - **Automatically enabled**
          - Always-on-traffic monitoring and real-time mitigation of common network-level attacks provide same defenses that Microsoft's online services use.
        - **Standard**
          - Additional mitigation capabilities tuned specifcally to Microsoft Azure Virutal Network resources.
          - Requires no application changes
          - Protection policies tuned through dedicated traffic monitoring and ML algorithms
          - Policies applied to public IP addresses deployed in VNs, such as Azure Load Balancer & Application Gateway.
          - Can mitigate the following types of attacks:
            - Volumetric attacks: seemly legit traffic to overload network
            - Protocol attacks: exploit weakness to render a target inaccessible
            - Resource layer attacks: target web app packets to disrupt transmissions of data between hosts
- **Controlling traffic inside your Virtual Network**
  - **Virtual network security**
    - Limit communication between resources to only what is required
    - For communication between VMs, **Network Security Groups (NSGs)** are critical piece to restrict unnecessary communication.
      - Allow you to filter network traffic to and from Azure resources in an Azure virtual network.
      - Can contain multiple inbound and outbound security rules to enable you to filter traffic to and from resources by source and designation IP address, port, and protocol
      - Provide list of allowed and denied communication to and from network interfaces and subnets, and are fully customizable.
      - **Can completely remove public internet access by restricting access to service endpoints.** With service endpoints, Azure service access can be limited to your virtual network.
- **Network integration**
  - **Virtual private network (VPN)**
    - VPN connections are common way of establishing secure communication channels between networks
    - Connection between Azure Virtual Network and an on-premises VPN device is a great way to provide secure communication between your network and your VNet on Azure.
  - **Azure ExpressRoute**
    - Provides dedicated, private connection between your network
    - Lets you extend your on-premises networks into Microsoft cloud over a private connection facilitated by a connectivity provider.
    - Can establish connections to Microsoft cloud services, such as Azure, Office 365 etc. 
    - Improves security of your on-premises communication by sending this traffic over private circuit instead of over the public internet.
    - Do not need to allow access to these services for your end users over the public internet, you can send this traffic through appliances for further traffic inspection.
- References:
  - https://docs.microsoft.com/en-gb/learn/modules/intro-to-security-in-azure/5-network-security

### Protect your shared documents
- **Azure Information Protection (AIP)**
  - Cloud based solution that helps organisations classify and optionally protect documents and emails by applying labels.
  - Classifies content when you save, and suggests a label i.e. Confidential..
  - After it is classified, you can:
    - Analyze data flows 
    - Detect risky behaviours
    - Track access to documents
    - Prevent data leakage or misuse
  - Requires purchase cost
- References:
  - https://docs.microsoft.com/en-gb/learn/modules/intro-to-security-in-azure/6-azure-information-protection

# Azure Advanced Threat Protection (ATP)
- Cloud-based security that identidies, detects, and helps investigate advanced threats, compromised idenities, and malicious insider actions directed at your organization.
- Capable of detecting known malicious attacks and techniques, security issues, and risks against your network.
- **Azure ATP components**
  - **ATP Portal**
    - Has it's own portal
    - Allows you to create Azure ATP instance, and view data recevied from ATP sensors.
    - Monitor, manage, nad investigate threats in your network 
    - You can sign in to the Azure ATP portal at https://portal.atp.azure.com. You must sign in with a user account that is assigned to an Azure AD security group that has access to the Azure ATP portal.
  - **ATP sensor**
    - Installed directly on your domain controllers.
    - Sensor monitors domain controller traffic without requiring a dedicated server or configuring port mirroring.
  - **ATP cloud service**
    - ATP cloud service runs on Azure infrastructure and is deployed in US, Europe and Asia.
    - Connected to Microsoft's intelligent security graph.
- **Purchasing Azure Advanced Threat Protection**
  - available as part of the Enterprise Mobility + Security E5 suite (EMS E5) and as a standalone license.
  -  You can acquire a license directly from the Enterprise Mobility + Security Pricing Options page or through the Cloud Solution Provider (CSP) licensing model. It is not available to purchase via the Azure portal.
    - https://www.microsoft.com/cloud-platform/enterprise-mobility-security-pricing
- References:
  - https://docs.microsoft.com/en-gb/learn/modules/intro-to-security-in-azure/7-advanced-threat-protection

### Understand Security Considerations for Application Lifecycle Management Solutions
- References:
  - https://docs.microsoft.com/en-gb/learn/modules/intro-to-security-in-azure/7a-microsoft-sdl

### Summary with Learn More links
- https://docs.microsoft.com/en-gb/learn/modules/intro-to-security-in-azure/8-summary

### Creating VMs
- 

# **Microsoft Labs**

