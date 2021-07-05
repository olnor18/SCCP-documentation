### Helm release pipeline
When a helm chart repository is being developed there is a need to contineously release these for others to use, both for new features and for bugfixes.

The releases of the helm chart sholud be as follows:
1. On merge with main branch:
** Trigger Github workflow that, builds a package, creates a release and attaches the package as asset to the release.
2. On new asset upload to release: 
** Push the package to ArtifactHub with the released versoin. 
