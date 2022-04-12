# Halo Test Infra

We have deployed [Prow](https://prow.songliping.cc/) at <https://prow.songliping.cc/>.

Please see [command-help](https://prow.songliping.cc/command-help) to learn how to use Prow to automate Halo development
experience.

## How to build cluster

1. Bring Up Kubernetes Cluster

```shell
kk create cluster
```

2. Deploy [OpenEBS](https://openebs.io/)

```shell
make -C config/prow deploy-openebs
```

3. Deploy Prow

```shell
make -C config/prow deploy-prow
```