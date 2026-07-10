# mlops-bootcamp-mlflow-aws

This project demonstrates how to run MLflow on an AWS EC2 instance while keeping the rest of the machine learning workflow separate from the infrastructure layer.

## Overview

The idea is to host MLflow on AWS EC2 and use it as a tracking server for experiments, parameters, metrics, and model artifacts. In this setup, only MLflow is deployed on the cloud server, not the whole data science project.

## AWS Setup

1. Sign in to the AWS console.
2. Create an IAM user with AdministratorAccess.
3. Configure the AWS CLI locally by running:
   ```bash
   aws configure
   ```
4. Create an S3 bucket to store MLflow artifacts.
5. Launch an Ubuntu EC2 instance and open port 5000 in the security group settings.

## EC2 Setup

Run the following commands on the EC2 machine:

```bash
sudo apt update
sudo apt install -y python3-pip
sudo apt install -y pipenv
sudo apt install -y virtualenv

mkdir mlflow
cd mlflow

pipenv install mlflow
pipenv install awscli
pipenv install boto3
pipenv shell
```

Then configure your AWS credentials:

```bash
aws configure
```

Finally, start the MLflow server:

```bash
nohup mlflow server -h 0.0.0.0 --workers 1 --default-artifact-root s3://<your-bucket-name> > mlflow.log 2>&1 &
```

Open the EC2 instance's Public IPv4 DNS on port 5000 to access the MLflow UI.

## Local Configuration

Set the tracking URI in your local terminal and in your code:

```bash
export MLFLOW_TRACKING_URI=<insert-here>
```

This URI should point to your EC2 MLflow server endpoint.
