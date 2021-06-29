(INSET LOGO)
## SCCP
The secure computation platform has emerged as a weapon to deal with some of the problemes the energisystem is facing, when the energy production is becomming renewable. One of the bigger problems are that renable energi forces the system to run close to the edge as the production of energy becomes highly flucturating. This calls for improved algorithms helping operators of the critical systems making decitions. The SCCP is the foundation on which these models will live in any given situation. 

### Architecture
The platform is essentially a kubernetes cluster with an agent supporting the Gitops workflow. Following the Gitops workflow all infrastructure is configured as code in sepperate service git repositories. A single environment repository holds the desired configuration of each service and the agent continously pulls them in and changes the actual state accordingly.   

### Hosting
The platform is cloud agnostic, meaning that will be able to be setup both in bare metal situations as well as deployed in any cloud provider thus greatly increasing the stability and scalability of the platform, because vendor lock is avoided. 

TODO write about Mukube

### Security
Taking the next step in security the platform is enabeling a feature to fuse hardware and software security in a trusted execution environment. In its essence this means that the hardware will not run any software that does not have the correct digital signatures and the software will not run on any hardware that does not have the correct digital signature.

### Technologies (inset logos)
All technologies that is used to build the SCCP is open source. 
#### ArgoCD 
ArgoCD is used as the GitOps tool that contineously delivers changes to the platform based on pull request changes.

#### Prometheus
Prometheus is used as the monitoring time series database, that contineously scrabes metrics from all containers that expose them.

#### Grafana
Grafana is used as the monitoring visualization tool, where dashboards provide insights into the metrics.

#### Ceph
Ceph is used as a distributed persistent storage, handeling jobs like replication to avoid data loss at machine failiure. 

#### Metallb
Metallb is used as the software loadbalancer delegating ips to services that needs a static ip. 

#### Traefik
Traefik is used an ingress controller handeling ingress events to route communication.

### Roadmap
- [x] Monitoring (done)
- [x] Ingress (done)
- [x] Loadbalancer (done)
- [x] Distributed storage (done)
- [] TTE
- []PKI infrastructure
- []One click build pipeline
