# Kubernetes Boostrap

This is a repository with a bootstrap Kubernetes configuration, that configures
base services, that usually needed for the most Kubernetes clusters.

It configures from the scratch:

- A load balancer that uses external IP adresses of the nodes.

- Ingress, using ingress-nginx.

- Cert-Manager, using Let's encrypt to generate valid SSL certificates, or
  self-signed certificates.

- A LocalPath storage provisioner with ReadWriteMany mode enabled.

- Metrics Server with Prometheus and Grafana.

## How to configure and deploy

1. Copy the `settings.example.yaml` file to the `settings.yaml` in the same
   directory:
   ```
   cp settings.example.yaml settings.yaml
   ```

2. Change in it the example values to actual values, related to your cluster:

- `contactEmail`: A email address to use when configuring Let's Encrypt.

- `loadBalancerIps`: A list of external IP addresses of your nodes, to use them
  in the LoadBalancer.

- `sharedName`: A name to use when sharing resources between several services.

- `certManager`: Certificates manager configuration.

3. Install [Helmwave](https://github.com/helmwave/helmwave) utility that
   simplifies managing configuration of the whole cluster in a single
   repository. [How to install Helmwave >>](https://docs.helmwave.app/)

4. Build the resources and check the deployment plan using a command
   ```
   helmwave build
   ````

5. Deploy the changes:
   ```
   helmwave up
   ````

6. When you update some settings or add something new, use this command to
   deploy your changes:
   ````
   helmwave build && helmwave up
   ```

## Recommended structure for your whole cluster

To manage the whole cluster in a single repo, I recommend this structure:
```
.
├── infrastructure # k8s-bootstrap repo as a git submodule
│   └── settings.yaml
├── services # Your custom services in the cluster
│   └── helmwave.yml
├── project1
│   └── helmwave.yml
└── project2
    └── helmwave.yml
```
So, to add the `k8s-boostrap` to your cluster repo, use the command:
```
git submodule add https://github.com/Murz-K8s/k8s-bootstrap infrastructure
```
This will download the `k8s-bootstrap` repo into the `infrastructure`
subdirectory, so you will manage in your git repository only the custom settings
in the `settings.yaml` file.
