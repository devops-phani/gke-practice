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
