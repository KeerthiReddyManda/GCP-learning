# Virtual Private Cloud (VPC)
A Virtual Private Cloud (VPC) is a logically isolated network in Google Cloud where you can launch and manage resources such as virtual machines, subnets and firewall rules.

In GCP, a VPC network is global, but subnets are regional.

**A VPC contains:**

- Subnets (IP ranges in regions)
- Routes (how traffic moves)
- Firewall rules (allow/deny traffic)
- VPN / Interconnect / Peering (connectivity options)

## Auto Mode vs Custom Mode VPC

##### ✅ Auto Mode VPC
An auto mode VPC automatically creates:
  - One subnet per region (Google-managed)
  - Predefined IP ranges (like 10.128.0.0/9 split across regions)

**Use when:**
- You want quick setup for labs
- You don’t care about controlling subnet ranges

**Limitations:**
- Less control over subnet CIDRs and network design
- Not ideal for production architecture

#### ✅ Custom Mode VPC
A custom mode VPC requires you to manually create subnets:
  - You define the subnet name, region, and CIDR range
  - Full control over IP design

**Use when:**
- You want a clean enterprise-ready network design
- You need specific IP planning (peering, VPN, shared VPC, etc.)


## Prerequisites
Set the following environment variables before running the commands:

```bash
export VPC_NAME=my-custom-vpc
export SUBNET_NAME=my-subnet
export REGION_ID=us-central1
export ZONE_ID=us-central1-a
export CIDR_RANGE=10.0.0.0/24
export MACHINE_TYPE=e2-micro
export INSTANCE1_ID=vm-1
export INSTANCE2_ID=vm-2
export INSTANCE3_ID=vm-deny
export INSTANCE4_ID=vm-secure
```

#### Create a custom-mode VPC
```bash
gcloud compute networks create $VPC_NAME \
  --subnet-mode=custom
echo "Network created successfully..."
```

#### List VPC networks

```bash
gcloud compute networks list
```

#### Describe a VPC

```bash
gcloud compute networks describe $VPC_NAME
```

#### Delete a VPC

```bash
gcloud compute networks delete $VPC_NAME --quiet
```


### **Subnet (Subnetwork)**

A subnet is a regional segment of a VPC that defines an IP range for resources in that region.

**Key points:**

- Subnets are regional (example: us-central1)
- VMs in a subnet get internal IPs from the subnet CIDR
- A VPC can have multiple subnets (even in the same region)

**Example:**

- Subnet CIDR: 10.0.0.0/24
- Usable internal IPs: 10.0.0.1 to 10.0.0.254


### **CIDR Range (IP Range)**

**CIDR (Classless Inter-Domain Routing)** defines a block of IP addresses.

**Example:**

10.0.0.0/24

**Meaning:**

- /24 means 256 total IPs
- **Network address:** 10.0.0.0
- **Broadcast address:** 10.0.0.255 (GCP reserves the first 4 and last 1 IPs in each subnet)
- **Usable hosts:** 10.0.0.4 to 10.0.0.254

Note: GCP reserves some IPs in each subnet; not all usable host IPs are assignable.

#### **Common CIDRs:**

- /24 → 256 IPs
- /20 → 4096 IPs
- /16 → 65,536 IPs

#### Create a Subnet in the VPC

```bash
gcloud compute networks subnets create $SUBNET_NAME \
  --network=$VPC_NAME \
  --region=$REGION_ID \
  --range=$CIDR_RANGE
echo "Subnet created successfully..."
```

#### List Subnets

```bash
gcloud compute networks subnets list --network=$VPC_NAME
```

#### Describe a Subnet

```bash
gcloud compute networks subnets describe $SUBNET_NAME --region=$REGION_ID
```

#### Delete a Subnet

```bash
gcloud compute networks subnets delete $SUBNET_NAME --region=$REGION_ID --quiet
```


### **VM Instance**

A VM instance is a virtual machine created in Compute Engine.
When you create a VM, you typically choose:

 - Region & zone
 - Machine type (CPU/RAM)
 - VPC + subnet (network placement)
 - Whether it needs an external IP
 - Network tags (optional)

