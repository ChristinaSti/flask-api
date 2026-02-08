# Flask API deployed on Google Cloud Run using terraform

## Project Description
This is an Infrastructure as Code (IaC) project that uses Terraform to deploy a Cloud Run service on Google Cloud Platform (GCP).

## Current State
- Provisions a GCP project with billing configured
- Enables required APIs (Cloud Run, Cloud Build, Artifact Registry)
- Deploys a demo container (gcr.io/cloudrun/hello) to Cloud Run
-Configures public access via IAM

## Tech Stack
- **Terraform** (~> 1.14.4) for infrastructure provisioning
- **Google Cloud Platform**:
    - Cloud Run (serverless container hosting)
    - Cloud Build
    - Artifact Registry

## Purpose
A learning/demo project for deploying containerized applications to GCP Cloud Run using Terraform. The infrastructure is ready to be extended with a custom Flask API container image.

## Prerequisites
- local installations:
    - terraform
    - gcloud
- run `gcloud auth login`
    - authenticates you to Google Cloud for CLI work
    => gcloud commands are executed using your identity and permissions are based on your IAM roles
- run `gcloud auth application-default login`:
    - creates Application Default Credentials (ADC) on your machine
    - allows libraries to authenticate automatically 
- have an existing billing account on GCP
- the account must have the role `roles/resourcemanager.projectCreator`
    - check it with `gcloud organizations get-iam-policy <YOUR_ORGANIZATION_ID>`

## Getting started

- run `terraform init` (in the terraform directory)
    - initialize a working directory containing terraform configuration files
    - first command to run after writing a new configuration
- run `terraform plan`
    - show a plan of changes that will be made to match the resources to the configuration
- run `terraform apply`
    - checks the configuration and the actual state and makes the changes to your infrastructure resources to match the two (create/modify/destroy)

## View created resources
Note: you can leave out the corresponding flags in the following if you set the default region and project_id beforehand via:
- `gcloud config set project <project_id>`
- `gcloud config set compute/region <region>`
1. created project: 
- `gcloud projects describe flask-api-001`
- https://console.cloud.google.com/home/dashboard?project=<project_id>

2. List all Cloud Run services in your project:
- `gcloud run services list --region=<region> --project=<project_id>`
- Cloud Console -> Cloud Run -> Services

3. Check IAM policy on the service
`cloud run services get-iam-policy <service_name> --region=<region> --project=<project_id>`

4. Get the service URL: 
- `gcloud run services describe flask-api --region=<region> --project=<project_id> --format='value(status.url)'`

5. Enabled APIs:
- `gcloud services list --enabled --project=<project_id>`
- https://console.cloud.google.com/apis/dashboard?project=<project_id>

