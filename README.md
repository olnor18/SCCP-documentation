# wiki

### Git workflows 
The [**Build and release helm chart**](templates/github-workflows/build_release_helm_chart.yaml) workflow, lints,builds and releases a helm chart to github. The values to fill in is:
* asset_path: The name of the output file from helm package
* asset_name: The release file namepatt
