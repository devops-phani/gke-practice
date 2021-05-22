# gke-practice

### Install the GCP SDK
```
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list

sudo apt-get install apt-transport-https ca-certificates gnupg

curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -

sudo apt-get update && sudo apt-get install google-cloud-sdk -y

gcloud -v
```
- **Reference Documentation Links**
- https://cloud.google.com/sdk/docs/install

### Install kubectl
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

chmod +x kubectl

sudo mv kubectl /usr/local/bin/

kubectl version --client --short
```
- **Reference Documentation Links**
- https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/

Create the service account and give the required permissions to service account . Given owner access to project for testing 
This is not recommended 
### Login the gcloud with service account and enable the required api's
```
gcloud auth activate-service-account  gke-user@test-project.iam.gserviceaccount.com   --key-file=test.json --project=test-project

gcloud projects list

PROJECT_ID=test-project

gcloud projects list

gcloud config set project ${PROJECT_ID}

gcloud --project=${PROJECT_ID} \
    services enable\
    container.googleapis.com\
    containerregistry.googleapis.com\
    binaryauthorization.googleapis.com
```
- **Reference Documentation Links**
- https://cloud.google.com/kubernetes-engine/docs/how-to
### Create the GKE Regional Cluster

##### Get the GKE versions
```
gcloud container get-server-config --region us-central1
```
#### Get the machine-types
```
gcloud compute machine-types list --filter="zone:( us-central1-b us-central1-c )"
```
#### Create the cluster
```
gcloud container clusters create test-cluster \
--region=us-central1 \
--cluster-version=1.17.17-gke.7800 \
--labels=environment=dev,server=linux \
--no-enable-ip-alias \
--enable-shielded-nodes \
--disk-size=50 \
--disk-type=pd-standard \
--enable-autorepair \
--no-enable-autoupgrade \
--enable-stackdriver-kubernetes \
--no-enable-cloud-logging \
--image-type=UBUNTU \
--machine-type=e2-medium \
--max-surge-upgrade=1 \
--max-unavailable-upgrade=0 \
--node-labels=webserevr=nginx,app=frontend \
--node-locations=us-central1-a,us-central1-b \
--node-version=1.17.17-gke.7800 \
--num-nodes=1 \
--enable-autoscaling \
--max-nodes=4 \
--min-nodes=1 
```
- **Reference Documentation Links**
- https://cloud.google.com/sdk/gcloud/reference/container/clusters/create

#### Resize the cluster
```
gcloud container clusters resize test-cluster --num-nodes=1 --region=us-central1 --node-pool=default-pool
```
- **Reference Documentation Links**
- https://cloud.google.com/sdk/gcloud/reference/container/clusters/resize
#### Check the cluster operations history
```
gcloud container operations list
```
