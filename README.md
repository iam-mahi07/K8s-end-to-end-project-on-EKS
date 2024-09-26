# Kubernetes end-to-end project on Amazon EKS

Hey! Welcome👋

This repo would help you to gain a practical hands-on experience to deploy a web application on AWS EKS using Kubernetes. I promise!

# Project Title & Description

Title: Deploying a 2048 Game Application on Amazon EKS with ALB using Kubernetes.

Brief Description: This project demonstrates the deployment of a containerized web application (2048 game) on AWS using Kubernetes and EKS.

# Tools & Technologies Used

1. AWS EKS (Elastic Kubernetes Service)

2. Kubernetes (for managing containers and pods)

3. AWS Load Balancer Controller (for managing Application Load Balancers)

4. Fargate (serverless compute engine)

5. IAM Roles & Policies (for secure access and management)

6. kubectl, eksctl, AWS CLI (for command-line interaction)

# Key Achievements

1. Deployed a scalable web application using Kubernetes on AWS EKS, ensuring high availability and fault tolerance.

2. Configured ALB (Application Load Balancer) to route traffic to the Kubernetes service, optimizing load distribution across pods.

3. Implemented IAM roles and policies to secure the AWS resources, ensuring least-privileged access for Kubernetes workloads.

4. Leveraged AWS Fargate to run pods without managing underlying EC2 instances, reducing infrastructure management overhead.

# Let's jump into the pratical now!!

Follow these steps after the prerequisites are setup.

# Step-1: Installation of EKS

To find the below steps, please refer to https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html

You can install or setup the EKS either using Fargate or EC2 instances as well. 

Install EKS using Fargate
```
eksctl create cluster --name "cluster-name" --region "region-name" --fargate
```
Delete the cluster
```
eksctl delete cluster --name "cluster-name" --region "region-name"
```
# Reference image of EKS cluster creation

![image (6)](https://github.com/user-attachments/assets/c0089dd7-164a-4521-a2f9-b66384bde71b)

# Reference image of EKS cluster deletion

![image (16)](https://github.com/user-attachments/assets/cf29ff43-9e0d-497d-9eb5-e26203e027fb)

# Step-2: Configuration of IAM OIDC provider

To find the below steps. please refer to https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html

For the ALB controller to talk to AWS resources, it needs to have the IAM integrated. So, we use IAM OIDC provider.

i) To create an IAM OIDC identity provider for your cluster with eksctl:
```
oidc_id=$(aws eks describe-cluster --name $cluster_name --query "cluster.identity.oidc.issuer" --output text | cut -d '/' -f 5) 
```
ii) To determine whether an IAM OIDC provider with your cluster's issuer ID is already in your account ( means check if there's an IAM OIDC provider configured already).
```
aws iam list-open-id-connect-providers | grep $oidc_id | cut -d "/" -f4
```
If nothing is found, then create one;
```
eksctl utils associate-iam-oidc-provider --cluster $cluster_name --approve
```
# Reference images of IAM OIDC creation

![image (7)](https://github.com/user-attachments/assets/23a10206-586b-44e9-b8c1-1315d0768f7a)

![image (8)](https://github.com/user-attachments/assets/91ec6ea5-2fd0-45bb-9e36-6bbaa92dd8ee)

# Step-3: Creation of IAM policy and IAM Role

First, create an IAM policy with all necessary permissions (e.g., to manage load balancers) and then create an IAM role, attach the policy to it, and associate this IAM role with the IAM service account.

Here we’re creating an IAM service account for the AWS Load Balancer (ALB) Controller on your EKS cluster. This allows the controller uses the permissions in the IAM policy to interact with AWS resources (like creating and managing the Application Load Balancer) for your 2048 game web app.

NOTE: You only need to create an IAM Role for the AWS Load Balancer Controller one per AWS account. Check if AmazonEKSLoadBalancerControllerRole exists in the IAM Console. If the role exists, skip to Step-4 (Install AWS Load Balancer Controller).


To find the below steps, please refer to https://docs.aws.amazon.com/eks/latest/userguide/lbc-helm.html

i) Download IAM policy for the AWS Load Balancer Controller that allows it to make calls to AWS APIs on your behalf.
```
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/install/iam_policy.json
```
ii) Create IAM policy using the policy downloaded in the previous step:
```
aws iam create-policy --policy-name AWSLoadBalancerControllerIAMPolicy --policy-document file://iam_policy.json
```
iii) To create IAM Role:
```
eksctl create iamserviceaccount --cluster=<your-cluster-name> --namespace=kube-system --name=aws-load-balancer-controller --role-name AmazonEKSLoadBalancerControllerRole --attach-policy-arn=arn:aws:iam::<your-aws-account-id>:policy/AWSLoadBalancerControllerIAMPolicy --approve
```
# Reference images of IAM policy and IAM Role

