# Data Exploration in Dataplex

## 1. About

Data Exploration Workbench in Dataplex (DEW) helps you interactively query fully-governed, high-quality data with one-click access to Spark SQL scripts and Jupyter notebooks. It lets you collaborate across teams with built-in publishing, sharing, and searching of coding assets.

DEW provisions, scales, and manages the serverless infrastructure required to run your Spark SQL scripts and notebooks using user credentials. You can operationalize your work with one-click serverless scheduling from the workbench.

## 2. Capabilities in Data Exploration Workbench (DEW)

### 2.1. Spark SQL Workbench
**About:**<br> 
Explore data assets with Spark SQL for the SQL savvy users.<br> 


**When to use:**<br>
Query structured data in your data lake (Cloud Storage), at scale, when you cant load the data into BigQuery managed tables


### 2.2. PySpark Jupyter Notebooks as a Service

PySpark Jupyter notebooks that can be scheduled 

**When to use:**<br>
When you prefer a notebook - <br> 
(a) to author SQL, Spark, Python etc <br> 
(b) to query BQ, structured data in your data lake, databases<br> 
(c) to not only query, but also visualize using your favorite Python visualization libraries<br> 
(d) because you also need to share it<br> 
(e) as an IDE<br> 

**Positioning by persona:**<br>
(a) Data Engineer: Spark IDE, Apache Hive Metastore IDE <br> 
(b) Data Scientist: Experimentation - preprocessing, model training, tuning, batch scoring<br> 
(c) Data Analyst: Analyze data with a mix of SQL and programmatic logic, at scale<br> 
(d) Data Product Manager: Analyze data & ideate on data products to build, analyze usage and trends with data products<br> 
(e) Data Steward: Analyze data profile, data quality, data access, audit logs and more <br> 

## 3. Terminology Levelset

### 3.1. Dataproc Metastore Service

### 3.2. Environment

### 3.3. Default Environment

### 3.4. Session

## 4. Getting started with DEW - what's involved



## 5. The lab

### 5.1. IAM permissions

We will use the customer-sa@ service account to create the DPMS and environments. Lets grant the service account, the requisite permissions by running the below in Cloud Shell-

```
PROJECT_ID=`gcloud config list --format "value(core.project)" 2>/dev/null`

CUSTOMER_UMSA_FQN="customer-sa@${PROJECT_ID}.iam.gserviceaccount.com"

gcloud projects add-iam-policy-binding $PROJECT_ID --member=serviceAccount:$CUSTOMER_UMSA_FQN \
--role="roles/metastore.admin"

gcloud projects add-iam-policy-binding $PROJECT_ID --member=serviceAccount:$CUSTOMER_UMSA_FQN \
--role="roles/metastore.metadataEditor"

gcloud projects add-iam-policy-binding $PROJECT_ID --member=serviceAccount:$CUSTOMER_UMSA_FQN \
--role="roles/metastore.metadataUser"
```

### 5.2. Create a Dataproc Metastore Service

a) In Cloud Shell scoped to your project, create a DPMS, by running the command below-
```
PROJECT_ID=`gcloud config list --format "value(core.project)" 2>/dev/null`
PROJECT_NBR=`gcloud projects describe $PROJECT_ID | grep projectNumber | cut -d':' -f2 |  tr -d "'" | xargs`
DPMS_NM="dpms-${PROJECT_NBR}"
LOCATION="us-central1"

gcloud metastore services create $DPMS_NM \
  --location=$LOCATION \
  --impersonate-service-account=$CUSTOMER_UMSA_FQN
```

b) Enable the gRPC endpoint of the metastore
```
curl -X PATCH \
-H "Authorization: Bearer $(gcloud auth print-access-token)" \
-H "Content-Type: application/json" \
"https://metastore.googleapis.com/v1beta/projects/$PROJECT_ID/locations/$LOCATION/services/$DPMS_NM?updateMask=hiveMetastoreConfig.endpointProtocol" \
-d '{"hiveMetastoreConfig": {"endpointProtocol": "GRPC"}}'
```

### 5.3. Create an Environment

### 5.4. Analyze data in DEW SQL workbench


### 5.5. Analyze data from a DEW Notebook


<hr>
