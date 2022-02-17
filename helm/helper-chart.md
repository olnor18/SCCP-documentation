# Helper charts
### Dependency
The purpose of a helper chart is to provide a standard way of deploying the same resource along with different charts.

<!-- Lets say f.eks. that you have an application the uses a `ceph-bucket-claim` you then add the `ceph-bucket-claim` to a yaml file in the templates folder for your helm chart, and that is fine.
But then you have a second chart that also needs a `ceph-bucket-claim` so you do the same thing, add the resource to the template folder.

But now you have two charts that uses the same resource and this could be done in two different ways.

This is where using a helper chart would be helpful, instead of making resources in the template folder for both these charts, you instead add a dependency to both charts.
Something like:
```yaml
dependencies:
- name: helper-ceph-crd
  version: "0.1.0"
  repository: "https://distributed-technologies.github.io/helm-charts/"
  condition: helper-ceph-crd.enabled
```

This will then work like the umbrella charts where you write the name of the dependency chart as the first map and add values to that map.
f.eks.
```yaml
helper-ceph-crd:
  enabled: true
```

Will enable the chart with the default values (Not adviced).

Of couse you would have to add the right values to the helper chart, but refer to the helpers `values.yaml` for that -->

So now instead of having a `ceph-bucket-claim` resources defined in each chart, you have a reference to the same `ceph-bucket-claim` in the helper chart, giving us a single source of truth for that resource.

Because these charts will be the single source of truth for a resource we can also define certain "hardcoded" values that we know we would always want to use in relation to a given resource.

An example could be deploying a CRD with the annotation `argocd.argoproj.io/sync-options: SkipDryRunOnMissingResource=true`


### Stand-alone helper chart
There is also a second way of using a helper chart, as a stand-alone deployment.
Lets say some deployments want to use the same bucket to share data between them. Instead of the `object-store`, `storage-account`, and `bucket-claim` being tied to a single application and it's lifespan, it could be deployed as a stand-alone with it's own lifespan.

This would be done by adding it to the `config.yaml` in Yggdrasil like any other service.
