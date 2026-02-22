# What is Google Cloud Storage(GCS)?

Cloud Storage is an object storage service in Google Cloud used to store unstructured data as objects inside buckets. An **object** is immutable data (a file of any format) stored in a **bucket**. Buckets can appear “folder-like” using **object name prefixes** (GCS is flat storage; folders are logical).

Buckets belong to a **project**. Projects can be grouped under an **organisation**. Projects, buckets, objects, and managed folders are Google Cloud resources.

### **Best use cases**
- Backups
- Logs
- Media files (images/videos)
- Data lakes
- Static website hosting

### **Key features**
- High durability
- Lifecycle rules
- Object versioning
- IAM-based access control
- Encryption at rest (default)


## Creating a bucket - Key decisions
Details to consider before creating a bucket 

### **Name:** 
Bucket names must be **globally unique**  

### **Location:** 
Choose where the data is stored. 
- **Multi-region:** Highest availability across largest area
- **Dual-region:** High availability and low latency across 2 regions
- **Region:** Lowest latency within a single region

### **Storage Classes**
- **Autoclass:** automatically optimizes object storage class based on access patterns (good when usage is unpredictable).
- **Default storage Class:** applies to objects unless overridden by object-level settings or lifecycle rules:
    1. **Standard:** frequently accessed data
    2. **Nearline:** backups / < once a month
    3. **Coldline:** DR / < once a quarter
    4. **Archive:** long-term / < once a year

Note: if you store data in a colder class but access it frequently, you may pay retrieval charges.

### **Access Control**
Use IAM 
 1. **Uniform:** recommended; IAM only at bucket level
 2. **Fine-grained:** object ACLs + IAM (legacy in many setups)

### **Data Protection**
- **Soft Delete:** Retains deleted objects for a retention period (depends on configuration/policy)
- **Object Versioning:** keeps older versions when objects are replaced/deleted (bucket-level setting)
- **Retention policy / bucket lock:** prevents deletion/modification for a period

### Data Encryption:
GCP encrypts data at rest by default. You can also use customer-managed encryption keys(CMEK) and customer-supplied encryption keys(CSEK).


### Commands

### Set PROJECT_ID 

```bash
export PROJECT_ID="$(gcloud config get-value project)"
echo "$PROJECT_ID"
```

### Create a bucket (CLI)

```bash
gcloud storage buckets create gs://BUCKET_NAME \
    --project="$PROJECT_ID" \
    --location=BUCKET_LOCATION \
    --default-storage-class=STORAGE_CLASS \
    --uniform-bucket-level-access \
    --soft-delete-duration=RETENTION_DURATION
```
Example 

```bash
gcloud storage buckets create gs://keerthi-bucket --location=us-central1

echo "hello" > gcpbatch26.txt
gsutil cp gcpbatch26.txt gs://keerthi-bucket/
gsutil ls gs://keerthi-bucket/
```

### Lifecycle Rules Example

Example: Move objects from Standard to Coldline after 30 days

```bash
gcloud storage buckets update gs://BUCKET_NAME \
  --lifecycle-file=lifecycle.json
```

gsutil ls without a bucket URL attempts to list buckets for the current project and requires permission + correct auth.

**Note:** gsutil ls gs://your-bucket lists objects in that bucket.
gsutil ls lists buckets accessible in the active project (and fails if auth/project is wrong).


To access bucket from VM need to have service account and its access 

### Create a service account in google
```bash
gcloud iam service-accounts create instance-storage --display-name="InstanceToStorage"
```

### Create a key for the service account
```bash
gcloud iam service-accounts keys create instancestorage.json --iam-account=instance-storage@$PROJECT_ID.iam.gserviceaccount.com
```

###  Add storage admin role to the service account 
```bash
gcloud projects add-iam-policy-binding $PROJECT_ID --member=serviceAccount:instance-storage@$PROJECT_ID.iam.gserviceaccount.com --role=roles/storage.admin
```

### Login into the VM in another session
```bash 
gcloud compute ssh --zone "us-central1-a" "$VM_NAME" --project "$PROJECT_ID"
```

### Set service account to the instance 
```bash
gcloud config set account instance-storage@$PROJECT_ID.iam.gserviceaccount.com
```

### Before activating the service account in the VM copy the instancestorage.json file from previous folder and add in VM 

### Activate the service account
```bash
gcloud auth activate-service-account instance-storage@$PROJECT_ID.iam.gserviceaccount.com --key-file=instancestorage.json
```

**⚠️ Best Practice:**
Avoid downloading service account keys in production environments.
Instead, attach the service account directly to the VM using:
--service-account flag during VM creation.


### Cleanup to avoid charges:

```bash
gsutil rm -r gs://BUCKET_NAME/
```

**Note:** `gsutil` is the legacy command-line tool for Cloud Storage.
Google now recommends using `gcloud storage` for new implementations.
