# hasura-helm-chart
Kubernetes Helm chart to deploy Hasura and initialize the metadata and migrations from a git repository. 
The Helm charts also contain a questions.yaml file which provides an enhanced user experience when deploying Hasura
via Rancher.

To use the helm chart, first add the helm repo:

```
helm repo add hasura https://2start.github.io/hasura-helm-chart/
```

If you don't use Rancher to install the Helm chart. Please take a look at the [values.yaml](charts/hasura/values.yaml) file to see all the values that you can overwrite. For descriptions of the values you can take a look into [questions.yaml](charts/hasura/questions.yaml). It should be self explaining and describe all the parameters that you are supposed to change.