### **Internal IP**

An **internal IP** is a **private IP** address assigned to a VM from the subnet CIDR.

**Used for:**

- VM-to-VM communication inside the VPC
- Communication over VPC peering/VPN (private networking)

**Example:**

VM internal IP: 10.0.0.5

* **Private IP address ranges are reserved blocks (10.0.0.0-10.255.255.255, 172.16.0.0-172.31.255.255, 192.168.0.0-192.168.255.255) for internal networks**


### **External IP**

An **external IP** is a **public IP** assigned to a VM for internet connectivity.

**Used for:**
- SSH/RDP from your laptop
- Hosting websites/services directly on a VM
- Internet-facing access (when allowed by firewall)

**Notes:**

External IPs can be **ephemeral** (changes on stop/start) or **static** (reserved)

#### Create VM Instances in the Subnet

#### Create an Instance with External IP

```bash
gcloud compute instances create $INSTANCE1_ID \
  --zone=$ZONE_ID \
  --subnet=$SUBNET_NAME \
  --machine-type=$MACHINE_TYPE
```

#### Create an Instance without external IP

```bash
gcloud compute instances create $INSTANCE2_ID \
  --zone=$ZONE_ID \
  --subnet=$SUBNET_NAME \
  --machine-type=$MACHINE_TYPE \
  --no-address
```

### **Firewall Rules** (Detail in [Firewall-rules](Networking/2-Firewall-rules.md))

Firewall rules in GCP control network traffic to and from VMs in a VPC.

**Two directions:**

- **Ingress:** traffic coming into the VM
- **Egress:** traffic leaving from the VM

**Firewall rules are:**

- Stateful (return traffic is automatically allowed)
- Evaluated by priority (lower number = higher priority)
- Applied at the VPC level, but enforced on VMs

Example allow SSH:

```bash
--direction=INGRESS --action=ALLOW --rules=tcp:22
```

### **Network Tags**

Network tags are labels applied to VM instances to control which firewall rules apply to them.
A firewall rule can use --target-tags=...

Only VMs with that tag are affected by the rule

**Example:**
VM tag: allow-http

Firewall rule targets: 

```bash
--target-tags=allow-http
```

Add tag to an existing VM:

```bash
gcloud compute instances add-tags VM_NAME \
  --zone=ZONE \
  --tags=allow-http
```

**Why tags are useful:**
- Apply firewall rules to specific VMs without changing IP ranges
- Group workloads (web, db, bastion, private-vm)


#### Create an Instance with Network Tag deny

```bash
gcloud compute instances create $INSTANCE3_ID \
  --zone=$ZONE_ID \
  --subnet=$SUBNET_NAME \
  --machine-type=$MACHINE_TYPE \
  --tags=deny
```

#### Create an Instance with Network Tag secure-vm

```bash
gcloud compute instances create $INSTANCE4_ID \
  --zone=$ZONE_ID \
  --subnet=$SUBNET_NAME \
  --machine-type=$MACHINE_TYPE \
  --tags=secure-vm
```

#### List VM Instances 

 ```bash
 gcloud compute instances list
 ```

#### Describe a VM Instance

```bash
gcloud compute instances describe $INSTANCE_ID --zone=$ZONE_ID
```

#### Delete an VM Instance

```bash
gcloud compute instances delete $INSTANCE_ID --zone=$ZONE_ID --quiet
```

#### Add Network Tags to an Existing VM

```bash
gcloud compute instances add-tags $INSTANCE1_ID \
  --zone=$ZONE_ID \
  --tags=allow-http
```

### **Cleanup Section** (To Avoid Charges)

```bash
gcloud compute instances delete $INSTANCE1_ID $INSTANCE2_ID $INSTANCE3_ID $INSTANCE4_ID --zone=$ZONE_ID --quiet
gcloud compute networks subnets delete $SUBNET_NAME --region=$REGION_ID --quiet
gcloud compute networks delete $VPC_NAME --quiet
```
