# Revision AZ-900 (Speedrun)  

Solutions Architect always ask:

- How secure is the solution?
- How much is it going to cost?  

## High Availability

Ability for your service to remain available by ensuring these isn't a single point of failure.  

## Scalability

`Vertical` - Scale up more tech specs  
`Horizontal` - More services or servers  

## Elasticity

Increase/decrease, it is automatic  
`Out` - Add Servers  
`In` - Remove Servers  
`Azure VM Scale Sets` - demand or schedule  

## Fault Tolerant

No single point of failure  
Use failovers - eg, primary DB and secondary DB  
`Azure Traffic Manager` - DNS-based traffic balancer  
`Load Balancer` - Standard LB  

## High Durability

Your ability to recover from a loss  
Backups
RPO
Integrity

## Business Continuity Plan

RPO -> Data Loss -> Disaster -> Downtime -> RTO  
RPO - max acceptable data loss after incident (willing to lose)  
RTO - max downtime of business (willing to go down)  

## DR

Cost vs Time to Recover  
Backup Restore - Pilot Light - Warm Standby - Multi-site (Active/Active)  

## Evolution of Computing

Dedicated - Bare Metal  
VMs - Hypervisor, shared with multiple customers  
Containers - Docker, fast, small footprint  
Functions - runs specific functions only, cost effective, cold start takes a while  

> 2nd hour  
> 2nd hour  
> 2nd hour  
> 2nd hour  

## Geography

`Region` - Grouping of DCs(Availability Zones)  

- Each region is paired with another region 300 miles away  
- Some services rely on paired regions for DR, eg GRS  

`Geography` - Two or more regions that preserve data residency and compliance boundaries  

- US  
- Azure Government (US)  
- Canada  
- Brazil  
- Mexico  

## Region Types and Service Availability

- `Recommended` - Most services
- `Alternate` - Not designed to support AZs  

- GA - Available for everyone  
  - `Foundational` - when GA, available immediately or in 12 months in recommended and alternate regions  
  - `Mainstream` - when GA, available immediately or in 12 months in recommended regions  
  - `Specialised` - depeneds on customer demand  

## Availability Zones

- Made up of 1 or more datacenter  
- Region generally contains 3 availability zones, DCs are physically different buildings  

## AZ Supported Regions  

- Alternate or Other - may not have them
- Recommended have at least 3 AZs  

## Fault Domain and Update Domain  

`Fault Domains` - grouping of hardware to avoid single point of failure (racks), share power source or network switch  
`Update Domains` - when you apply updates to the underlying hardware & software, UD ensure they don't go offline  
`Availability Set` - logical groupings in Azure to place VMs in different FDs and UDs to avoid downtime  

## Computing Services  

Azure Virtual Machines  
Azure Container Services  
Azure Kubernetes Service  
Azure Service Fabric - Tier 1 - Enterprise Containers as a Service - Runs in Azure or on-prem, container management  
Azure Functions - Serverless, pay for compute services  
Azure Batch - plans, schedules and executes batch computer workloads across running 100+ jobs in parallel- Use spot VMs to save money  

## Azure VMs

20 per region  
Can attach multiple managed disks to Azure VMs  
NSG - Network Security Group, firewall rules  
NIC - a device that handles the IP Protocol and network communication  
vNet - The Virtual Network where the VM will live  

## Azure Scale Sets  

Increase or decrease your VM capacity of identical VMs, same image and size  

- Create Scaling Policies  
- Create Health Checks and set a Repair Policy  
- Associate a Load Balancer to distribute VMs across AZs  
- Scale to 100s or 1000s

## Load Balancer  

Associated with a scale set  
LB health checks  

Two types:  

- Application Gateway (HTTP/HTTPS)  
  - web traffic  
  - URL-based routing  
  - SSL termination  
  - Session Persistence  
  - WAF
- Azure Load Balancer (TCP/UDP)  
  - network traffic  
  - port forwarding  
  - outbound flows  

## Scaling Policy  

When to add/remove to scale sets  

- min number of vms  
- CPU threshold  
- duration in minutes  

When creating, it gives limited options in the GUI  
After creation, you can modify and it has a lot more options  

Lots of scale rules:  

- Disk, CPU, Network
- Add more metrics by using:  
  - App Insights - Small package on a VM that gathers telemetry  
  - Azure Diagnostic Extensions - Agent in the VM, stores perf metrics to Azure Storage. More detailed host based metrics.  