![image (9)](https://github.com/user-attachments/assets/8633d66e-aa22-4516-a547-2fe1680a73f6)

![image (11)](https://github.com/user-attachments/assets/8c8fbc17-b16d-48d3-9d7e-5b95ba897f2b)

![image (10)](https://github.com/user-attachments/assets/8b0e157c-2fcf-4eab-88d0-1006133c39cd)


# Step:4 Installation of AWS Load Balancer Controller

We create an ALB Ingress controller, which reads the Ingress resource and it’ll create an Application Load Balancer to access the application with an address and just creating ALB is of no use. So, it’ll also configure entire load balancer with target groups, ports etc and everything is taken care by the Ingress Controller only.

Before executing the helm commands to install ALB controller, first install helm on windows to perform the helm commands. 

Reference for Helm download: https://helm.sh/docs/intro/install/#from-the-binary-releases

# Installation of helm 

i) Commands (you can choose any one of the commands from below)

a) Using -o in the command will help to get a desired name to the file after downloading onto the terminal/server

```
curl -o my-helm.zip https://get.helm.sh/helm-v3.16.1-windows-amd64.zip```
```

b) Using -O in the command helps to download the binary file with the original name

```
curl -O https://get.helm.sh/helm-v3.16.1-windows-amd64.zip
```

c) Using -sLO in the command helps, -s makes curl silent, -L follows redirects, and -O saves the file with its original name.

```
curl -sLO https://get.helm.sh/helm-v3.16.1-windows-amd64.zip
```

ii) Unzip the binary and move to bin directory to make it executable

```
unzip helm-v3.16.1-windows-amd64.zip -d $HOME/bin
```

# Download using binary to local machine

i) You can directly download the helm binary.
```
https://get.helm.sh/helm-v3.16.1-windows-amd64.zip
```
ii) Once it's downloaded in your local machine, unzip the binary and extract the file.

iii) Move the helm file to the bin folder for the binary file can be executed. The binary moved to bin acts as a "helm cli" and the helm commands can be be performed. 

```
mv helm.exe $HOME/bin
```
iv) Type helm and hit enter, if you get the commands related to helm, then helm is installed and you can proceed with the below mentioned steps.


To find the below steps, please refer to https://docs.aws.amazon.com/eks/latest/userguide/lbc-helm.html

i) To add the eks-charts Helm chart repo: 
```
helm repo add eks https://aws.github.io/eks-charts
```
If using Windows PowerShell, first install chocolatey package manager and then install Helm using the command 
```
choco install kubernetes-helm
```
ii) To update the helm repo (to make sure that you have the most recent charts): 
```
helm repo update eks
```
# To install AWS ALB Controller

i) Create ALB using the helm
```
helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=<your-cluster-name> --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller --set region=<region> --set vpcId=<your-vpc-id>
```
ii) To verify that the controller is installed and running
```
kubectl get deployment -n kube-system aws-load-balancer-controller
```

# Reference image of ALB controller creation using helm

![image (17)](https://github.com/user-attachments/assets/d2854290-44bb-48dc-baaf-b362b250321f)


# Step-4: To deploy the application (2048 game)

In this step, we create a Fargate profile inside a cluster within a desired region to deploy the application in a namespace. In the next step, we create namespace, deployment, service, ingress, alb controller, load balancer through which we access the web application (2048 game). 

To find the below steps, please refer to https://docs.aws.amazon.com/eks/latest/userguide/alb-ingress.html

i) To deploy using Fargate, create a Fargate profile. You can create the profile by running the following command.
```
eksctl create fargateprofile --cluster <cluster-name> --region <region-code> --name alb-sample-app --namespace game-2048
```
ii) Now deploy the deployment, service, ingress run the following command.
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.7.2/docs/examples/2048/2048_full.yaml
```
# Reference images of final O/P such as Fargate, Ingress, Load balancer and 2048 game

![image (13)](https://github.com/user-attachments/assets/9d54fa80-5d5e-4509-8779-17549f0ebbce)

![image (18)](https://github.com/user-attachments/assets/29e5f280-fa09-4ca0-88a2-faa62a59b7fa)

![image (19)](https://github.com/user-attachments/assets/7c6636e6-7d08-4a0d-ab60-56d00cef56c6)

![image (15)](https://github.com/user-attachments/assets/c248e9b8-1b80-40ec-a726-4968b3a0c765)

![image (14)](https://github.com/user-attachments/assets/206ee272-a0f7-49a9-9c5c-13d6b66be1d0)

![image (12)](https://github.com/user-attachments/assets/e32a4d5c-1ff2-4041-a59f-609109346e23)

# Congratulations you've deployed the application!! :clap: :heart_on_fire:
# Thanks for visiting and trying :wave:
