# Devops---internship---assignment
#version control (git &ssh )
##SSH setup 
-ssh key generated using "ssh-keygen" in cli 
-public & private key was generated where public key was given to github  and private key is to be with in our server  
-ssh keys are for authenticate the pull pulls and git merge to the repositories itsmore secure then this repository is cloned through ssh.
## Git pull vs GIT fetch
-git fetch gives the chnages that are made to the remote  repository and git pull fetches and merges changes into our current branch 
## Merge conflict simulation
-if we have two branches for developemt (dev) and production (prod) then if we edit the same thing in both branches then triggers a conflict
-we can resolve by [ git checkout dev, git merge prod ] then git add .
then git commit -m
-----------------------------------------------------------------------------------------------------------------
2. DOCKER containerization
 ## docker file
1. this dockerfile consists python as base image,then dependencies to craete a image out of it we use dockeriles to craete a docker image then this docker image runs as acontainer
 # The difference between Dockerfile, Docker image, and Docker container. 
2. dockerfile- it is packaged artifact where it have all the dependencies ,base images to build an image from it
dockerimage - build from the docker file contains os, appcode everything
 Docker container - like a running instance which has docker image
 # How you would reduce the image size if your first build is too large. 
to reduce the  if build is large we have to use small base image & using no-cache-dir in pipinstall gives no caching and adding .dockerignorefile give no unneccesary files
#  Run your application container locally and show it working (screenshots/logs are fine).
i have used my project image as an example to run container 
   ---------------------------------------------------------------------------------------------------------------
 3: Kubernetes (EKS Basics)
 # What is the difference between a Pod, Deployment, and Service in Kubernetes? 
 - pod is a smallest deployable unit in the kubernetes this pods contains docker containers running in them
 - deployments are the controllers of the pods or replicasets ensures desried and actual state of replicasets or pods and manage scalling and rolling updates
 - on top of the pods and deployments we have service for network enndpoint to access pods by service we can have permannent dns
# Why do we need EKS (or managed Kubernetes) instead of running Kubernetes on 
VMs? 
 - in virtual machine type we need to install, configure and upgrade the kubernetes  control plane , we have to manage etcd (cluster database), API server, scheduler, networking, and security patches.
 - in EKS we only manage worker nodes , aws itself do Automatic upgrades, security patches, and HA out of the box
 - so EKS is far better option
  # Create a simple Kubernetes YAML file that deploys your app (from Part 2) with: 
# 2 replicas 
# A LoadBalancer service (dummy, it doesn’t need to be deployed to AWS, just the 
YAML)
- created a yaml files which gives 2 replica sets and exposes to port 80 (nginx)
  ---------------------------------------------------------------------------------------------------------------
  4.CI/CD Pipeline
  # Write a basic GitHub Actions workflow (or Jenkinsfile if you prefer) that: 
# Builds your Docker image 
# Runs simple tests (can be just echo "Tests passed" for this assignment) 
# Pushes the Docker image to DockerHub (you can simulate without pushing if you 
don’t want to set up credentials)
#  Explain how this pipeline would change if we wanted to deploy to Kubernetes after building. 
Add kubeconfig setup (to connect to your cluster).
Add step to apply Kubernetes manifests:
      - name: Deploy to Kubernetes
        run: |
          kubectl apply -f k8s-deployment.yaml
          ---------------------------------------------------------------------------------------------------------
#  : Monitoring & Logs 
1. Explain the difference between metrics, logs, and traces.
     Metrics → Numeric time-series data (CPU, memory, request latency).
Logs → Textual records of what happened in the system (errors, debug info).
Traces → Show request flow across microservices (helps debug latency).
#  If your Kubernetes pod crashes, how would you debug the issue? (write the commands and 
reasoning).
describe → shows why pod failed (crash loop, scheduling issue).
logs → check container logs for Python/Java errors.
events → cluster-level issues (OOM, image not found, etc.).
# What tools would you suggest for monitoring in AWS EKS and why? (Prometheus, 
CloudWatch, Grafana, ELK stack, etc.).
Prometheus → Scrapes metrics from pods.
Grafana → Visualizes metrics, dashboards.
CloudWatch → Native AWS logging & metrics (easy integration).
ELK stack (Elasticsearch, Logstash, Kibana) → Centralized logging at scale.
#  Problem-Solving Scenario 
You are asked to set up a new microservice in AWS EKS. The team says: 
#  Code is on GitHub 
# Needs to be containerized 
# Should auto-deploy on merge to main branch 
# Logs should be visible to the dev team 
# Question: How would you approach this? Write down the steps you would take (high-level design, 
not exact code). 
-To set up a new microservice in AWS EKS, the first step I would take is to containerize the application. Since the code is already in GitHub, I would create a Dockerfile that defines how the application should be built and packaged into a container image. This image would then be stored in a container registry, such as Amazon ECR or DockerHub, so that it can be easily accessed by Kubernetes. Containerization ensures that the application runs consistently across different environments and makes it portable.

Next, I would set up a Kubernetes deployment for the service. This involves writing YAML manifests for both the Deployment (to define the desired replicas and rolling update strategy) and the Service (to expose the application within the cluster or to the internet via a LoadBalancer). These Kubernetes manifests would also be version-controlled in the GitHub repository so that any infrastructure changes are tracked alongside the application code.

For automation, I would configure a CI/CD pipeline using GitHub Actions. The pipeline would trigger on every merge to the main branch, build the Docker image, run basic tests, push the image to the registry, and finally apply the Kubernetes manifests to the EKS cluster using kubectl. This ensures that every code change is automatically deployed without manual intervention, supporting faster iterations and reducing human error.

Finally, I would set up centralized logging and monitoring so that the development team can easily view application logs and metrics. In AWS EKS, the most straightforward option is integrating with CloudWatch for logs and metrics collection. This allows developers to see pod logs directly in CloudWatch dashboards without needing access to the Kubernetes cluster. For more advanced monitoring, I would also consider Prometheus and Grafana for metrics visualization, or the ELK stack for richer log analysis. This combination ensures that the dev team has visibility into application health, performance, and failures.
