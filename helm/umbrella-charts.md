# Umbrella charts

## Motivation
During development, the team has encountered multiple problems with not being able to add our own manifests to the applications and services we deploy on our cluster. For example, we sometimes want to use our own PVC for certain services, but the Helm charts for those services do not allow us to create custom PVCs.
When referring to external Helm charts, whether that be a chart that is located on the Open Olympus Platform or an external on Github, there are also times when we would like to create a base configuration for a Helm chart. However, since this base will have configurations that match our environment but not necessarily others, we need to be able to both store it, but also control the access to the configured Helm chart. 

## Initial workaround 
When development started, we had talks about forking vs. creating an umbrella chart with a dependency. We came to the conclusion that we did not want to fork any repositories, since they would fall behind and not get any updates unless we manually did this for every repository we had cloned. Since we use a large number of tools that are often updated, we did not find this to be a possible solution. We then started to create an umbrella chart for every Helm chart we needed to modify the values file of, which worked fine as an initial solution to the problem.

## Problem
We decided to align with another team at Energinet who are working in the same domain as us. They already had a lot of the configurations for the services we will needed to implement and it seemed like a good idea to create a common base of repositories that could be deployed on any platform. 

- Problem 1: We would like to be able to have our own specific configurations for all of our clusters be the base of the repository, but this configuration is not necessarily aligned with what other would like. 
- Problem 2: If the base configuration from problem 1 is merely created on a branch, how will branch protection work since our "main" configuration no longer the main branch? 
- Problem 3: How will we add custom manifests and resources to the repository? This would be on a branch in the repository and could quickly become unclear. 
- Problem 4: We would like to test our Helm charts to make sure that both current and new versions will have the correct functionality needed. 

## Discussion
The team had a discussion on the best way to try and either solve these problems or at least find a feasible solution that will at least work temporarily. We had multiple suggestions in play, one being to create a single repository to hold all custom manifests and resources. However, during the discussion, our ideas seemed to add more complexity and change the way Yggdrasil would work. After some discussion we ended up on the concept that we already had been using before which was umbrella charts. 

## Umbrella charts
An umbrella chart is a chart that holds another chart(s) as a dependency. This enables us to put custom manifests and resources into the templates folder of the umbrella chart, add acceptance tests, as well as having a protected main branch and good branching conventions. This means that we will add a repository for every service we have running on the cluster, even if we don't initially configure it. 

## How to create an umbrella chart
1. To create an umbrella chart, first follow the steps in [creating a repository](../git/repo_creation.md).
2. After having done this, you need to clone the repository onto your local machine, create a new branch and run ```helm create chart``` from the root folder of the repository. This will create a boiler plate helm chart for you to edit.
3. Run the following command to clean out the Helm chart: ```cd chart && rm -rf charts && rm -rf templates/*```. Furthermore, you can delete the content of `values.yaml` and remove all the comments and the AppVersion field from `Chart.yaml`. 
4.  Add your dependency in `Chart.yaml`. An example of how this is done can be found [here](https://helm.sh/docs/helm/helm_dependency/). It is possible to add an install condition to each dependency, so the deployment of the dependencies can be controlled from the values file. [Here](https://helm.sh/docs/chart_best_practices/dependencies/#conditions-and-tags) is an example of how to add a condition to a dependency. 
5.  Add any **general** settings to the values file in the umbrella chart. These are the values that will be set for any install of the umbrella chart, unless overwritten in Yggdrasil, meaning that only generally needed and accepted values should be defined in the values file of the umbrella chart. 
6.  Create a pull request into the main branch. 
