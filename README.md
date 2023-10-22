## Update
Added setup for AWS deployment

## End to End MAchine Learning Project

1. Docker Build checked
2. Github Workflow
3. Iam User In AWS

## Docker Setup In EC2 commands to be Executed

#optinal

sudo apt-get update -y

sudo apt-get upgrade

#required

curl -fsSL https://get.docker.com -o get-docker.sh

sudo sh get-docker.sh

sudo usermod -aG docker ubuntu

newgrp docker

## Configure EC2 as self-hosted runner:

## Setup github secrets:

AWS_ACCESS_KEY_ID=

AWS_SECRET_ACCESS_KEY=

AWS_REGION = us-east-1

AWS_ECR_LOGIN_URI = demo>>  566373416292.dkr.ecr.ap-south-1.amazonaws.com

ECR_REPOSITORY_NAME = simple-app

## AWS permissions
Create a user with the following permissions:
- AmazonEC2ContainerRegistryFullAccess
- AmazonEC2FullAccess

Create an access key with the CLI.  Make sure to download the access key and keep it handy for the CI/CD setup in Github.

## AWS ECR
Within AWS ECR create a new repository.  Set the visibility to private.  Give it the ECR repo name you want.  Leave everything else as default and create the repository.

## Create EC2 instance
The next step is to create an EC2 instance to act as the virtual cloud server.
Pick any name you want.  Select Ubuntu x64 as the OS image.  It was suggested to use t2.medium compute but note this would incur charges.  Pick a key pair for login. 

Create a security group and check:
- Allow SSH traffic from Anywhere 0.0.0.0/0
- Allow HTTPS traffic from the internet
- Allow HTTP traffic from the internet

Can leave the storage as default.

After everything is configured launch the instance.

## Connect to the instance and install more packages

Run the following commands:
sudo apt-get update -y
sudo apt-get upgrade
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker

## Run the runner
Within GitHub, go to the settings of the repo and under actions, click Runners.  Click new runner and select Linux.  Then run the specified commands.

For the name of the runner enter 'self-hosted' (without the quotes).
Can enter a blank (simply press enter) for the additional labels.
Also no need to enter a name for the work folder.

Then to actually run the runner: ./run.sh

You can see the runner in GitHub by going to runners.  You should see a runner named 'self-hosted'.  At first it should be in the idle state.

## Adding secret keys in GitHub

AWS_ACCESS_KEY_ID=

AWS_SECRET_ACCESS_KEY=

AWS_REGION = us-east-1

AWS_ECR_LOGIN_URI = take this from image you added in ecr

ECR_REPOSITORY_NAME = simple-app

## Security Group
Need to make an inbound rule of type 'Custom TCP' for port 8080 allowing all traffic in (0.0.0.0/0)