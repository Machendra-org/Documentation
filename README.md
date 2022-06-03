# Nodewebsite-Project
## _Overview of Project_

I worked in Nodewebsite Project and first we setup the Private Network using AWS resource on VPC (Virtual Private Cloud) after creating the VPC and we are create one instance(Ubuntu Server) on same Network in that we are installing the Terraform software and Java , and using the Terraform software write a terraform script we create another instance(Ubuntu Server) on that instance we are installing the Java,docker,kubectl,Jenkins and Helm, this instance call it as a JumpBox is configuring the EKS Cluster, we clone project details from the github url and we write Dockerfile using the Dockerfile we create custom image and this image push to the ECR after doing all this we write yaml file and deploying the Nodewebsite Application to the Cluster and we are setup the logging and monitoring on this application.

## Project Architecture
![alt text](https://github.com/Machendra-org/Documentation/blob/c081c42d11815adb242501dab7e22640aeafaa23/Project%20Architecture.drawio.png?raw=)
## _Introduction_

In this project we are using many tools are below and i will explains below tools how we used in this Nodewebsite Project.

- GitHub
- Cloud Platform (Amazon Web Services)
- Terraform
- Jenkins
- Docker
- Kubernetes (AWS resource is EKS)
- Monitoring tools (Prometheus,Grafana)
- Logging tools (ELK,EFK)

## Detailed Explaination of Tools and Project:
This Project have six phases in this phases I explained in detailed before I installed some tools below ....
## _Installation of Tools in Ubuntu Server_
First I install the `Java software` using below Link
```sh
https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-on-ubuntu-20-04
```
Installation of `Terraform` using below link
```sh
https://www.terraform.io/cli/install/apt
```
## Phase - 1
In this Phase I used two tools are Terraform and GitHub 
### _Terraform_
Terraform is HashiCorp's infrastructure as code tool. It lets you define resources and infrastructure in human-readable, declarative configuration files, and manages your infrastructure's lifecycle.
> Terraform can manage infrastructure on multiple cloud platforms.
The human-readable configuration language helps you write infrastructure code quickly.
Terraform's state allows you to track resource changes throughout your deployments.

In this Project I write some terraform files for create insfrastrature on Amazon Web Services and all terraform files I pushed to this Github Account refer below URL.
```sh
https://github.com/Aatmaani-org/Devops.git
```
Using terraform I create instance name is JumpBox
### _GitHub_
The GitHub is a Soure Code Management tool and GitHub repository can be used to store a development project. It can contain folders and any type of files. This my project GitHub URL is 
```sh
git clone https://github.com/VV-MANOJ/AatmaaniProject
```
we are create GitHub account and create Organization,Repositories and Branches after created this and Above url contains project information this information push to the we created GitHub Account.
we create two repo of this Project
##### Production repo
  In this repo contains project related source code and Dockerfile.
##### Devops repo
  In this contains Helm chart of Project and Jenkinsfile
In this GitHub we create Branch Protection Rules are..
> Require pull request reviews before merging
> Require status checks to pass before merging
> Require conversation resolution before merging
> Require signed commits
> Require linear history
> Include administrators

this are rules are should not accept direct commits to main branch, it should accept only  merges to main branch.
All my project information have below Github Account.
```sh
https://github.com/Aatmaani-org
```
## Phase - 2

Above we created a one instance name is called JumpBox in that we need to install Java, Jenkins and AWS-CLI, In this phase have two tools are Jenkins and AWS-CLI 
- First I install the `Java software` using below Link
```sh
https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-on-ubuntu-20-04
```
### _Jenkins_
Jenkins is an open source automation server. It helps automate the parts of software development related to building, testing, and deploying, facilitating continuous integration and continuous delivery.
- Installation of `Jenkins` using below Link
```sh
https://www.jenkins.io/doc/book/installing/linux/#debianubuntu
```
In this Project I created a CI/CD Pipeline for automate deploying Application and I installed some Plugins are..
> CloudBee AWS Credentials Plugin
> ECR Plugin
> AmazonEC2 Plugin

I created three Jenkins Jobs for three different Environments are `Dev,QA,Prod` to automate the each Environments deployment.

### _Cloud Platform (Amazon Web Services)_
We are using Cloud Platform is Amazon Web Services(AWS), In Amazon Web Services we are some Services are...
> Elastic Cloud Computing(EC2)
> Virtual Private Cloud(VPC)
> LoadBalancer
> Identity and Access Management (IAM)
> Elastic Container Registry(ECR)
> Elastic Kubernetes Service(EKS)
- Installation of `AWS-cli` using below Link
```sh
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
```
After installing AWS-CLI we need to configure instance to AWS resources using below commands
> aws configure
> After run above command we type your AccessKey, ScreatKey,Which Region your Resource is working that Region Name enter there and format enter Json.

## Phase - 3
The Phase-3 contains two components are Docker and Kubernetes of AWS Resource is EKS
### _Docker_
Docker is an open source Containerization platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications.
- First we Install `Docker` using below Link
```sh
https://docs.docker.com/engine/install/ubuntu/ 
```
In this Project using the Docker and I write Dockerfile for create custom Image and I create a container on this Application.
In this Project Dockerfile we push to the below given GitHub Repo..
```sh
https://github.com/Aatmaani-org/Production.git
```
This Dockerfile used to create Dokcer Image and then Docker image push to the Elastic Container Registry(ECR).

### _Kubernetes (AWS resource is EKS)_
Kubernetes is an open-source container orchestration system for automating software deployment, scaling, and management.
Installation of `Kubectl` using below Link
```sh
https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
```
`Amazon EKS` automatically manages the availability and scalability of the Kubernetes control plane nodes responsible for scheduling containers, managing application availability, storing cluster data, and other key tasks.
We are Attaching the `AutoScalingGroups` and `Metrices server` to this Cluster..
this AutoScalingGroups is used to scale the Node and pods whenever high load in the node or pod.
this Metrice Server is used to gather all node and pods Metrices.

### _Helm_
`Helm` is a Kubernetes deployment tool for automating creation, packaging, configuration, and deployment of applications and services to Kubernetes clusters.
- Installation of `Helm-Charts` using below Link
```sh
https://helm.sh/docs/intro/install/
```
In this Phase I created three Namespaces are..
- Dev
- QA
- Prod

Using Helm I create a Helm-chart using this command `helm create Nodewebsite` I open the Nodewebsite directory in that i see `values` file. we need to create same values for different Environments like below
- values-dev
- values-qa
- values-prod

We Pull Docker Image from `ECR` and we pass image to the `values` file and then  I installed above values files like below..
In Dev we are devloping application of Nodewebsite and pull image to ECR and then installing this file `helm install node-dev Nodewebsite -n dev -f values-dev.yml`
In Qa we are testing the application working fine or not installing this file `helm install node-qa Nodewebsite -n qa -f values-qa.yml`
In Prod we are see application working fine and we increase Replicas and we need add Horizontal Pod AutoScaler, it is used to whenever pods goes down this will creating same pods of that pods. `helm install node-prod Nodewebsite -n prod -f values-prod.yml`
this values files push to the below given GitHub repo..
```sh
https://github.com/Aatmaani-org/Devops.git
```
After Create these things and to automate above Process creating `Jenkinsfile` and my Jenkinsfile have below given GitHub repo..
```sh
https://github.com/Aatmaani-org/Devops.git
```
## Phase - 4
In this Phase we are Installing the Monitoring Tools and Configuring tools to the my EKS Cluster 
## Monitoring Tools
### _Prometheus_
Prometheus is an open-source systems monitoring and alerting, Prometheus collects and stores its metrics as time series data, i.e. metrics information is stored with the timestamp and the Prometheus listening Port is `9090`. 
### Prometheus-Grafana Architecture
![alt text](https://github.com/Machendra-org/Documentation/blob/6287c7a171fc87fce4a7eb64f94dd4e97527068d/Prometheus.drawio.png?raw=)

Installation of `Prometheus` using below Link
```sh
https://linoxide.com/how-to-install-prometheus-on-ubuntu/
```
In this Iam creating a alerting.rules.yml for Checking the below information
- InstanceDown
- HostOutOfMemory
- HostOutOfDiskSpace
- HostHighCpuLoad

this alerting.rules.yml file pass to the `sudo vi /opt/promethues/promethues.yml`
### _Alert-Manager_
The Alertmanager handles alerts sent by client applications such as the Prometheus server. It takes care of deduplicating, grouping, and routing them to the correct receiver integration such as email, Slack. It also takes care of silencing and inhibition of alerts and the Alert-manager listening Port is `9093`.
Installation of `Alert-Manager` using below Link
```sh
https://linuxhint.com/install-configure-prometheus-alert-manager-ubuntu/
```
To send alert to slack from alertmanager edit alertmanager.yml in 
`sudo vi /etc/alertmanager/alertmanager.yml`
### _Node-Exporter_
Node Exporter is a Prometheus exporter for server level and OS level metrics with configurable metric collectors. It helps us in measuring various server resources such as RAM, disk space, and CPU utilization and the Node-exporter listening Port is `9100`.

Installation of `Node-Exporter` using below Link
```sh
https://linuxhint.com/install-prometheus-on-ubuntu/
```
This server IP pass to the prometheus.yml in `static_configs/target/`
### _Grafana_
Grafana is a multi-platform open source analytics and interactive visualization web application. It provides charts, graphs, and alerts for the web when connected to supported data sources and the Grafana listening Port is `3000`.
Installation of `Grafana` using below Link
```sh
https://computingforgeeks.com/how-to-install-grafana-on-ubuntu-linux-2/
```
In this Project I configure some DashBoards in Grafana is given below
- Node Exporter full
- Kubernetes Cluster
- Kubernetes Cluster AutoScaler

## Phase - 5
In this Phase we are using the Logging Tools are EFK and ELK
## Logging Tools
### _ElasticSearch Filebeat/FluentBit Kibana_
EFK is a popular and the best open-source choice for the Kubernetes log aggregation and analysis. It is used to check logs of Kubernetes Cluster.
### Architecture of EFK and ELK
#### ElasticSearch Filebeat Kibana(EFK)
![image alt text](https://github.com/Machendra-org/Documentation/blob/f4e8380a9837d910f30e914a88af27ceef223736/EFK.drawio.png?raw=)
#### ElasticSearch Logstash Kibana(ELK)
![image alt test](https://github.com/Machendra-org/Documentation/blob/1bfa30a6ecab16fa13b5ebf208a792c2b4cf2d18/ELK.drawio.png?raw=)
#### ElasticSearch
Elasticsearch is a NoSQL database, Elasticsearch also allows you to store, search and analyze big volume of data. the ElasticSearch listening Port is `9200` 
Installation of `Elasticsearch` using below Link
```sh
https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-20-04
```
In elasticsearch.yml we are edit some sections are Network and Discovery  
#### Filebeat
Filebeat is a lightweight shipper for forwarding and centralizing log data. Installed as an agent on your servers, Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
Installation of `Filebeat` using below Link
```sh
https://logit.io/sources/configure/ubuntu
```
We are edit filebeat.yml in that we are add elasticsearch IP to the filebeat.output section.
#### FluentBit
Fluent Bit is a lightweight log processor and forwarder that allows you to collect data and logs from different sources, unify them, and send them to multiple destinations. this installation is done by using `Helm-chart`
Installation of `FluentBit` using below Link
```sh
https://artifacthub.io/packages/helm/fluent/fluent-bit
```
In this FluentBit we are edit congfigmaps of FluentBit,we give Elasticsearch IP to fluent-bit.conf/Output section.
#### Kibana
Kibana is a visualization tool, which accesses the logs from Elasticsearch and is able to display to the user in the form of line graph, bar graph, pie charts etc.
Installation of `Kibana` using below Link
```sh
https://www.elastic.co/guide/en/kibana/current/deb.html
```
In kibana.yml we are edit some sections are server.host and server.port. and the Kibana listening Port is `5601`
#### Logstash
Logstash is the data collection pipeline tool. It collects data inputs and feeds into the Elasticsearch. It gathers all types of data from the different source and makes it available for further use.
Installation of `Logstash` using below Link
```sh
https://www.elastic.co/guide/en/logstash/current/installing-logstash.html
```
Collect logs and events data. It even parses and transforms data.
## Phase - 6
In this Phase I Explain what causes happens to me, but it is small cause and what cause is Verisons mismatch Elastic Filebeat/fluent-bit Kibana, after time we sort out this problem installing correct versions of Elastic Filebeat/fluent-bit Kibana. before installation tools you need check prerequisite that particular and Versions.

### Conclusion
This Project help me to improve how do Project, I Learn new things like Monitoring tools (Prometheus,Grafana), Logging tools (EFK and ELK) and creating Documentation of Project.









