# Cloud Engineering - Fundamentals

## Why Companies are moving to cloud?

### Business Perspective
1. Shift from **Capital Expenditure (CapEx)** to **Operational Expenditure (OpEx)**
2. **Scalability** based on demand
3. Built-in **High Availability** & **Disaster Recovery**
   
### Tech Perspective
1. Rapid provision of resources
2. Automation using **Infrastructure as Code (IaC)**
3. Global Scalability and performance 

## What does a Cloud Engineer Team do?
Cloud teams design, build, manage and maintain cloud based services.

**Typical roles include:**

1. Cloud Architect
2. Cloud Engineer
3. Support / Operations Team
4. Interns / Trainees

## Cloud Architect vs Cloud Engineer
- **Cloud Architect** - Designs the overall cloud solution and architecture
- **Cloud Engineer** - Implements, operates and maintains the designed solution

## What is Cloud computing?
Cloud computing is the **delivery of computing services** such as servers, storage, databases, networking and software over the internet, allowing access anytime & from any place.

## Major Cloud service providers:
1. Google Cloud Platform(GCP)
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

## Creating a Google Cloud account 
* Step 1: Visit https://console.cloud.google.com
* Step 2: Sign in using existing Google account or create a new account
* Step 3: Accept the Terms of Service
* Step 4: Activate the free trial (Google provides $300 in free credits (Valid for 90 days or until credits are exhausted)
  - Step 1 of 2 Account Information > Select Country > Accept Terms > Continue 
  - Step 2 of 2 Payment Verification Information > Contact Information > Add name & address > Fill all the required details Individual or Organisation > Click Save > Payment Method > Select payment option > Credit/Debit Card or Bank Account > Provide Details (Note: No charges without approval) > Save > Start Free 


## Cloud Infrastructure & Hierarchy

### What is Cloud Infrastructure?
Cloud infrastructure includes:
  - Virtual Machines (Compute Engine)
  - Virtual Networks (VPC, Subnets)
  - Storage (Disks, Buckets)
  - Firewalls & IAM
  - Load balancers

### Regions & Zones
  - **Region**: An independent geographical location containing multiple data centres
  - **Zone**: An isolated area within the region
**Example:**
  - Regions: ~40+
  - Zones: ~120+
  - Most regions have 3 zones
  - us-central1 has 4 zones

### Choosing a Region or Zone
Selection depends on:
  - Compliance Requirements
  - Customer location
  - Latency
  - High availability and disaster recovery needs

### Deployment models: 

Zonal < Regional < Multi Regional < Dual Regional 

### GCP Resource Hierarchy
Organisation
  └── Folders (Optional)
        └── Projects (Mandatory)
              └── Resources
              
* Note: Individual account start directly at the project level.

### Project Creation in GCP
**Steps:**
  1. Click My First Project
  2. Select New Project
  3. Enter Project Name
  4. Edit Project ID(only during project creation)
  5. Select Location(Organisation only)
  6. Click Create
     
#### Project Identifiers
  1. **Project Name** - Created during the project - User Friendly, editable
  2. **Project ID** - Created during the project - Globally Unique, immutable (editable only during creation)
  3. **Project Number** - After the project creation - Auto generated, immutable, created and used internally by Google 


#### Creating a Virtual Machine (VM)
**Steps:**
  1. Enable Compute Engine 
  2. Navigate to Compute Engine **->** VM Instances
  3. Click Create Instance
  4. Configure:
     - Name
     - Region & Zone
     - Machine Type
  5. Click Create

**Example:**
  - Name: vm-1
  - Region: us-central1
  - Machine Type: e2-micro (2 vCPU, 1 CORE, 1GB memory)

**Note:** This is basic VM creation using Console will learn further in detail in the coming chapters.





