1. Create a free GCP account.
2. Create a GKE cluster using Terraform with the following specifications:
- The cluster should be deployed as multi regional
- number of nodes: 3
- Install Airflow using the helm chart (https://airflow.apache.org/docs/helm-chart/stable/index.html)


## Steps:
1. ensure yoy have gclod SDK
    - brew install --cask google-cloud-sdk
    - gcloud init (/usr/local/Caskroom/google-cloud-sdk/latest/google-cloud-sdk/install.sh to add to path if needed)
    - gcloud auth application-default login (add account to Application Default Credentials (ADC) or terraform to access gCloud)
    - gcloud -v
2. git clone https://github.com/hashicorp/learn-terraform-provision-gke-cluster
    - vpc.tf provisions a VPC and subnet. A new VPC is created for this tutorial so it doesn't impact your existing cloud environment and resources. This file outputs region.
    - gke.tf provisions a GKE cluster and a separately managed node pool (recommended). Separately managed node pools allows you to customize your Kubernetes cluster profile — this is useful if some Pods require more resources than others. The number of nodes in the node pool is defined also defined here.
    - terraform.tfvars is a template for the project_id and region variables.
    - versions.tf sets the Terraform version to at least 0.14.
3. terraform init
4. Enable Compute Engine API and Kubernetes Engine API on GCP UI
5. terraform apply
6. Setup kubectl config to acess cluster
    - gcloud container clusters get-credentials pacific-byte-327001-gke --region=us-west1
    - kubectl config get-contexts
    - kubectl config set-context {gcp context name}
    - kubectl config current-context
7. Airflow
    - kubectl create namespace airflow
    - helm repo add apache-airflow https://airflow.apache.org
    - helm install airflow apache-airflow/airflow --namespace airflow
8. Validate
    - kubectl get all -n airflow
    - kubectl port-forward svc/airflow-webserver 8080:8080 --namespace airflow  (login is admin/admin)


# Finding
1. Setting region to an actual region (us-west1) vs an actual zone (us-west1-a) will make cluster a regional cluster
    - Node count of 1 results in 3 nodes spun up, one in each zone
2. Ingress not setup for airflow in helm chart, thus port forward is needed. Next steps could be setting up a lb or virtual server
3. Multiregional cluster not supported by default. What we have is multizonal cluster. Possibility of deploying multiple GKE in differnt regions and putting them behind a LB
    - https://medium.com/swlh/going-multi-regional-in-google-cloud-platform-18495a833838
    - https://github.com/GoogleCloudPlatform/terraform-google-examples/blob/master/example-gke-k8s-multi-region/main.tf



## Console Output 
NAME: airflow
LAST DEPLOYED: Thu Sep 23 22:03:36 2021
NAMESPACE: airflow
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Thank you for installing Apache Airflow 2.1.2!

Your release is named airflow.
You can now access your dashboard(s) by executing the following command(s) and visiting the corresponding port at localhost in your browser:

Airflow Webserver:     kubectl port-forward svc/airflow-webserver 8080:8080 --namespace airflow
Flower dashboard:      kubectl port-forward svc/airflow-flower 5555:5555 --namespace airflow
Default Webserver (Airflow UI) Login credentials:
    username: admin
    password: admin
Default Postgres connection credentials:
    username: postgres
    password: postgres
    port: 5432

You can get Fernet Key value by running the following:

    echo Fernet Key: $(kubectl get secret --namespace airflow airflow-fernet-key -o jsonpath="{.data.fernet-key}" | base64 --decode)