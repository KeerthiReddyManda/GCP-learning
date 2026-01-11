# Virtual Private Cloud (VPC)
A Virtual Private Cloud (VPC) is a logicaly isolated network in Google Cloud where you can launch and manage resources such as virtual machines, subnets and firewall rules.

## Prerequisites
Set the following environment variables before running the commands:

```bash
export VPC_NAME=my-custom-vpc
export SUBNET_NAME=my-subnet
export REGION_ID=us-central1
export ZONE_ID=us-central1-a
export CIDR_RANGE=10.0.0.0/24
export MACHINE_TYPE=e2-micro
```

## Create a custom-mode VPC
```bash
gcloud compute networks create $VPC_NAME \
  --subnet-mode=custom
echo "Network created successfully..."
```

##  List VPC networks

```bash
gcloud compute networks list
```

## Describe a VPC

```bash
gcloud compute networks describe $VPC_NAME
```

## Delete a VPC

```bash
gcloud compute networks delete --network=$VPC_NAME --quiet
```

## Create a Subnet in the VPC

```bash
gcloud compute networks subnets create $SUBNET_NAME \
  --network=$VPC_NAME \
  --region=$REGION_ID \
  --range=$CIDR_RANGE
echo "Subnet created successfully..."
```

## List Subnets

```bash
gcloud compute networks subnets list --network=$VPC_NAME
```

## Describe a Subnet

```bash
gcloud compute networks subnets describe $SUBNET_NAME --region=$REGION_ID
```

## Delete a Subnet

```bash
gcloud compute networks subnets delete $SUBNET_NAME --region=$REGION_ID --quiet
```

## Create VM Instances in the Subnet

## Create an Instance with External IP

```bash
gcloud compute instances create $INSTANCE1_ID \
  --zone=$ZONE_ID \
  --subnet=$SUBNET_NAME \
  --machine-type=$MACHINE_TYPE \
```

## Create an Instance without external IP

```bash
gcloud compute instances create $INSTANCE2_ID \
  --zone=$ZONE_ID \
  --subnet=$SUBNET_NAME \
  --machine-type=$MACHINE_TYPE \
  --no-address
```

## Create an Instance with Network Tag deny

```bash
gcloud compute instances create $INSTANCE3_ID \
  --zone=$ZONE_ID \
  --subnet=$SUBNET_NAME \
  --machine-type=$MACHINE_TYPE \
  --tags=deny
```

## Create an Instance with Network Tag secure-vm

```bash
gcloud compute instances create $INSTANCE4_ID \
  --zone=$ZONE_ID \
  --subnet=$SUBNET_NAME \
  --machine-type=$MACHINE_TYPE \
  --tags=secure-vm
```

## List VM Instances 

 ```bash
 gcloud compute instances list
 ```

## Describe a VM Instance

```bash
gcloud compute instances describe $INSTANCE_ID --zone=$ZONE_ID
```

## Delete an VM Instance

```bash
gcloud compute instances delete $INSTANCE_ID --zone=$ZONE_ID --quiet
```

## Add Network Tags to an Existing VM

```bash
gcloud compute instances add-tags $INSTANCE1_ID \
  --zone=$ZONE_ID \
  --tags=allow-http
```
