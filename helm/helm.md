# Helm

Distributed technologies uses Helm to deploy services and applications on the platform. The following is a guideline describing how we use Helm. 

## Helm structure

All repositories that contain helm charts has a folder 
called `chart` in the root of the repository.

This folder contains two files:

- Chart.yaml
- values.yaml

And may further contain three folders:

- charts
- templates
- crds

# Best practices

## Subcharts

When we use charts that are made by a third party we will reference it through an umbrella chart, as described in [Umbrella charts](umbrella-charts.md). 