## Scaling Policies

- Scale-In Policy  
  - You can delete the Default (highest Instance ID, balanced across AZs and FDs), Newest, Oldest  
- Update Policy  
  - Auto (immediate and random order), Manual, Rolling (batches with optional pause)  

## Health Montitoring  

- 2 modes  
  - App health extension  
    - http, expects  200 status
  - LB Probe  
    - check TCP, UDP or HTTP  
  - Auto repair policy can be switched on  

> 3rd Hour  
> 3rd Hour  
> 3rd Hour  
> 3rd Hour  

## Azure App Services (PaaS)

Deploy and manage webapps

- Pay by App Service Plans  
  - Shared - Free, Shared (Linux not supported!)  
  - Dedicated - Basic, Standard, Premium, PremiumV2, PremiumV3  
  - Isolated Tier -  
It can also run docker - single or multi-containers  
the DNS name is xxxx.azurewebsites.net  

## App Service Runtimes

- A runtime generally means what programming languages and libraries and framework you are using.  
- A runtime for Azure App Services will be a pre-defined contaier that has your programming language and commonly used libraries installed.  
- .net, .net core, java, node.js, php, python etc  

## Custom containers

- Create your own docker container, push it to the registry and then deploy to the app service  

## Deployment Slots

- allows you to create different environments. prod, staging etc  
  - you can swap them out (blue/green)  

## App Service Environments (ASE)

- Fully isolated and dedicated environments at a very high scale
  - ASE comes with it's own pricing tier, being the `Isolated tier`  
- There are 2 deployment types:  
  - External ASE  
  - ILB ASE - Same as above but also has an Internal Load Balancer  

## App Service Plans  

- 3 pricing tiers  
  - Dev/Test - F1, B1  
  - Production - Standard & Premium  
  - Isolated - used for ASE

## ACI

launches containers  
win or linux  
persist storage with Azure Files  
ACIs are accessed via fully qualifies domain names (FQDN) xxx.xxx.azurecontainer.io  

- `Container Groups`  
  - Get scheduled on the same host, they share resources like:  
    - lifecycle, resources, local network, storage volumes  
    - Container Groups are similar to k8s pods  
    - Two ways to deploy, ARM templates or YAML files  
- `Container Restart Policies`  
  - Always  - long running tasks, eg, web server  
  - Never - run one time only eg, background job  
  - OnFailure - If there is an error, it restarts
- `Container Environment Variables`  
  - Portal, CLI, powershell  
  - pass in secured environment variables - through CLI with the flag `--secure-environment-variables SECRET=$SECRET_KEY` on an `az container create`  
- `Persistent Storage`  
  - Containers are stateless by default  
  - mount Azure files, secret volume, empty directoy, cloud git repo etc  
  - have to do this through the CLI when creating the container  
  - user the flags `--azure-files-volume-share-name` and `--azure-files-volume-mount-path`  

## vNets

- Private (not a thing, you have to define it yourself) and Public  
- Lots of components:  
  - Azure DNS  
  - Address Spaces, Route Tables, Subnets  
  - Network Security Groups  
  - Express Route - connection between vNet and on-prem  
  - Virtual WAN - centralised network to route different network connections  
  - Virtual Network Gateway - A site-to-site VPN between vNet and local networks  
  - Network Interface - virtual device to allow VMs to communicate using IP protocol  
- `Peering`  
  - regional peering - two in the same region  
  - global peering - any two regions  
- `Gateway Subnet`  
  - A special type of subnet that is used by Azure Virtual Network Gateway, it launches specialised VMs in the subnet  
- `Azure DNS` cannot be used to purchase domains, use App Services for this

## Virtual Network Gateway

it's a software VPN devices  

- creates them in 2 specialised VMs in specific `gateway subnets`  
- VMs contain routing tables to run specific gateway services  
- Gateway type can be VPN or ExpressRoute  

## Azure Express Route  

- Like AWS Direct Connect  
- Doesn't go over the public internet  
- Private Peering for vNets  
- `Express Route Direct`, even faster, bigger bandwidth  

## Private Links  

- Establish Secure connections between Azure Resources so that the traffic remains within the Azure Network, everything goes across the Azure backend network  
- Faster and more secure  
- Doesn't go over the private network  
- You need to create a `Private Link Endpoint`, it's just a nic, private IP on your vNet  
- `Private Link Service` allows you to connect your workload to Private Link. You will need an `Azure Standard Internal Load Balancer` to associate it with the link service.  

