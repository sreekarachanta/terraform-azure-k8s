## Deploys a Kubernetes cluster on AKS with monitoring support through Azure Log Analytics
This Terraform module deploys a Kubernetes cluster on Azure using AKS (Azure Kubernetes Service) and adds support for monitoring with Log Analytics.


## Pre-requisites

1. Install Terraform
2. Install Azure cli(azcli)
3. Create a resource group manually in the Azure portal that we will be using in the next steps.

## Procedure:

Clone the repo and update the below values based on your requirement,

In variables.tf

Update the `agents_min_count` and `agents_max_count` values to `null` if you do not want to enable autoscaling( `enable_auto_scaling` will be `false`). If you want to enable autoscaling(default), then update the appropriate number with a minimum of 1 for the `min count` to any number for the `max count`.

Once you are done with above changes,

`terraform init`

## To save the plan
`terraform plan -out out.plan`      

## To apply the plan
`terraform apply out.plan`   

## Note:
 You will be prompted to enter the prefix for the resources and resource-group name . Please enter them accordingly.


Upon completion of creation of resources, give the below command to access the cluster.

az aks get-credentials --name <cluster-name> --resource-group <resource-group-name>

## Example:

az aks get-credentials --name poc-aks --resource-group poc-rg

## You are now ready to access the cluster resources. Try the below to see the cluster resources,

kubectl get nodes

## To Destroy the entire setup

terraform destroy.

