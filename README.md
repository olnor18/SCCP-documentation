![Energinet logo](images/Energinet-logo.png)

# Overview
The Secure Compute and Communication Platform (SCCP) project is an attempt at creating a modern, fast, secure, robust, and uniform platform for running a variety of application suites for running and monitoring infrastructure.
The platform is made up of multiple components: 

### Hardware
Hardware plays an essential part in the design of the platform. The platform rests on multiple servers that participate in a Kubernetes cluster which provide redundancy. 
Eventually hardware based trust should become the pillar.

### mukube  
Mukube is the operating system of SCCP. It is a custom Linux distribution built using the [Yocto Project](https://www.yoctoproject.org/) and is preconfigured. Features of mukube are:

* RAM based file system using zram
* Secure Boot support

For details on mukube, please refer to the [mukube repository](https://github.com/distributed-technologies/mukube).

### Kubernetes
At the end of the boot seqence mukube deploys a Kubernetes cluster. [Kubernetes](https://kubernetes.io/) is a container orchestration tool responsible for scheduling and supervising workloads on the platform.

### Yggdrasil
Yggdrasil describes the applications and services that run on the platform. Once the Kubernetes cluster is up, Yggdrasil is automatically deployed. This results in the bootstrapping of services and applications of the platform.

Importantly, one of these services is a GitOps operator ([ArgoCD](https://argo-cd.readthedocs.io/en/stable/)), which ensures other services and applications get deployed, and allows for easy configuration of the environment. Information about Yggdrasil and deploying applications can be found [here](https://github.com/distributed-technologies/yggdrasil).

# Deploying
This section will go into detail on how you can deploy SCCP onto different architectures.

### Azure
If you would like to deploy the platform onto Azure for development purposes, please refer to the [Yggdrasil documentation](https://github.com/distributed-technologies/yggdrasil) which provides an in-depth guide on how to deploy the platform on Azure. 

### Deploy to a VM
In order to build the mukube image, refer to the [mukube repository](https://github.com/distributed-technologies/mukube). To configure the image one needs a configuration, created with the [mukube-configurator tool](https://github.com/distributed-technologies/mukube-configurator).
During the first boot the image copies the content a partition labelled `config` to the main file system ([see this](https://github.com/distributed-technologies/mukube/blob/main/meta-k8s-setup/recipes-k8s-configuration/k8s-configuration/files/copy-config-to-state.service)). Thus in order to boot the image with the configuration you must either attach the boot configuration as an additional disk labelled `config` or edit the partition table of the mukube image and add it there, [see](https://github.com/distributed-technologies/wiki/blob/main/mukube/production-image-creation.md). E.g.
1. To deploy a virtual setup runnnig libvirt, see our [terraform module](https://github.com/distributed-technologies/mukube-terraform).
2. To boot in a VMWare setup the it is possible to convert the mukube image to the vmdk format.

### Deploy to hardware
The process is the similar to deploying to VMs. Currently this is untested.

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

# Decision log
Any decisions made during meetings about SCCP that are open to the public can be found in the [decision log](docs/decision-log.md).

# Repos related to the project
This is a list of the repositories that have a relation to the project and what they contain.

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
- [Conventions](git/conventions.md)
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
