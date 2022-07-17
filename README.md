# NodeJs project documentation
# Executive Summary
- Project Description and Goals
- Project Architecture
- Technical Specifications

  -- Software tools used
  
  -- Software Description
- Project Workflow

# Project Description and Goals
In this project we have used NodeJs web application to automate the infrastructure set up on AWS Cloud using Terraform i.e. we created a VPC and EKS cluster. Deployed the application in different environments like DEV, QA and PROD based on the requirements using Helm and Jenkins pipeline and then pushed the image to ECR ,we monitored the application using the monitoring tools i.e Prometheus and Grafana ,further we checked the logs of the executed application using logging tools i.e  ElasticSearch, Fluent-bit and Kibana.
To access the project sources log on to https://github.com/namrata-aatmani.

## Project Architecture
![alt test](https://github.com/namrata4447/Aatmani-Project-Doc/blob/main/Project%20Overview.drawio.png)
  
# Technical Specifications
###  Software Tools used:
- Amazon Web Services(AWS)
- Terraform
- Docker
- Jenkins
- Kubernetes
- Helm
- Prometheus
- Grafana
- ElasticSearch
- Kibana
- Fluent-bit

### Software Description
#### Amazon Web Services(AWS)
AWS is a cloud service from Amazon, which provides services in the form of building blocks, these building blocks can be used to create and deploy any type of application in the cloud.

#### Git
Open source version control tools for developers to manage source code.

#### Terraform
Terraform is the infrastructure as code tool from HashiCorp. It is a tool for building, changing, and managing infrastructure in a safe, repeatable way.

#### Docker
Docker is an open source platform that enables developers to build, deploy, run, update and manage containerized applications.

#### Jenkins
Jenkins is an automation tool written in Java with built-in plugins for continuous integration tasks. It is used to continuously build and test projects making it easier to integrate the changing codes to it.

#### Kubernetes
Kubernetes (also known as k8s or “kube”) is an open source container orchestration platform that automates many of the manual processes involved in deploying, managing, and scaling containerized applications.

#### Helm
Helm is a Kubernetes deployment tool for automating creation, packaging, configuration, and deployment of applications and services to Kubernetes clusters.
Helm uses a packaging format called charts. A chart is a collection of files that describe a related set of Kubernetes resources.

#### Prometheus
Prometheus is a free and open-source monitoring and alerting tool which records real-time events in a time-series database. It also provides flexible queries and real-time alerting which helps in quick diagnosis and troubleshooting of errors.

#### Grafana
Grafana is an open-source platform for metric analytics, monitoring, and visualization.

####  ElasticSearch
ElasticSearch is a database with search engine where all logs are stored.

####  Kibana
Kibana is an open source data visualization dashboard for Elasticsearch. It provides visualization capabilities on top of the content indexed on an Elasticsearch cluster. Users can create bar, line and scatter plots, or pie charts and maps on top of large volumes of data.

#### Fluent-bit
Fluent Bit Enables You To Collect Logs And Metrics From Multiple Sources, Enrich Them With Filters, And Distribute them to any defined destination.

## Project Workflow
#### Workflow 1
##### Create a VPC and EKS ( Amazon Elastic Kubernetes Service ) Cluster using Terraform
Firstly, we created a EC2 instance and installed Terraform,using Terraform code we created a VPC , Subnet , IG (internet gateway) , NAT (Network Address Translation), Security group, EC2 instance(Jumpbox),EKS (Elastic Kubernetes Service ).
An Amazon EKS cluster operates in a Virtual Private Cloud (VPC), a secure private network within an Amazon data center. EKS deploys all resources to an existing subnet in a VPC you select, in one Amazon Region.
##### General Information
###### Amazon Elastic Kubernetes Service (Amazon EKS)
A Amazon Elastic Kubernetes Service (Amazon EKS) is a managed AWS Kubernetes service that scales, manages, and deploys containerized applications. It typically runs in the Amazon public cloud, but can also be deployed on premises. The Kubernetes management infrastructure of Amazon EKS runs across multiple Availability Zones (AZ).
EKS Clusters are made up of a control plane and EKS nodes.The EKS control plane is responsible for controlling Kubernetes master nodes,EKS nodes.
- Kubernetes master nodes are distributed across several AWS availability zones (AZ), and traffic is managed by Elastic Load Balancer (ELB).
- Kubernetes worker nodes run on EC2 instances in your organization’s AWS account.

> Installation link: Terraform
https://computingforgeeks.com/how-to-install-terraform-on-ubuntu/
> link used: EKS
https://ap-southeast-1.console.aws.amazon.com/eks/home?region=ap-southeast-1#/clusters

#### Workflow 2
##### Git reposistories: Productionteam and Development
Created organization by name namrata-aatmani on Github having two different repositories namely productionteam and development ,added and invited team members to access the same github organization.
- In productionteam repo we pulled the AatmaaniProject i.e 
  from https://github.com/VV-MANOJ/AatmaaniProject
- Created and Pushed the Dockerfile  written according to the requirement in README.md from the AatmaaniProject.Also,added GIT_COMMIT to get the latest image(to get commit ID of   the untagged image) from the ECR (point in ref with workflow 5 ).
- In development repo we pushed the Helm chart resources created for nodejs having values depending on evironments dev-values.yml,qa-values.yml,prod-values.yml (point in ref with workflow 4 )and pushed the jenkins nams-dev-pipeline,nams-qa-pipeline and nams-prod-pipeline scripts (point in ref with workflow 6 ).

#### Workflow 3
##### Git merge and branch protection rules
In github organization namely namrata-aatmani's productionteam repo, if a developer updates any code change in the main branch,it should not directly merge to the main branch, instead it should raise a Pull Request (PR) using the branch protection rules and one of the team member should review,confirm and approve the changes.Thereafter the code is merged in the main branch after approval.
> link used:
https://spectralops.io/blog/how-to-set-up-git-branch-protection-rules/

#### Workflow 4
##### Intallation of AWS Configure,Docker, Kubernetes and Helm
- Here, we installed AWS Configure,Docker, Kubernetes and Helm in Jumpbox.
> Installation link :AWS configure
https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html
> Installation link: Docker
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04
> Installation link: Kubernetes
https://www.codegrepper.com/code-examples/shell/install+kubectl+ubuntu+20.04
> Installation link: Helm
https://phoenixnap.com/kb/install-helm

- Created 3 namespaces in EKS Cluster namely dev,qa and prod in the jumpbox where helm is installed , dev environment is for developer Team , qa environment is for testing team and prod environment is for production team.
- After creating the helm chart rename the values.yml file to dev-values.yml,qa-values.yml,prod-values.yml add ECR repo url 
(point in ref with Workflow 5),replicaCount:1 (dev),replicaCount:1 (qa),replicaCount:2 (prod), Service type: LoadBalancer, port number:3000 and push the code to github repo. Three loadbalancers will be created w.r.t dev,qa and prod in aws console.A service of type LoadBalancer is the simplest and the fastest way to expose a service inside a Kubernetes cluster to the external world. 

#### Workflow 5
#####  Create a Amazon Elastic Container Registry(ECR) repository
- Create a Amazon Elastic Container Registry(ECR) repo and upload latest image.
> link used : ECR
https://ap-southeast-1.console.aws.amazon.com/ecr/repositories?region=ap-southeast-1

#### Workflow 6
##### Installation of Jenkins,Slack integration and pipeline scripts for dev environment, qa environment and prod environment
- Install Java,Jenkins in the same jumpbox and Create a jenkins job and install the plugins related to AWS,ECR and slack  notification.
> Installation link: Jenkins
https://linuxhint.com/install-jenkins-on-ubuntu/
> link used: Slack intergration on Jenkins
https://medium.com/appgambit/integrating-jenkins-with-slack-notifications-4f14d1ce9c7a

- Create credentials for slack(secret text),GIT and AWS(AWS credentials)
- Create 3 jenkins job and push these nams-dev-pipeline,nams-qa-pipeline and nams-prod-pipeline  scripts to namrata-aatmani's development repo.

1. Build and Deploy to the Dev  
     - Create a webhook in namrata-aatmani's development repo by including the jenkins url and in the jenkins job click on the ‘Build Triggers’ tab 
       and then on the ‘GitHub hook trigger for GITScm polling’ so that the  job triggers automatically when there is merge in to main branch.
     - In the jenkins pipeline script Git clone
       the repo https://github.com/namrata-aatmani/productionteam.git, the script should pull the nodejs repo.
     - login into ECR and run the docker build . , so that the new dev-latest image is uploaded into ECR,use  GIT_COMMIT in the script  to get the latest image
       (gives  commit ID of the untagged image) from the ECR.
     - Tag the latest image(uploaded earlier while creating ECR) to dev-latest (point in ref with workflow 5).
     - Push the dev-latest to the ECR, now the output in ECR has a commit ID of the untagged image and new image as dev-latest.
     - Integrate slack with jenkins by creating new channel (#jenkins-alert) to set up an alert on slack when the job is successful/failed.
     - If any stage fails in between it shouldn't proceed to the next phase and send alert on slack.
     
2. Deploy to the QA 
     - In the jenkins pipeline script Git clone 
       the repo https://github.com/namrata-aatmani/productionteam.git, the script should pull the nodejs repo.
     - login into ECR.
     - Tag the dev-latest image  to qa-latest.
     - Push the qa-latest to the ECR, now the output in ECR has a new image as qa-latest along with commit ID of the untagged image and dev-latest .
     - Set up an alert on slack when the job is successful/failed. 
     - If any stage fails in between it shouldn't proceed to the next phase and send alert on slack.
 
3. Deploy to the Prod 
     - In the jenkins pipeline script Git clone 
       the repo https://github.com/namrata-aatmani/productionteam.git, the script should pull the nodejs repo.
     - login into ECR.
     - Tag the qa-latest image  to prod-latest.
     - Push the prod-latest to the ECR, now the output in ECR has a new image as prod-latest along with commit ID of the untagged image,dev-latest and prod-latest.
     - Set up an alert on slack when the job is successful/failed.
     - If any stage fails in between it shouldn't proceed to the next phase and send alert on slack.
   ![alt text](https://github.com/namrata4447/Aatmani-Project-Doc/blob/main/Jenkins%20ECR.drawio.png)

#### Workflow 7
#### Deployment of Metric server,Cluster Autoscalar (CA) and Horizontal Pod Autoscalar (HPA)
##### Metrics Server set up
You will be able to see cpu and memory utilization metrics with metrics-server.The Metrics server role frequently checks the metrics of every running pods in EKS cluster. The main role of this server is it will help to Horizontal Pod Autoscaler and Vertical Pod Autoscaler.
Metrics Server is a scalable, efficient source of container resource metrics for Kubernetes built-in autoscaling pipelines.
Metrics Server collects resource metrics from Kubelets and exposes them in Kubernetes apiserver through Metrics API for use by Horizontal Pod Autoscaler and Vertical Pod Autoscaler. Metrics API can also be accessed by kubectl top, making it easier to debug autoscaling pipelines.
> Installation link : Metrics Server
- https://docs.aws.amazon.com/eks/latest/userguide/metrics-server.html

##### Cluster Autoscalar (CA) set up
Cluster Autoscalar is a tool that automatically adjusts the size of the Kubernetes cluster based on the utilization of Pods and Nodes in your cluster.
The Cluster Autoscalar doesn’t directly measure CPU and memory usage values to make a scaling decision. Instead, it checks every 10 seconds to detect any pods in a pending state, suggesting that the scheduler could not assign them to a node due to insufficient cluster capacity.
In the scaling-up scenario, CA is automatically started when the number of pending (un-schedulable) pods increases due to resource shortages and works to add additional nodes to the cluster.
It sets the number of nodes in the cluster when nodes are underutilized or when pods fail to schedule.
> link used :
- https://www.kubecost.com/kubernetes-autoscaling/kubernetes-cluster-autoscaler/

#### Workflow 8
##### Horizontal Pod Autoscaler (HPA) set up
HPA is a form of autoscaling that increases or decreases the number of pods in a replication controller, deployment, replica set, or stateful set based on CPU utilization—the scaling is horizontal because it affects the number of instances rather than the resources allocated to a single container.
HPA can make scaling decisions based on custom or externally provided metrics and works automatically after initial configuration. All you need to do is define the MIN and MAX number of replicas.
Once configured, the Horizontal Pod Autoscaler controller is in charge of checking the metrics and then scaling your replicas up or down accordingly. By default, HPA checks metrics every 15 seconds.
- In this project we have enabled the HPA autoscaling to true in prod-values.yml for prod environment.
> link used:
- https://www.kubecost.com/kubernetes-autoscaling/kubernetes-hpa/

#### Workflow 9
### Monitoring Tools installation and setup
##### Prometheus Architecture Components
![alt test](https://github.com/namrata4447/Aatmani-Project-Doc/blob/main/Prometheus%20Grafana.drawio.png)

Prometheus is an open source tool for monitoring and alerting applications.
- Prometheus Server - The Prometheus server handles the scraping and storing of metrics collected from multiple nodes.
- Node Exporter - Helps us in measuring various server resources such as RAM, disk space, and CPU utilization of all target machines.
- Alert Manager - It handles alerts sent based upon the metrics data collected in Prometheus and send event notifications to your configured notification
  channels  like Slack  when your Kubernetes cluster meets pre-configured events and metrics rules in the Prometheus server.
- Web UI - Provides end user with an interface to visualize data collected by prometheus.For this we will be using  Grafana to visualize the data.
> Installation link : Prometheus
https://artifacthub.io/packages/helm/prometheus-community/prometheus

#### Prometheus set-up
- Install prometheus using 
  helm repo:   https://artifacthub.io/packages/helm/prometheus-community/prometheus
- Expose the prometheus server with type NodePort and set target port to 9090,now you can access the prometheus server externally with the URL that is determined
- In the alert-rules.yml file we declared alert for slack notification and alert for rules in alert manager like 
  InstanceDown,HostOutOfMemory,HostHighCpuLoad,KubePodCrashLooping,KubePodNotReady etc. and sends event notifications to your configured notification channel 
  including like Slack  when your Kubernetes cluster meets pre-configured events and metrics rules in the Prometheus server.
- Now we will configure the service and start it .
- You can view the metrics in the browser by putting Public-IP with the port number 9090
  
#### Grafana
Grafana is an open-source platform for metric analytics, monitoring, and visualization .After installing Prometheus successfully, we can install the Grafana and configure Prometheus as a datasource.
> Installation link : Grafana
https://artifacthub.io/packages/helm/grafana/grafana

#### Grafana set-up
- Install Grafana using 
  helm repo: https://artifacthub.io/packages/helm/grafana/grafana
- After installing Grafana ,put the public-ip with port number 3000 and login to grafana using the credentials.
- Click on Datasources, choose Prometheus and set the IP address in grafana.
- A Dashboard is created and we imported necessary dashboard in grafana to visualize the metrics.

#### Workflow 10
### Logging Tools installation and setup
![alt test](https://github.com/namrata4447/Aatmani-Project-Doc/blob/main/EFK%20Overview.drawio.png)

#### EFK [ Elasticsearch, Fluent-bit and Kibana ]
- EFK - is a suite of tools combining Elasticsearch, Fluent-bit and Kibana to manage logs. 
- EFK stack - is a centralized logging which identifies problems with servers or applications. It searches for the logs in a single place, helps to find 
  issues in multiple servers by connecting logs for a specific time series.
- ElasticSearch - Acts as a database where the data is collected and logs will be stored.
- Fluent-Bit - is an open source, light-weight, and multi-platform service created for data collection mainly logs and streams of data.
- Kibana - is a visualization tool, which accesses the logs from Elasticsearch and is able to display to the user in the form of line graph, bar graph,
  pie charts etc. 
  
#### Installation and set-up of Elasticseach:
- Create new  EC2 Instance[Jumpbox] for Installation of Elasticsearch and Kibana.
- Install and Configure Elasticsearch using:
  https://phoenixnap.com/kb/how-to-install-elk-stack-on-ubuntu
- Enable Basic Authentication on EFK Stack
  https://kifarunix.com/how-to-enable-basic-authentication-on-elk-stack/
- After installing we need to make necessary changes w.r.t network.host: mention Private IP of the instance where elastic and kibana are installed,
  port address: 9200 , discovery.type: single-node , enable Elasticsearch security features to protect your data in elasticsearch.yml file.
- Start and Enable the Elasticsearch service then check the status if Elasticsearch is active.
- You can view the elasticsearch in the browser by putting Public-IP with the port number 9200 and login using the elasticsearch credentials setup.

#### Installation and set-up of Kibana:
- Install Kibana in the same instance[Jumpbox] where Elasticsearch is intalled.
- Install and Configure Kibana using the same link used for Elasticsearch:
  https://phoenixnap.com/kb/how-to-install-elk-stack-on-ubuntu
- Enable Basic Authentication on EFK Stack
  https://kifarunix.com/how-to-enable-basic-authentication-on-elk-stack/
- After installing we need to make necessary changes w.r.t server.port:5601,server.host: mention Private IP of the instance where elastic and kibana 
  are installed,elasticsearch.hosts:mention Public IP of instance where elastic and kibana are installed) ,basic authentication: set up elasticsearch username 
  and password that the Kibana server uses to perform maintenance on the Kibana in kibana.yml file.
- Start and Enable the Kibana service then check the status if Kibana is active.
- You can view the Kibana in the browser by putting Public-IP with the port number 5601 after logging in using the elasticsearch credentials setup.
- Kibana is a web application where we can see all the logs of pods.

#### Set-up of Fluent-Bit:
- Deploy Fluent-Bit on EC2 instance[Jumpbox] connected to EKS cluster,as it consists logs of all the pods in EKS cluster , which sends the logs collected 
  to ElasticSearch .
- Install and Configure Fluent-Bit :
  https://docs.fluentbit.io/manual/v/1.3/installation/kubernetes
- Fluent Bit must be deployed as a DaemonSet, so on that way it will be available on every node of your Kubernetes cluster.
- To get started  create the namespace, service account and role setup.
- Create a ConfigMap that will be used by our Fluent Bit DaemonSet.
- After installing we need to make necessary changes in FLUENT_ELASTICSEARCH_HOST : mention Public IP of the instance where elastic and kibana are installed in 
  fluent-bit.yml file.
- Now Fluent Bit DaemonSet is ready to be used with Elasticsearch on a normal  EKS Cluster.


