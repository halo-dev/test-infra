# Halo Test Infra

We have deployed [Prow](https://prow.halo.run/) at <https://prow.halo.run/>.

Please see [command-help](https://prow.halo.run/command-help) to learn how to use Prow to automate Halo development
experience.

## How to build cluster

1. Bring Up Kubernetes Cluster Using [KubeKey](https://github.com/kubesphere/kubekey)

    ```shell
    kk create cluster
    ```

2. Deploy [OpenEBS](https://openebs.io/)

    ```shell
    make -C config/prow deploy-openebs
    ```

3. Deploy [Ingress Controller](https://kubernetes.github.io/ingress-nginx/)

    ```shell
    make -C config/prow deploy-ingress-controller
    ```

4. Deploy [Cert Manager](https://cert-manager.io/)

    ```shell
    make -C config/prow deploy-certmanager
    ```

5. Deploy [Load Balancer](https://metallb.universe.tf/)

    ```shell
    make -C config/prow deploy-metallb
    ```

6. Create GitHub Secrets

    1. github-token

       ```shell
       kubectl create secret generic github-token --from-file=cert=my-prow-test.2022-03-11.private-key.pem --from-literal=appid=179827 --from-file=token=bot-access-token --dry-run=client -oyaml | kubectl apply -f -
       ```

    2. hmac-token

       ```shell
       kubectl create secret generic hmac-token --from-file=hmac=github-webhook-secret --dry-run=client -oyaml | kubectl apply -f -
       ```

7. Deploy Prow

    ```shell
    make -C config/prow deploy-prow
    ```