## Storage Services  

- `Blob` Object servless storage, text and binaries etc  
- `Azure Disk Storage` used by VMs, like an EBS volume  
- `Azure File Storage` used for shared volume access, like a file server  
- `Azure Queue Storage` for SQS, NoSQL store
- `Azure Table Storage` for wide column NoSQL DBs  
- `Azure DataBox` and `Azure DataBox Heavy` rugged briefcase computer and storage designed to move TB and PB of data  
- `Azure Archive Storage` long term cold storage  
- `Azure Data Lake Storage` storage for structured and unstructured data at any scale  

> 4th Hour  
> 4th Hour  
> 4th Hour  
> 4th Hour  

## AZCopy  

To copy files and blobs to or from a storage account  
`azcopy login`  
`azcopy copy <source> <destination>`  

## Azure Files  

Use it to persist storage for containers  

## Azure Active Directory  

It is now called `Azure Entra ID`  

## SSO support  

- OpenID Connect and OAuth - web and mobile apps  
- SAML - federated  
- Password-based - traditional  
- Linked - link multiple accounts from different identity providers  
- Integrated Windows Authentication (IWA) - windows domain creds from their existing windows session  
- Header-based Auth - app accepts the auth token in the form of a header in each request  

## External Identities  

External users outside of your organisation to use internal resources  

## Defense in Depth  

Layers:  

- Data  
- Application  
- Compute  
- Network  
- Perimeter  
- Identity and Access  
- Physical  

## Azure Defender

Provides advanced protection for Azure and on-prem workloads  
Accessed in the security center  
Can utilise Azure Arc to perform protection  

## Key Vault

Store cryptographic keys and other secrets  
Also does cert management  

> 5th Hour  
> 5th Hour  
> 5th Hour  
> 5th Hour  

## Azure Firewall  

Manages and protects Azure vNet resources  
It is a fully stateful firewall  
Centrally create, enforce and log application abd network connectivity policies across subscriptions and virtual networks  
Uses a static public IP  
Fully integrated with Azure Monitor  
Firewall is in its own vNet  
Other vnets pass through the central vnet  

## Application Gateway

Application level-routing and load balancing  
Works on Layer 7  
Frontends -> Routing Rules -> Backend Pools  

- Frontend:
  - Public - Will create a Public or External Load Balancer  
  - Private - Will create an Internal Load Balancer  
- Backends:  
  - It's a collection of resources that the app gateway sends traffic to  
  - A backend pool can contain:  
    - VM, VMSS, IP addresses, domain names, App Services  
- Routing Rules:  
  - Listeners:  
    - Basic - forward all requests to backend pools  
    - Multi-site - forward requests to different backend pools based on the host header or host name  
    - Requests are matched depending on order of the rules  
  - Backend Targets:  
    - either to a backend pool or redirection  
  - HTTP Settings:  
    - how to handle cookies, connection draining, port request time outs and more  
    - Ports, cooke-based affinity, connection draining, request time out, override backend path(override the URL), override hostname  

## RBAC

- A role Assignment consists of 3 things:  
  - `security principal` - user, group, service principle(security identity used by apps or services to access specific azure resources), managhed identity(ID that is automatically managed in Azure)  
  - `role definition` - read, write and delete to Azure resources
  - `scope` - set of resources that the role assignment applies to. Scope them to Management, Sub or resource groups.  
- 4 fundamental roles  
  - `Owner` - Read, Grant, CreateUpdate/Delete  
  - `Contributor` - Read, CreateUpdate/Delete
  - `Reader` - Read  
  - `User Access Administrator` - Grant  

## Management Groups

Manage multiple subs in a hierarchal structure  

## Azure Integration Services

- Azure Notification Hub, this is like AWS SNS  
  - pub/sub - push notifications to any platform from any backend  
  - Azure API Apps - Build and consume APIs in the cloud, route API requests to Azure Services  
  - Azure Service Bus - Messaging as a Service, similar to AWS SQS  
  - Azure Logic Apps - Orchestrate takes, business processes and workflows. Integration with Enterprise SaaS and Enterprise Applications  
  - Azure API Management - Hybrid, multi-cloud management platform for APIs across all environments, placed in front of exiting APIs to add additional functionality  
  - Azure Queue Storage - Message Queue for delivering messages between applications  

