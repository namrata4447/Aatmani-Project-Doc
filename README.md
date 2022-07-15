# NodeJs project documentation
# Executive Summary
- Project Description and Goals
- Project Architecture
- Technical Specifications

  -- Software tools used
  
  -- Software Description
- Project Workflow

# Project Description and Goals
In this project we have used NodeJs web application to automate the infrastructure set up on AWS Cloud using Terraform i.e we created a VPC and EKS cluster and deployed the application in three different environments which includes DEV,QA,PROD using Helm .We used jenkins pipeline to build jobs for DEV,QA,PROD and then push the image to ECR ,we need to monitor the application using the monitoring tools i.e Prometheus and Grafana , further checked the logs of the executed application using logging tools i.e  ElasticSearch, Fluent-bit and Kibana.
To access the project sources log on to https://github.com/namrata-aatmani.

## Project Architecture
![alt test](https://github.com/namrata4447/Aatmani-Project-Doc/blob/main/Project%20Overview.drawio.png)
  
# Technical Specifications
###  Software Tools used:
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
#### Terraform
Terraform an infrastructure provisioning tool where you can store your cloud infrastructure setup as codes. IaC allows you to build, change, and manage your infrastructure in a safe, consistent, and repeatable way by defining resource configurations that you can version, reuse, and share.

#### Docker
Docker is an open source platform that enables developers to build, deploy, run, update and manage containerized applications.

#### Jenkins
Jenkins is an automation tool written in Java with built-in plugins for continuous integration tasks. It is used to continuously build and test projects making it easier to integrate the changing codes to it.

#### Kubernetes
Kubernetes, also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications in desired states.

#### Helm
Helm is a package manager for Kubernetes that allows developers and operators to more easily package, configure, and deploy applications and services onto Kubernetes clusters.
Helm packages are called charts, and they consist of a few YAML configuration files and some templates that are rendered into Kubernetes manifest files.

#### Prometheus
Prometheus is a free and open-source monitoring and alerting tool which records real-time events in a time-series database. It also provides flexible queries and real-time alerting which helps in quick diagnosis and troubleshooting of errors.

#### Grafana
The open-source platform for monitoring and observability,it allows you to query, visualize, alert on and understand your metrics no matter where they are stored. Create, explore, and share dashboards with your team and foster a data-driven culture.

####  ElasticSearch
Elasticsearch is a distributed, RESTful search and analytics engine capable of addressing a growing number of use cases. As the heart of the Elastic Stack, it centrally stores your data for lightning fast search, fine‑tuned relevancy, and powerful analytics that scale with ease.

####  Kibana
Kibana is an open source data visualization dashboard for Elasticsearch. It provides visualization capabilities on top of the content indexed on an Elasticsearch cluster. Users can create bar, line and scatter plots, or pie charts and maps on top of large volumes of data.

#### Fluent-bit
Fluent Bit Enables You To Collect Logs And Metrics From Multiple Sources, Enrich Them With Filters, And Distribute them to any defined destination.

## Project Workflow
#### Workflow 1
Firstly we created a EC2 instance and installed Terraform,using Terraform code we created a VPC , Subnet , IG (internet gateway) , NAT (Network Address Translation), Security group, EC2 instance(Jumpbox),EKS (Elastic Kubernetes Service ) i.e a Master Cluster and Worker Nodes respectively.
> Installation link: Terraform
https://computingforgeeks.com/how-to-install-terraform-on-ubuntu/
> link used: EKS
https://ap-southeast-1.console.aws.amazon.com/eks/home?region=ap-southeast-1#/clusters

#### Workflow 2
Created organization by name namrata-aatmani on Github having two different repositories namely productionteam and development ,added and invited team members to access the same github organization.
- In productionteam repo we pulled the AatmaaniProject i.e 
  from https://github.com/VV-MANOJ/AatmaaniProject
- Created and pushed the Dockerfile with respect to details mentioned in README.md    of AatmaaniProject.also,add GIT_COMMIT to get the latest image(to get commit ID of   the untagged image) from the ECR (point in ref with workflow 5 ).
- In development repo we pushed the Helm chart resources created for nodejs with    dev-values.yml,qa-values.yml,prod-values.yml (point in ref with workflow 4 )and pushed the jenkins nams-dev-pipeline,nams-qa-pipeline and nams-prod-pipeline scripts (point in ref with workflow 6 ).

#### Workflow 3
In github organization namely namrata-aatmani's productionteam repo, if a developer updates any code change in the main branch,it should not directly merge to the main branch, instead it should raise a Pull Request (PR) using the branch protection rules and one of the team member should review,confirm and approve the changes.Thereafter the code is merged in the main branch once approved.
> link used:
https://spectralops.io/blog/how-to-set-up-git-branch-protection-rules/

#### Workflow 4
- In this phase we installed AWS Configure,Docker, Kubernetes and Helm in Jumpbox.
> Installation link: Docker
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04
> Installation link: Kubernetes
https://www.codegrepper.com/code-examples/shell/install+kubectl+ubuntu+20.04
- Created 3 namespaces in EkS Cluster ie. dev,qa and prod in the jumpbox where helm is installed i.e dev environment is for developer Team , qa environment is for testing team. And prod environment is for production team.
- After creating the helm chart rename the values.yml file to dev-values.yml,qa-values.yml,prod-values.yml add ECR repo url 
(point in ref with Workflow 5),replicaCount:1 (dev),replicaCount:1 (qa),replicaCount:2 (prod), Service type: LoadBalancer, port number:3000 and push the code to github repo. Three loadbalancers will get created w.r.t dev,qa and prod in aws console.A service of type LoadBalancer is the simplest and the fastest way to expose a service inside a Kubernetes cluster to the external world. 

#### Workflow 5
- Create a Amazon Elastic Container Registry(ECR) repo and upload latest image.
> link used : ECR
https://ap-southeast-1.console.aws.amazon.com/ecr/repositories?region=ap-southeast-1

#### Workflow 6
- Install Java,Jenkins in the same jumpbox and Create a jenkins job and install the plugins related to AWS,ECR and slack  notification.
> Installation link: Jenkins
https://linuxhint.com/install-jenkins-on-ubuntu/
> link used: Slack intergration on Jenkins
https://medium.com/appgambit/integrating-jenkins-with-slack-notifications-4f14d1ce9c7a

- Create credentials for slack(secret text),GIT and AWS(AWS credentials)
- Create 3 jenkins job and push these nams-dev-pipeline,nams-qa-pipeline and nams-prod-pipeline  scripts to namrata-aatmani's development repo.

1. Build and Deploy to the Dev  
     - Create a webhook in namrata-aatmani's development repo by including the jenkins url and in the jenkins job click on the ‘Build Triggers’ tab 
       and then on the ‘GitHub hook trigger for GITScm polling’ so that the  job triggers automaticaly when there is merge in to main branch.
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
### Metrics Server
Metrics server deployment is required if you are planning to use Horizontal pod autoscaling in your cluster. You will be able to see cpu and memory utilization metrics with metrics-server.
Metrics Server is a scalable, efficient source of container resource metrics for Kubernetes built-in autoscaling pipelines.
Metrics Server collects resource metrics from Kubelets and exposes them in Kubernetes apiserver through Metrics API for use by Horizontal Pod Autoscaler and Vertical Pod Autoscaler. Metrics API can also be accessed by kubectl top, making it easier to debug autoscaling pipelines.
> Installation link : Metrics Server
- https://docs.aws.amazon.com/eks/latest/userguide/metrics-server.html

### Cluster Autoscaler (CA)
The Cluster Autoscaler automatically adds or removes nodes in a cluster based on resource requests from pods. The Cluster Autoscaler doesn’t directly measure CPU and memory usage values to make a scaling decision. Instead, it checks every 10 seconds to detect any pods in a pending state, suggesting that the scheduler could not assign them to a node due to insufficient cluster capacity.
In the scaling-up scenario, CA automatically kicks in when the number of pending (un-schedulable) pods increases due to resource shortages and works to add additional nodes to the cluster.
> link used :
- https://www.kubecost.com/kubernetes-autoscaling/kubernetes-cluster-autoscaler/

#### Workflow 8
### Horizontal Pod Autoscaler (HPA)
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
- Expose the prometheus server with type NodePort and set target port to 9090,now you can access the prometheus server externally,here a URL is determined.
- In the alert-rules.ymlfile we will declare alert for slack notification and alert for rules in alert manager like 
  InstanceDown,HostOutOfMemory,HostHighCpuLoad,KubePodCrashLooping,KubePodNotReady etc. and sends event notifications to your configured notification channel 
  including like Slack  when your Kubernetes cluster meets pre-configured events and metrics rules in the Prometheus server.
- Now we will configure the service and start it .
- You can view the metrics in the browser by putting Public-IP with the port number 9090
  
#### Grafana
Grafana is an opensource tool which is used to provide the visualization of your metrics.After installing Prometheus successfully, we can install the Grafana and configure Prometheus as a datasource.
> Installation link : Grafana
https://artifacthub.io/packages/helm/grafana/grafana

####  Grafana set-up
- Install prometheus using 
  helm repo: https://artifacthub.io/packages/helm/grafana/grafana
- After installing Grafana ,put the public-ip with port number 3000 and login to grafana using the credentials.
- Click on Datasources, choose Prometheus and set the IP address in grafana.
- A Dashboard is created and we imported necessary dashboard in grafana to visualize the metrics.

#### Workflow 10
### Logging Tools installation and setup
![alt test](https://github.com/namrata4447/Aatmani-Project-Doc/blob/main/EFK%20Overview.drawio.png)

#### EFK [ Elasticsearch, Fluent-bit and Kibana ]
- EFK- is a suite of tools combining Elasticsearch, Fluent-bit and Kibana to manage logs. 
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
- After installing we need to make necessary changes w.r.t network.host: mention Private IP of the instance where elastic and kibana are installed,
  port address: 9200 , discovery.type: single-node , enable Elasticsearch security features to protect your data in elasticsearch.yml file.
- Start and Enable the Elasticsearch service then check the status if Elasticsearch is active.
- You can view the elasticsearch in the browser by putting Public-IP with the port number 9200 and login using the elasticsearch credentials setup.

#### Installation and set-up of Kibana:
- Install Kibana in the same instance[Jumpbox] where Elasticsearch is intalled.
- Install and Configure Kibana using the same link used for Elasticsearch:
  https://phoenixnap.com/kb/how-to-install-elk-stack-on-ubuntu
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


