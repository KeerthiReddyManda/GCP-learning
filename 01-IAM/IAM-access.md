# How to access the project?

## IAM : Identity & Access Management - provides access to project - It helps to control who can do what on which resources
1. Authentication 
2. Authorization 

-- Who? 
1. People Accounts (Human Accounts)
2. Service Accounts (Manchine / Non-Human Accounts)

* People Accounts:
1. Personal accounts (any mail-id gmail or non gmail)
2. Workspace accounts (google workspace accounts accessible to all google apps/cloud)
3. Cloud identity (only console access)
4. Group's (DL's i.e., Directory lists)
5. Special accounts (No mail id, seperate login not through console) all authenticated users & all users 

* Service accounts: 
1. User Managed: 1 is created by user & other is cretaed by google automatically but both are managed by user. 
> User Created - service-account-name@project-id.iam.gserviceaccount.com 
> Google created (automatically) - project-number-compute@developer.gserviceaccount.com 
2. Google Managed : Managed by google for internal purpose 

-- Can do what? 
Who can do what is defined by the roles which is a collection of permisssions and role is assigned to a person. 
Note: Permissions cannot be assigned individually to a user they should be a part of role. 
Role(Collection of permissions)

#### Different types of Roles:
##### Primitive Roles (Basic roles) - Legacy - Project Level
1. Owner - Full Control including IAM Management
2. Editor - Create, Modify except IAM & billing
3. Viewer - Only view access to everything in the project
4. Browser - Gives read-only access to see the project hierarchy and its IAM Policies, but no permission to view or interact with the actual resources(like VM's , storage buckets) inside the project
##### Pre-defined Roles - Granular roles for specific services(non editable as they are pre defined by google)
##### Custom Roles - Mix & Match of pre defined roles. Even more granular compared to pre defined roles

-- On which resources? 
Everything that can be accessed in GCP is a resource

#### How to provide access to a user?
* Using Console: Search IAM > Select IAM > Click Grant Access > Fill Details New Principals - user id & Assign role - select a role > Save

#### How to create a role 
```bash
gcloud iam roles create role_id \
  --organisation=organisation_id \
  --project=project_id \
  --file=file_id
```

## How to assign a role to a user 
>Console: Grant access option 
```bash
gcloud projects add-iam-policy-binding project_id \
  --member=user:user_id(principal) \
  --role=roles/role_id
```


* we create a file with the role details instead of manually entering all the permissions in the command, this helps in future usage 

* How to create a file and what details are required?
vi file_id > Insert > Insert all the role details such as 

```bash
title: "custom_role"
description: "Custom Role to create Instances"
stage: "GA"
includedPermissions:
  - compute.instances.create 
  - compute.acceleratorTypes.list
  - compute.disks.create
  - compute.disks.list
  - compute.instances.create
  - compute.instances.list
  - compute.instances.setServiceAccount
  - compute.machineTypes.list
  - compute.networks.get
  - compute.networks.list
  - compute.projects.get
  - compute.regions.list
  - compute.subnetworks.get
  - compute.subnetworks.list
  - compute.subnetworks.use
  - compute.subnetworks.useExternalIp
  - compute.zones.list
```

if any additions or modifications to the file can be done with the same option 

To view file details 
```bash
cat file_id
```

To make changes directly in the command line then use option --add-permissions or --remove-permissions 

# How to assign multiple roles / role binding to a user?
```bash
gcloud projects add-iam-policy-binding project_id \
  --member=user:user_id \
  --role=roles/role_id
```
Multiple roles can be assigned in the same way 

# How to check what roles are assigned to a user?
```bash
gcloud projects get-iam-policy PROJECT_ID \
  --flatten="bindings[].members" \
  --filter="bindings.members:user:USER_EMAIL" \
  --format="table(bindings.role)"
```