## Cloud-Native Networking Services
  
- Azure DNS  
- Azure vNet  
- Azure Load Balancer - Layer 4  
- Azure Application Gateway - Layer 7  
- Network Security Groups - firewall at the subnet level  

## Hybrid Networking Services

- FrontDoor: scalable entry point for fast delivery of global applications  
- Express Route: on-prem to cloud connection  
- Virtual WAN  
- Azure Connection: VPN connections between two Azure local networks via IPSec  
- Virtual Network Gateway: Site-to-Site VPN between Azure vNet and your local network  

## Azure Traffic Manager

- DNS layer routing based on routing policies, such as:  
  - weighted  
  - performance  
  - priority  
  - geographic  
  - multivalue  
  - subnet  

## Servless  

- Event Driver at scale  
- Abstracted Servers  
- Micro Billing  
- The types include:  
  - Azure Functions - runs small bits of code
  - Blob Storage - upload fields and don't think about underlying infra
  - Logic Apps - serverless workflows composed of Azure Functions  
  - Event Grid - Pub/Sub messaging system to trigger other cloud services such as Azure Functions

## Azure Subscriptions

- 4 Levels  
  - Free Sub - 12 months
  - PAYG Sub - monthly charges  
  - Enterprise Agreement - between Azure and your organisation  
  - Student Sub - $100USD credit for 12 months  

> 6th Hour  
> 6th Hour  
> 6th Hour  
> 6th Hour  

## Azure Policies

Enforce Organisational standards and assess compliance at scale, they don't restrict access, they only observe compliance  

- Policy Deifinitions - JSON  
- Policy Assignments - Scope  
- Policy Parameters - Values passed to the policy definition  
- Initiative Definitions - Collection of policy definitions  

## Resource Locks

Lock sub, resource group or resource  

- CanNotDelete  
- ReadOnly  

## Azure Blueprints

Quick creation of governed subscriptions  
Uses Policies, role assignments ARM templates, resource groups
Used for tracking and auditing  

## Azure Preview Features

Go to `preview.portal.azure.com` for preview features  

> 7th Hour  
> 7th Hour  
> 7th Hour  
> 7th Hour  

## Azure Resource Manager  

Basically a service that allows you to manage Azure Resources, everything goes through ARM  
It's a management layer  
Subs, management groups, resource groups, resource providers, resource locks, Azure blueprints, Resource tags, IAM, RBAC, Azure Policies and ARM Templates etc  
ARM is a gatekeeper.
Everything goes through it Azure Portal, REST Client go to ARM, while Azure Powershell, Azure CLI go to AzureSDK and then to ARM  

## Scoping

- Management Group -> Subscription -> resource groups -> Resource  

## ARM  

Declarative IaC templates  
modularity  
extensible  
arm-ttk for testing  
track rdeployments  

## ARM Templates  

parameters, variables, resources  

## BICEP  

Newer way to do IaC  
Similar to Terraform  

## Observability Pillars  

Logs, Observability, Metrics and Traces  

## Azure Monitor  

collecting, analysing and acting on telemetry  
source of data - app, os, resources, subs, tenant, custom sources  
They are either logs or metrics  
Insights, visualise, analyse, respond, integrate  
Check the Azure doco for this  

## Log Analytics  

Edit and run log queries on data win Azure Monitor Logs
Uses KQL

## Workspaces  

Unique envrionment for Azure Monitor log data  
Each workspace has its own data repo and configuration  
data sources and solutions are configured to store their data in particular workspaces  

## Azure Alerts

Metric, log and activity log alerts  
does an action when it's triggered  

## Application Insights  

It's an APM  
It has a unique instrumentation key (ikey)

Just an agent installed on the app or service or resource etc

- Monitor performance and availability  
- Auto-detects performance anomolies  
- includes powerful analytic tools  
- continuously improve performance and usability  
- works on a host of apps, node.js, python, .net etc  
- integrates with the DevOps process  

It monitors a whole bunch of stuff:

- request rates, response times, failure rates, dependency rates, exceptions, page views, AJAX calls, host diagnostics, user, session counts etc  

Where can you see the telemetry?  
Smart detection and manual alerts, dashboards, Visual Studio, Live Metrics Stream, Analytics, Power BI
