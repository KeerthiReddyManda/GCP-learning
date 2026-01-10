# How to access a GCP project (IAM Basics)?

## IAM - Identity & Access Management 
IAM controls ***who can do what on which resources*** in a GCP project
1. **Authentication** - Who are you? 
2. **Authorization** - What are you allowed to do?

To view IAM Policy for a project 

```bash
gcloud projects get-iam-policy PROJECT_ID
```

## Who can access a project?
### 1️⃣ User (Human) Accounts

#### Types of user identities:
1. **Google Accounts**
   - Personal accounts (Gmail or non-Gmail email)
2. **Google Workspace Accounts**
   - Managed by an organization
   - Access to Google Workspace + GCP
3. **Cloud Identity Accounts**
   - Managed identity
   - Console access only (no Gmail)
4. **Groups**
   - Collection of users (e.g., mailing lists / DLs)
5. **Special Identities**
   - `allAuthenticatedUsers` – Any authenticated Google account
   - `allUsers` – Anyone on the internet (public access)

---

### 2️⃣ Service Accounts (Non-human / Machine identities)

Service accounts are used by applications, VMs, and services.

#### Types of service accounts:

**User Managed**
  - **User Created** - Created & managed by user
    Represented as : service-account-name@project-id.iam.gserviceaccount.com
  - **Google Created** - Created by Google & managed by user
    Represented as : project-number-compute@developer.gserviceaccount.com
    
**Google Managed** 
Created & Managed by google for internal purposes.

## Can do what? 

### What can be accessed? 
Access means the ability to perform actions (permissions) on GCP resources.

Roles and Permissions
In GCP, access is controlled using roles.
A role is a collection of permissions that defines what actions a user, group, or service account can perform on resources.

Note: Permissions cannot be assigned individually to a user they should be a part of role. 

### Different types of Roles:
**Primitive Roles (Basic roles)** - Legacy - Project Level
1. Owner - Full Control, including IAM Management
2. Editor - Create and Modify, except IAM & billing
3. Viewer - Read-only access to all resources
4. Browser - View project hierarchy and IAM policies, but no access to resource data (such as VM's or storage buckets)

**Pre-defined Roles** 
- Granular roles designed for specific GCP services
- Managed and maintained by Google
- Non editable

**Custom Roles** 
- User-defined roles
- Built by selecting specific permissions
- More granular than pre defined roles

## On which resources? 

Everything that can be accessed in GCP is a resource, including
- Projects
- VMs
- Storage buckets
- Databases
- Networks
- APIs and services

## How to provide access to a user?

✅ 1️⃣ Using the Google Cloud Console (UI)
Steps:
1. Open Google Cloud Console
2. Navigate to IAM & Admin - IAM
3. Click Grant Access
4. Enter the user's email address
5. Select the required role(s)
6. Click Save

✅ 2️⃣ Using the gcloud CLI (Command Line)
Access can be granted programmatically using IAM policy bindings.

```bash
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member=user:USER_EMAIL \
  --role=roles/ROLE_NAME
```

✅ 3️⃣ Granting Access via Groups (Best Practice)
Instead of assigning roles directly to users, roles can be assigned to groups.

```bash
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member=group:team@example.com \
  --role=roles/editor
```

✅ 4️⃣ Using Service Accounts (Application Access)
Applications and VMs access resources using service accounts.

```bash
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member=serviceAccount:SA_NAME@PROJECT_ID.iam.gserviceaccount.com \
  --role=roles/storage.objectViewer
```

✅ 5️⃣ Granting Access at Different Resource Levels
Roles can be granted at multiple levels:
- Organization
- Folder
- Project
- Resource (e.g., bucket, VM)
Example (resource-level access):

```bash
gcloud storage buckets add-iam-policy-binding BUCKET_NAME \
  --member=user:USER_EMAIL \
  --role=roles/storage.objectViewer
```

✅ 6️⃣ Using Custom Roles
When predefined roles are insufficient, create and assign custom roles.

```bash
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member=user:USER_EMAIL \
  --role=projects/PROJECT_ID/roles/customRoleId
```

#### How to create a custom role?

```bash
gcloud iam roles create customRoleId \
  --project=PROJECT_ID \
  --file=custom-role.yaml
```
Then assign it to the user


### Custom role YAML file example

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
Save the file and quit the editor


To view file contents:

```bash
cat file_id
```

To modify permissions directly using CLI, use:

**Add Permissions**
```bash
gcloud iam roles update ROLE_ID --project=PROJECT_ID --add-permissions="required permissions"
```
**Remove Permissions**
```bash
gcloud iam roles update ROLE_ID --project=PROJECT_ID --remove-permissions="required permissions"
```

####  How to assign multiple roles to a User

```bash
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member=user:user_id \
  --role=roles/role_id
```
Multiple roles can be assigned by running the command multiple times.

#### How to check what roles are assigned to a User

```bash
gcloud projects get-iam-policy PROJECT_ID \
  --flatten="bindings[].members" \
  --filter="bindings.members:user:USER_EMAIL" \
  --format="table(bindings.role)"
```


## Best practices
- Prefer granting roles to **groups** instead of individual users.
- Follow **least privilege** (grant minimum access needed).
- Avoid primitive roles in production (Owner/Editor/Viewer).
- Review IAM policies regularly.

