# Helm

**Everything** that Distributed technologies will deploy on 
our clusters will be done through helm.

## Helm structure

All repositories containing helm charts contain one folder 
name '/chart' in the root of the repository.

This folder will always contain 3 files:

- ./Chart.yaml
- ./values.yaml
- ./values.schema.json

And may further contain 3 folders:

- ./charts/
- ./templates/
- ./crds/

The values.schema.json is used to validate the value input 
files, and should always ensure correct use of the chart, 
more info can be found here: https://helm.sh/docs/topics/charts/

# Best practices

## Subcharts

**Whenever** we use charts that are made by a third party and 
only require changes to the values file we will **reference** 
their chart instead of copying it. This done by adding a dependency 
section to *Chart.yaml* file, see below.

    dependencies:
      - name: argo-cd
        version: "2.17.1"
        repository: "https://argoproj.github.io/argo-helm"

The repository is not a Git repository but the Chart repository.
All chart repositories can be explored using a browser navigating 
to {repository}/index.yaml in the example above 
https://argoproj.github.io/argo-helm/index.yaml

### Values

When overwriting the values of a subchart one adds a new section
to the values file with the name of the subchart.

    #values.yaml
    somevalue: hello

    argo-cd:
      somevalue: world

In the example with argo-cd everything in the argo-cd will be forwarded
and will overwrite the values in the values.yaml file in the subchart.

More can be read [here](https://helm.sh/docs/chart_template_guide/subcharts_and_globals/)
