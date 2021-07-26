### Helm release pipeline
When a helm chart repository is being developed there is a need to continuously release these for others to use, both for new features and for bug fixes.

The releases of the helm chart should be as follows: 
 * Trigger Github workflow on merge with the main branch:
 1. package the Helm chart.
 2. create a GitHub release.
 3. attach the package as an asset to the release.
 4. push the asset to our [helm registry](https://github.com/distributed-technologies/helm-repository) and update the index file for the helm registry.
  
To authenticate with the helm repository that acts as a helm registry we use organization tokens for email and auth token.
The described workflow is accessible as a [template](https://github.com/distributed-technologies/.github/blob/main/workflow-templates/check_helm_chart_version.yaml) that can be used on all organization repositories.
