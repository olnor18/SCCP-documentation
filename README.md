<img src="images/Energinet-logo.png" width="250" style="margin-bottom: 3%">

# Overview
Secure compute and communication platform(SCCP) is a highly available, fast, secure, robust platform which will run future applications for the energy grid in Denmark. The platform consists of three main components: 

### Mukube  
Mukube is the operating system on which SCCP runs. It is a configuration of Poky, the Yocto project's reference distribution. To read more on Mukube, please refer to the [documentation](www.url-to-mukube-docs-that-i-could-not-find.com).
### Kubernetes
Once the operating system boots, it will automatically deploy Kubernetes and attempt to join the cluster created by the first master node. Kubernetes is a container orchestration tool and more on this can be found [here](https://kubernetes.io/).
### Yggdrasil
Yggdrasil is the top part of the platform that boots on top of Kubernetes. Once the Kubernetes cluster is up, Yggdrasil will automatically be deployed onto the cluster and bootstraps all services and applications into a running state. One of these services is a GitOps operator, which allows for easy configuration of the environment. More on Yggdrasil can be found [here](https://github.com/distributed-technologies/yggdrasil).

# Getting started
This section will go into detail on how you can deploy SCCP onto different architectures. 

### Azure
If you would like to deploy the platform onto Azure for development purposes, please refer to the [Yggdrasil documentation](https://github.com/distributed-technologies/yggdrasil) which provides an in-depth guide on how to deploy the platform on Azure. 

### Hardware
To deploy SCCP on bare metal...

### Virtual Machines
...

# Design choices
The platform has been designed with many different use cases in mind. It can be deployed on both cloud as well as bare metal. There is no strict requirement for a number of nodes you need in your Kubernetes cluster, however, the platform has been developed with a constant goal of achieving high availability. 

There were a number of requirements for the platform from before development started. 

1. One command should bootstrap Yggdrasil
2. Booting Mukube should bootstrap the entire platform automatically
3. We would like to be able to monitor the system with itself
4. Infrastructure as code principles followed
5. We will make Helm releases of any service or application, so an upgrade to a newer version of the platform does not require pulls, but rather pointing to a newer version
6. The platform has to be highly configurable to anyone using it
7. We would like to use ArgoCD as our continuous delivery tool
8. Allow certain services to boot before others

These are just some of the many requirements there were for the platform and many more have come forward during development. 

### Yocto
After having completed a successfull PoC of the platform, we soon realized that the build method we were using was slow and did not have all the features we needed. We spent some time looking for other build tools and found the Yocto project. Yocto takes advantage of caching layers of the operating system, which means that if anything changes in a particular layer, only that layer has to be recompiled. This sped up OS compiling significantly and has allowed us to automatic builds. 

# Repos related to the project

## [Mukube](https://github.com/distributed-technologies/mukube)
Mukube is the repository for the Poky-based operating system we build. 

## [Yggdrasil](https://github.com/distributed-technologies/yggdrasil)
Yggdrasil is the top part of SCCP that is deployed onto the Kubernetes cluster. It holds all the services and applications on the platform. 

## [helm-charts](https://github.com/distributed-technologies/helm-charts)
The helm-charts repository holds the proxy charts for all of our services. This is also where we lint and test the services that we deploy onto the platform. 

## [Filemover-poc](https://github.com/distributed-technologies/filemover-poc)
The PoC for the filemover that will replace the old filemover at Energinet once it reaches its EOL. 

# License
The project uses a lot of cloud native open source projects. We are currently using the Apache 2.0 license for the repositories we create. This does, however, not overwrite the licenses that the respective open source projects choose to use and we will always honor the terms in the licenses they have chosen. 

# For developers

## Git
- [Convensions](git/conventions.md)
- [Branch naming and use](git/branches.md)
- [Main branch protection](git/main-branch.md)
- [Repository creation](git/repo_creation.md)
- [Repository structure](git/structure.md)
- [Github workflows](git/workflows.md)
## Helm 
- [Helm](helm/helm.md)
- [Testing](helm/test.md)
- [Umbrella charts](helm/umbrella-charts.md)
## mukube
- [Secure Boot](mukube/trusted_execution/secure-boot.md)
- [TPM](mukube/trusted_execution/tpm.md)
- [Virtualization management platform](mukube/virtualization-management-platform.md)
- [Production image creation](mukube/production-image-creation.md)
