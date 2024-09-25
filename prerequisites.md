# Prerequisites

# 1. kubectl (Kubernetes CLI)

A command line tool for working with Kubernetes clusters. For more information, see Installing or updating kubectl at https://kubernetes.io/docs/tasks/tools/

1. To install kubectl
```
curl.exe -LO "https://dl.k8s.io/release/v1.31.0/bin/windows/amd64/kubectl.exe"
````
2. To validate the binary (Optional)
```
curl.exe -LO "https://dl.k8s.io/v1.31.0/bin/windows/amd64/kubectl.exe.sha256"
```
3. To ensure the same version of kubectl is downloaded
```
kubectl version --client
```
# Reference image of kubectl CLI
![image (1)](https://github.com/user-attachments/assets/afdadd34-9e06-411a-8a24-6b4d664b3dc5)


# 2. eksctl (Amazon EKS CLI)

A command line tool to work with EKS clusters that automates many individual tasks. For more information refer to, https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html

# Installation with Chocolatey

1. Go to Chocolatey (package manager for Windows) website https://chocolatey.org/ and hover the mouse on Product and click on Chocolatey Open Source.

2. You'll be on this page (https://community.chocolatey.org/) and then click on "Install Chocolatey".

3. Follow the steps mentioned under "Install Chocolatey for Individual Use" using Windows PowerShell (from the below link), then use the command.

https://chocolatey.org/install?_gl=1*1lvm7er*_ga*MTE5MTIzNzMzOC4xNzI3MTU1NTUx*_ga_0WDD29GGN2*MTcyNzE1NTU1MS4xLjEuMTcyNzE1NjQ3NC4wLjAuMA..

4. Install eksctl

```
choco install eksctl -y
```  
5. Check if eksctl is installed
   
```
eksctl version
```

# Install eksctl using binary

1. Click on "Direct download (latest release) from this link https://eksctl.io/installation/. A binary gets downloaded, right click on the file and extract.
 
2. Delete the zip file and save the exec file.
         
3. Check if it's installed using command

```
eksctl version 
```

# Install eksctl using GitBash on Windows

Reference: https://github.com/eksctl-io/eksctl

1. Download the eksctl binary for windows
   
```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_windows_amd64.zip" (or)
```
```
curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_windows_amd64.zip
```
```
curl -sL "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_checksums.txt" | grep $PLATFORM | sha256sum --check
```
 
2. Unzip the file and move to bin folder located in home to execute the file

```
unzip eksctl_windows_amd64.zip -d $HOME/bin
```
3. Remove the zip file
```
rm eksctl_windows_amd64.zip
```
4. Check if eksctl is installed
```
eksctl version
```
# Reference image of eksctl CLI
![image (2)](https://github.com/user-attachments/assets/f5cb28d0-3243-4be3-804e-47b55b6811d1)

# 3. AWS CLI

A command line tool for working with AWS services, including Amazon EKS. For more information, see Installing, updating, and uninstalling the AWS CLI in the AWS Command Line Interface User Guide at 
```
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html#getting-started-install-instructions
```

# Install using binary 

Please use the below link to download the binary directly into your local machine and run the AWS CLI MSI installer for Windows (64-bit).

```
https://awscli.amazonaws.com/AWSCLIV2.msi
```

# To install using commands using GitBash on Windows

Reference: https://github.com/aws/aws-cli

1. Download the aws cli
```
curl -o "AWSCLIV2.msi" "https://awscli.amazonaws.com/AWSCLIV2.msi" -o "AWSCLIV2.msi"
```
( or )
```
curl -LO "https://awscli.amazonaws.com/AWSCLIV2.msi"
```
2. Check if aws cli is installed
```
aws --version
```

# To configure aws cli: 

1. Enter the command and hit enter

```
aws configure
```
2. Enter the Access key, Secret key and mention the region and the output format if required.

3. To check if aws cli is installed

```
aws --version
```
# Reference images of aws CLI
![image (5)](https://github.com/user-attachments/assets/792ff4ab-eeb0-4535-b39c-95b59362ffaa)
![image (3)](https://github.com/user-attachments/assets/92c17821-4e36-4ba2-8066-11d0cc1dc005)
