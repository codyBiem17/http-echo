# CICD Pipeline to Deploy http-echo app to K8S Cluster using Helm
=========
HTTP Echo is a small go web server that serves the contents it was started with
as an HTML page.

The default port is 5678, but this is configurable via the `-listen` flag:

```
http-echo -listen=:8080 -text="hello world"
```

Then visit http://localhost:8080/ in your browser.

## Steps to Set Up CICD Pipeline with Github Actions
==========

This project provides a step by step guide to set up/create a Github Actions Pipeline for deployment for this project to kubernetes cluster, the following DevOps tools have been used:

* VCS: Git/Github
* OS: Linux - Ubuntu
* Container technology/Registry: Docker/Dockerhub, Kubernetes/Minikube, Helm
* CICD tool: Github Actions

## Brief information about each choice of tool above:

### Git
This is version control system that is used to track changes in project repo. It is also known as a source control management. It uses cli commands to help developers interact or manage code in their repo. IMore information about installation can be found [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

### Github
This is an online /cloud-based platform that helps developers to create, store, manage, track and control changes to their code. It is a remote repository that helps developers with collaboration.
[Sign up](https://github.com/) to het started

### Linux/Ubuntu
An open-source operating system. I have installed this on a Virtual Box, this is an alternative approach to using cloud-based VM or WSL on Windows host system. For more installation info, click [vbox](https://www.virtualbox.org/)

### Docker/Dockerhub
**Docker** is a light-weight software for building containerized applications. More info at [Docker](https://docs.docker.com/get-docker/)
**Dockerhub** is a container registry publicly available for developers to find, use and share containers. Dockerhub was used because no cost is incurred. For advanced test, you can use AWS ECR, Azure Container Registry, etc. Sign up at [dockerhub](https://hub.docker.com/)

### Kubernetes/Minikube
**Kubernetes** is a container orchestration tool for managing.scaling containerized applications.

**Minikube** is a local-based kubernetes cluster used for test development. This is easy to use and learn, however, an alternative is Kubernetes in Docker Desktop. More information about [minikube](https://minikube.sigs.k8s.io/docs/start/)

**Helm** is a package manager software used for managaing kubernetes deployment. I have used the [binary realease](https://helm.sh/docs/intro/install/#from-the-binary-releases) option

### Github Actions
This is used to automate workflow in Github repository. Easy to use as no extra/external software is needed to get it working. This is directly integrated in Github repository. More info at [Github Actions](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions)

## Steps to Set Up Project

### Step 1
I have forked project into my github account and cloned afterwards. Fork the project at [http-echo](https://github.com/hashicorp/http-echo). To set up project locally, run the following commands:
```
# clone project

git clone https://github.com/codyBiem17/http-echo.git
cd http-echo

# run/test project locally by installing Go with respect to system OS - for Linux, use the wget command or click the button and then follow the steps under
# [Go installation](https://go.dev/doc/install) by clicking respective OS tab
wget https://go.dev/dl/go1.22.1.linux-amd64.tar.gz 

# run the 'go' app project and visit localhost:8080 in your browser

go build
./http-echo -listen=:8080 -text="hello-world"

```

### Step 2
```
# Dockerize 'go' app project using Dockerfile and Makefile
# I will not dive into details about [Dockerfile] and [Makefile]
# The project uses Makefile to build the docker image using the values defined in the Dockerfile, run the following commands

make docker

docker run --rm http-echo:local
```
To access your containerized application on the browser, use the url format http:<dockerip>:<containerport>. Run docker inspect <container-id> to get dockerip and containerport

### Step 3
Push local docker image to a public container registry. I have used Dockerhub. You must tag your local image with registry host before pushing to any container registry
```
#to push to a registry, run the commands:

docker tag <source-image:tag> target-image:tag # where target-image in dockerhub is username/repository

docker push target:tag

```

### Step 4

Install Helm from official doc: https://helm.sh/docs/intro/install/#from-the-binary-releases. Create an Helm chart and remove
files that are not needed. The major files required for our project deployment with helm chart are deployment, service and values files, modify accordingly

Once done with adjusting the files, deploy a release to cluster. Run '``` minikube service list ``` to get service url

### Step 5

It is time to set up our pipeline using Github Actions

1. Configure Github Secrets with your Dockerhub credentials. Go to your github account under, Settings -> Secrets and Variables -> New Repository Secret.
2. Create the folder '.github/workflows' in your local repo, then create a workflow file with yaml extension
3. Write your pipeline steps to replicate your helm chart deployment process
4. Push to github repo so that workflow gets triggered for continuous integration and deployment