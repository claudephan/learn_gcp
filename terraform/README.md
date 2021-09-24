1. Create a free GCP account.
2. Create a GKE cluster using Terraform with the following specifications:
    a. The cluster should be deployed as multi regional
    b. number of nodes: 3
    c. Install Airflow using the helm chart
    (https://airflow.apache.org/docs/helm-chart/stable/index.html)