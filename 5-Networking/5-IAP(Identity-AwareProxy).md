# IAP - Identity Aware Proxy
The Identity-Aware Proxy (Cloud IAP) controls access to your cloud applications and VMs running on Google Cloud Platform(GCP).
IAP is a Google Cloud service that allows you to securely access resources based on identity, not network location.

**Using IAP, you can:**
- SSH into VM instances without a public IP
- Avoid exposing port 22 to the internet
- Control access using IAM roles
- Eliminate the need for VPN

## Why use IAP for VM access?
Traditional SSH access requires:
- A public IP
- Firewall rule open to 0.0.0.0/0
- Higher security risk

**With IAP:**
- ✅ No public IP required
- ✅ No inbound internet exposure
- ✅ Access controlled via IAM 
- ✅ Fully auditable


#### IAM Permissions Required for IAP SSH
To SSH into a VM using IAP, the user must have:

**Required roles:**
- roles/iap.tunnelResourceAccessor
- roles/compute.instanceAdmin.v1 or roles/compute.osLogin
- roles/iam.serviceAccountUser (recommended)


```bash
gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member="user:USER_EMAIL" \
  --role="roles/iap.tunnelResourceAccessor"
```

```bash
gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member="user:USER_EMAIL" \
  --role="roles/compute.instanceAdmin.v1"
```

**How IAP SSH works:**

User → IAM Authentication → IAP Tunnel → VM (private IP)


### Create Firewall Rule for IAP SSH
**IAP uses a fixed IP range:**

```bash
35.235.240.0/20
```


#### Create VPC, Subnet, VM & firewall for IAP SSH 

```bash
export PROJECT_ID="your-project-id"
gcloud config set project $PROJECT_ID
export VPC_NAME="secure-vpc"
export SUBNET_NAME="secure-subnet"
export REGION_ID="us-central1"
export ZONE_ID="us-central1-a"
export CIDR_RANGE=10.9.0.0/24
export MACHINE_TYPE="e2-micro"
export INSTANCE="secure-vm"
```

```bash
gcloud compute networks create $VPC_NAME --subnet-mode=custom

gcloud compute networks subnets create $SUBNET_NAME \
  --region=$REGION_ID \
  --range=$CIDR_RANGE \
  --network=$VPC_NAME 

gcloud compute instances create $INSTANCE \
  --zone=$ZONE_ID \
  --machine-type=$MACHINE_TYPE \
  --subnet=$SUBNET_NAME \
  --tags=iap-ssh \
  --no-address

gcloud compute firewall-rules create allow-iap-ssh \
  --network=$VPC_NAME \
  --direction=INGRESS \
  --action=ALLOW \
  --rules=tcp:22 \
  --source-ranges=35.235.240.0/20 \
  --target-tags=iap-ssh \
  --description="Allow SSH via IAP only"
```

#### SSH into VM Using IAP

Use the following command from your local terminal:

```bash
gcloud compute ssh $INSTANCE --zone=$ZONE_ID --tunnel-through-iap --project=$PROJECT_ID
```

**Best Practices:**
- Disable external IPs on sensitive VMs
- Use IAP instead of opening port 22
- Combine IAP with OS Login
- Audit access using Cloud Logging


### **Cleanup Section** (To Avoid Charges)

```bash
gcloud compute firewall-rules delete allow-iap-ssh --quiet
gcloud compute instances delete $INSTANCE --zone=$ZONE_ID --quiet
gcloud compute networks subnets delete $SUBNET_NAME --region=$REGION_ID --quiet
gcloud compute networks delete $VPC_NAME --quiet
```


