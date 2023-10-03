
# Github Action for Kubernetes CLI

This action provides `kubectl v1.25.14` for Github Actions.

## Usage

In your workflow, here is an example that deploys your new image and verifies it is successful.

```yaml
on: push
name: Deploy
jobs:
  deploy:
    name: Deploy to cluster
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    - name: Deploy to cluster
      uses: mjamaah/kubectl@master
      with:
        config_data: ${{ secrets.kube_config }}
        args: set image --record deployment/<my-deploy> <my-container>=<my-image>:<new-tag>
    - name: Verify deployment
      uses: mjamaah/kubectl@master
      with:
        config_data: ${{ secrets.kube_config }}
        args: '"rollout status deployment/<my-deploy>"'
```

## Kube configuration

Make sure to base64-encode your kubeconfig file and put it in a github secret.  You can get the string by running:

```bash
cat $HOME/.kube/config | base64
```
