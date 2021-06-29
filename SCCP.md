<img src="images/Energinet-logo.png" width="250" style="margin-bottom: 3%">

**Secure compute and communication platform** has emerged as a tool to deal with some of the problems the energy system is facing, as the production is shifting towards more renewable energy. One of the bigger problems is that renawable energy forces the system to run close to the edge as the production of energy becomes highly fluctuating. This calls for improved algorithms that helps operators of the critical systems make decisions. The SCCP is the foundation on which these models will live in any given situation, providing a platform that raises the bar for security both for communication and computation. 

### Architecture
The platform is essentially a kubernetes cluster with an operator supporting the Gitops workflow. Following the Gitops workflow all infrastructure is configured as code in separate git repositories as individual services. A single environment repository holds the desired configuration of each service and the agent continously pulls them in and changes the actual state accordingly.   

### Hosting
The platform is cloud agnostic, meaning that it will be able to be deployed onto both in bare metal situations as well as on deployed in any cloud provider thus greatly increasing the stability and scalability of the platform, because vendor lock-in is avoided. Avoiding vendor lock-in we reduce the risk caused by cloud providers having an outage, a security event or simply discontinues sevices, as a cloud agnostic platform can seamlessly move to whereever fits its demands. 
Compiling our own Linux operating system, we reduce the dependencies and packages to a minimum, further reducing the potential attack vectors for adversaries. Basically, the only features enabled are the ones that are needed to start up a Kubernetes cluster. 

### Security
Taking the next step in security the platform is enabling a feature to fuse hardware and software security in a trusted execution environment. In its essence this means that the hardware will not run any software that does not have the correct digital signatures and the software will not run on any hardware that does not have the correct digital signature.

### Technologies 
All technologies that are used to build the SCCP are open source.

#### [ArgoCD](https://github.com/argoproj/argo-cd/) 
ArgoCD is used as the GitOps tool that contineously delivers changes to the platform based on pull request changes.

#### [Prometheus](https://github.com/prometheus/prometheus)
Prometheus is used as the monitoring time series database, that continuously scrapes metrics from all containers that expose them.

#### [Grafana](https://github.com/grafana/grafana)
Grafana is used as the monitoring visualization tool, where dashboards provide insights into the metrics.

#### Ceph
Ceph is used through an [opensource Rook operator](https://github.com/rook/rook) as a distributed persistent storage, handling jobs like replication to avoid data loss at machine failure. 

#### [Metallb](https://github.com/metallb/metallb)
Metallb is used as the software loadbalancer delegating IPs to services that needs a static IP. 

#### [Traefik](https://github.com/traefik/traefik)
Traefik is used as an ingress controller handling ingress events to route communication.

### Roadmap
- [x] Monitoring 
- [x] Ingress 
- [x] Loadbalancer 
- [x] Distributed storage 
- [ ] TEE
- [ ] PKI infrastructure
- [ ] One click build pipeline
