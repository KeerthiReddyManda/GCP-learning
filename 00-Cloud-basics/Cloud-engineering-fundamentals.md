# Cloud Engineering - Fundamentals

## Why Companies are moving to cloud?

### Business Perspective
1. Shifting from Capital Expenditure to Operational Expenditure
2. Scalability
3. Built in High Availability & Disaster Recovery

### Tech Perspective
1. Can create provision resources instantly
2. Automation (HCL as code)
3. Scalability (Global reach & performance)

## What does Cloud Engineer team do?
They manage, maintain & develop the cloud deployed services.
Team members: 
1. Cloud Architecture
2. Cloud Engineer
3. Support Team
4. Intern

## Difference between Cloud Architecture & Cloud Engineer
Cloud Architecture - Designs the req uirement
Cloud Engineer - Executes the requirement

## What is Cloud computing?
Cloud computing is to deliver computing services such as servers, storage, databases, networking etc., over the internet at any time & from any place.

## Different Cloud service providers:
1. GCP - Google
2. Azure - Microsoft
3. Amazon - AWS
4. IBM Cloud
5. Oracle Cloud
6. Alibaba Cloud and so on 

## Types of Cloud services:
1. Public
2. Private
3. Hybrid
4. Multi

## Cloud Computing Characteristics:
1. On demand self service (All we need is a simple interface)
2. Broad Network Access (From Anywhere through internet)
3. Resource pooling (shared resources)
4. Rapid Elasticity (The resources are elastic)
5. Measured service (Pay as you go)

* We will be working on Google Cloud Platform (GCP)

## How to create google cloud account?
* Step 1: Open > console.cloud.google.com (https://console.cloud.google.com), if existing gmail can directly click on login credentials or else select create account and proceed with the available email and verification steps to create a account.
* Step 2: Select the country, check box Terms of Service & Click Agree & Continue
* Step 3: Google is providing $300 in free credits > Select Try for free then proceed woth the steps
  Step 1 of 2 Account Information > Select Country > Click Agree & Continue 
  Step 2 of 2 Payment Verification Information > Contact Information > Add name & address > Fill all the required details Individual or Organisation > Click Save > Payment Method > Select payment option > CRedit/Debit Crad or Bank Account > Provide Details (Note: No amount will be directly debited without your approval) > Save > Start Free 
* Note: $300 Credits will be added to the console account which will be available for usage upto 91 days or credits usage completion whichever is earlier 


# Infrastructure > Hierarchy > Project Creation 

## Infrastructure : Where is Infra created?
* Regions: A independent geographical location where google cloud is having it's own data centre. 
* Zones: It's a isolated area within the region where data centres are available.

Regions(42) > Zones(127) Every region has 3 zones whereas only us-central1 region has 4 zones 

# How to select the region or zone?
It depends on multiple factors such as compliance, customer base, client base and so on 
Deployment will be in a particular location but access availability will be available from anywhere. 

If a zone is selected then the deployment is available only in that particular zone, if it is required in another location such as for high availability or disaster recovery multiple zones or regions can be opted. 

Zonal < Regional < Multi Regional < Dual Regional 

# Hierarchy:
Organisation (Company) > Folders (Optional to Group Projects) > Projects  (Mandatory) > Resources 
* Note: If individual account is created then it starts directly from project level

# Project Creation:
Click on My First Project > New Project > Fill Details Project Name > Can edit Project id with edit option > Location (Only in case of Organisation) > Create
Example Project Name: Keerthi-reddy-manda-project Project ID: keerthi-reddy-manda-project Project number: 231863212342 
* Select the project by clicking on Project name (It can be found in All)

## Project details will be as below  
# Project: 
Project is seggregated into
1. Project Name - Created during the project - User Friendly - Can be changed 
2. Project ID - Created during the project - Globally Unique - Cannot be changed (Editable only during creation)
3. Project Number - After the project creation - Globally Unique - Cannot be changed (Created by google) ** Unique & important 


# How to create a Virtual Machine (VM)?
To create a VM first enable Compute Engine 
Search Compute Engine > Enable 
After enabling the Compute Engine Create VM using Console 
Search VM > Select VM > Create Instance > Name*, Region*, Zone*, Machine Type > Create
Example: Name: vm-1, Region: us-central1, Zone: Any, Machine Type: e2-micro (2 vCPU, 1 CORE, 1GB memory)










