


###########################
DevOps EKS project
##########################


step:1 Create Role
step:2 Create VPC by using Cloudformation template
step:3 Create Cluster using AWSCLI
step:4 Update Cluster
step:5 Create nodes for the cluster using Cloudformation template
step:6 Map Nodes to Master
step:7 Check nodes of the cluster



EKS ROle ARN
--------------
ROLE ARN:arn:aws:iam::330931808725:role/AWA_EKS_ROLE



Installing kubectl :
----------------------------------
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.18.8/2020-09-18/bin/linux/amd64/kubectl
chmod +x ./kubectl
mkdir -p $HOME/bin
cp ./kubectl $HOME/bin/kubectl  
export PATH=$PATH:$HOME/bin
echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc
kubectl version --short --client


Installing aws-iam-authenticator
--------------------------------
curl -o aws-iam-authenticator https://amazon-eks.s3.us-west-2.amazonaws.com/1.18.8/2020-09-18/bin/linux/amd64/aws-iam-authenticator
chmod +x ./aws-iam-authenticator
mkdir -p $HOME/bin 
cp ./aws-iam-authenticator $HOME/bin/aws-iam-authenticator 
export PATH=$PATH:$HOME/bin
echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc
aws-iam-authenticator help


creating vpc  using cloudformation template:
--------------------------
https://amazon-eks.s3-us-west-2.amazonaws.com/cloudformation/2019-02-11/amazon-eks-vpc-sample.yaml



SecurityGroups, SubnetIds, VpcId 
---------------------------------

sg-01c3b3c5315005fb9
subnet-0ce8f9c06a72a859f,subnet-09e5438b946fb6e1b,subnet-06bfa2887541111e4
vpc-0789b5cef79556d9e

######################################
Creating cluster using AWSCLI:
---------------------------------
aws eks create-cluster \
  --name eks-cluster \
  --region ap-southeast-2 \
  --role-arn arn:aws:iam::330931808725:role/AWA_EKS_ROLE \
  --resources-vpc-config subnetIds=subnet-0ce8f9c06a72a859f,subnet-09e5438b946fb6e1b,subnet-06bfa2887541111e4,securityGroupIds=sg-01c3b3c5315005fb9


status check for eks cluster
----------------------------
aws eks --region ap-southeast-2 describe-cluster --name eks-cluster --query cluster.status



update eks cluster
------------------
aws eks --region ap-southeast-2 update-kubeconfig --name eks-cluster



cloudformation-worker-node template
---------------------------
https://amazon-eks.s3-us-west-2.amazonaws.com/cloudformation/2019-02-11/amazon-eks-nodegroup.yaml




chosing an ami id for my region
---------------------------------
https://docs.aws.amazon.com/eks/latest/userguide/eks-optimized-ami.html
Value
ami-0dadf836fc8220165 ( for ap-southeast-2 region )



NodeInstanceRole  arn:aws:iam::330931808725:role/eks-worker-node-NodeInstanceRole-ZXB02CVMIVO6

NodeSecurityGroup	sg-09e19f5afde39e672	


To enable worker nodes to join the cluster
-------------------------------------------
curl -o aws-auth-cm.yaml https://amazon-eks.s3-us-west-2.amazonaws.com/cloudformation/2019-02-11/aws-auth-cm.yaml



vi aws-auth-cm.yaml

kubectl apply -f aws-auth-cm.yaml

kubectl get nodes --watch

Deploy nginx image
------------------
kubectl run --image=nginx nginx-app --port=80

kubectl expose deployment nginx-app --port=80 --name=nginx-http --type LoadBalancer




delete cluster
---------------
aws eks --region ap-southeast-2 delete-cluster --name eks-cluster







########
part b
########

java,Apache Maven,GIT,Jenkins,Docker,Ansible,python
####################################################


Java:
-----
yum install java-1.8.0-openjdk  java-1.8.0-openjdk-devel –y

Install Git:
------------
yum install git -y

Install Maven:
--------------
Install Maven in /opt:
wget http://mirrors.estointernet.in/apache...

tar xvzf apache-maven-3.6.3-bin.tar.gz
 vi /etc/profile.d/maven.sh
 -------------------------------------------------
  export MAVEN_HOME=/opt/apache-maven-3.6.3
  export PATH=$PATH:$MAVEN_HOME/bin
 -------------------------------------------------
source /etc/profile.d/maven.sh
mvn -version

Install Jenkins:
----------------
sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stab...

sudo rpm --import https://jenkins-ci.org/redhat/jenkins...

sudo yum install jenkins -y
sudo service jenkins start

Install Docker:
----------------
yum install docker -y 
 service docker start
 usermod -aG docker jenkins
 
 Install Ansible
----------------
amazon-linux-extras install ansible2 -y

Install python 
----------------
yum install python3 -y

Install k8 module
------------------
pip3 install openshift




