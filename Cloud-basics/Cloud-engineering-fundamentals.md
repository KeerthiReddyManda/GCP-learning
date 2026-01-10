# Cloud Engineering - Fundamentals

## Why Companies are moving to cloud?

### Business Perspective
1. Shift from Capital Expenditure (CapEx) to Operational Expenditure (OpEx)
2. Scalability based on demand
3. Built-in High Availability & Disaster Recovery

### Tech Perspective
1. Rapid provision of resources
2. Automation using Infrastructure as Code (IAC)
3. Global Scalability and performance 

## What does a Cloud Engineer Team do?
Cloud teams design, build, manage and maintain cloud based services.

Teypical roles include:

1. Cloud Architect
2. Cloud Engineer
3. Support / Operations Team
4. Interns / Trainees

## Cloud Architect vs Cloud Engineer
Cloud Architect - Designs the overall cloud solution and architecture
Cloud Engineer - Implements, operates and maintains the designed solution

## What is Cloud computing?
Cloud computing is the deliver of computing services such as servers, storage, databases, networking and software over the internet, allowing access anytime & from any place.

## Major Cloud service providers:
1. Google Cloud Platfrom(GCP)
2. Microsoft Azure
3. Amazon Web Services (AWS)
4. IBM Cloud
5. Oracle Cloud
6. Alibaba Cloud and so on 

## Types of Cloud Deployment Models:
1. Public Cloud
2. Private Cloud
3. Hybrid Cloud
4. Multi Cloud

- Note: This repository focuses on learning Google Cloud Platform (GCP).

## Cloud Computing Characteristics:
1. On demand self service (All we need is a simple interface)
2. Broad Network Access (From Anywhere through internet)
3. Resource pooling (shared resources)
4. Rapid Elasticity (The resources are elastic)
5. Measured service (Pay as you go)

## Creating a google cloud account 
* Step 1: Visit https://console.cloud.google.com
* Step 2: Sign in using existing google account or create a new account
* Step 3: Accept the Terms of Service
* Step 4: Activate the free trial (Google provides $300 in free credits (Valid for 90 days or until credits are exhausted)
  - Step 1 of 2 Account Information > Select Country > Accept Terms > Continue 
  - Step 2 of 2 Payment Verification Information > Contact Information > Add name & address > Fill all the required details Individual or Organisation > Click Save > Payment Method > Select payment option > CRedit/Debit Card or Bank Account > Provide Details (Note: No amount will be directly debited without your approval) > Save > Start Free 


## Infrastructure > Hierarchy > Project Creation 

### Infrastructure : What is Cloud Infrastructure?
In cloud, infrastructure still exists — but you don’t see or manage the hardware.
Cloud infrastructure includes:
  - Virtual Machines (Compute Engine)
  - Virtual Networks (VPC, Subnets)
  - Storage (Disks, Buckets)
  - Firewalls & IAM
  - Load balancers


### Where is Infra created?
* Regions: An independent geographical location where Google cloud is having its own data centre. 
* Zones: It's a isolated area within the region where data centres are available.

Regions(42) > Zones(127) Every region has 3 zones whereas only us-central1 region has 4 zones 

### How to select the region or zone?
It depends on multiple factors such as compliance, customer base, client base and so on 
Deployment will be in a particular location but access availability will be available from anywhere. 

If a zone is selected then the deployment is available only in that particular zone, if it is required in another location such as for high availability or disaster recovery multiple zones or regions can be opted. 

Zonal < Regional < Multi Regional < Dual Regional 

### Hierarchy:
Organisation (Company) > Folders (Optional to Group Projects) > Projects  (Mandatory) > Resources 
* Note: If individual account is created then it starts directly from project level

### Project Creation:
Click on My First Project > New Project > Fill Details Project Name > Can edit Project id with edit option > Location (Only in case of Organisation) > Create
Example Project Name: Keerthi-reddy-manda-project Project ID: keerthi-reddy-manda-project Project number: 231863212342 

Select the project by clicking on Project name (It can be found in All)

#### Project details will be as below  
* Project: 
Project is segregated into
1. Project Name - Created during the project - User Friendly - Can be changed 
2. Project ID - Created during the project - Globally Unique - Cannot be changed (Editable only during creation)
3. Project Number - After the project creation - Globally Unique - Cannot be changed (Created by google) ** Unique & important 


#### How to create a Virtual Machine (VM)?
To create a VM first enable Compute Engine 
Search Compute Engine > Enable 
After enabling the Compute Engine Create VM using Console 
Search VM > Select VM > Create Instance > Name*, Region*, Zone*, Machine Type > Create
Example: Name: vm-1, Region: us-central1, Zone: Any, Machine Type: e2-micro (2 vCPU, 1 CORE, 1GB memory)






