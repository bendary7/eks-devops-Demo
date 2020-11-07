# spring-cloud-discovery

Pre-requisites:
--------
    - Install Git
    - Install Maven
    - Install Docker
    - EKS Cluster
    
Clone code from github:
-------
    git clone https://github.com/bendary7/eks-devops-project.git
    cd eks-devops-project/spring-cloud-discovery
    
Build Maven Artifact:
-------
    mvn clean install
 
Build Docker image for Springboot Application
--------------
    docker build -t vamsitechtuts/spring-cloud-discovery .
  
Docker login
-------------
    docker login
    
Push docker image to dockerhub
-----------
    docker push a84136757/spring-cloud-discovery
    
Deploy Spring Application:
--------
    kubectl apply -f deployment.yaml
    
Check Deployments, Pods and Services:
-------

    kubectl get deploy
    kubectl get pods
    kubectl get svc
    

Now we can cleanup by using below commands:
--------
    kubectl delete deploy spring-cloud-discovery
    kubectl delete svc spring-cloud-discovery
