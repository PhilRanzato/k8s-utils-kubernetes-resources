# Golang Backend

Create the kubeconfig for the service account

```shell
./create-sa-config.sh
```

Create the config map for the golang backend deployment with:

```shell
kubectl create cm kubeconfig --from-file=config
```
