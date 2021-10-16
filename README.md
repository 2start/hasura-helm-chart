![release_workflow_bage](https://github.com/2start/hasura-helm-chart/actions/workflows/releases.yml/badge.svg)

# hasura-helm-chart
Kubernetes Helm chart to deploy Hasura and initialize the metadata and migrations from a git repository.
The Helm charts also contain a questions.yaml file which provides an enhanced user experience when deploying Hasura
via Rancher.

## Installation

### Basic Installation

To use the helm chart, first add the helm repo:

```
helm repo add hasura https://2start.github.io/hasura-helm-chart/
```

1. Please take a look at the [values.yaml](charts/hasura/values.yaml) file to see all the values that you can overwrite. For descriptions of the values you can take a look into [questions.yaml](charts/hasura/questions.yaml). It should be self explaining and describe all the parameters that you are supposed to change. 
2. Then create a file `overwrites.yaml` which contains the keys from the `values.yaml` file.
For example:

```yaml
hasura:
  metadataDatabaseUrl: "postgres://myuser:mypassword@myhostname:myport/mydbname"
  adminSecret: "mysuperlongsecret1"
  metadataDir: "path/to/my/metadata/in/my/git/repository"
  migrationsDir: "path/to/my/migrations/in/my/git/repository"
  gitRepository: "https://gitlab-ci-token:myaccesstoken@gitlab.de/myuser/hasura.git"
```

### SSH Deploy Key 

You can authenticate to your git repository by SSH, for example by using a deployment key at Github (or your usual SSH key). To store the ssh key in k8s execute:

```
kubectl create secret generic my-hasura-ssh-key-secret --from-file=repo-private-key=path/to/my/private/key
```

Feel free to change the secret name from `my-hasura-ssh-key-secret` but leave the name of the secret file 'repo-private-key'.
If you specify an ssh key, add the secret name to the `overwrites.yaml`.

```
hasura:
  ...
  gitSshKey: my-hasura-ssh-key-secret
```

### Hasura Image

Set the following values to change the image:

```
image:
  repository: hasura/graphql-engine
  tag: v2.0.9.cli-migrations-v3

```

The Hasura initialization from a git repository relies on the `cli-migrations-vX` image variants of Hasura. The images check for migrations and metadata in a mounted folder and apply them after the container starts. 

### Rancher Installation
Add the repo `https://2start.github.io/hasura-helm-chart/` via the GUI.
Launch a new App afterwards. The autogenerated form fields specified in [questions.yaml](charts/hasura/questions.yaml) will guide you through the configuration process.

# Post Installation


1. Create an ingress to retrieve traffic from outside the k8s cluster.
2. Create a DNS entry that points to the ingress controller.

For more information, see [NOTES.txt](charts/hasura/templates/NOTES.txt) which will be shown to you after the installation.
