# My App Chart

This chart deploys a complete application stack including n8n, MinIO, PostgreSQL, and a Discord service.

## Prerequisites

- Kubernetes cluster
- Helm v3+
- A running Traefik v2+ Ingress Controller in the cluster

## Installation

1. **Create your values file**

   Copy `values.yaml.example` to `values.yaml` and update it with your specific configuration (passwords, domain, etc.).

   ```sh
   cp values.yaml.example values.yaml
   ```

2. **Add `values.yaml` to `.gitignore`**

   Ensure your `values.yaml` file containing secrets is not committed to git.

3. **Install the Chart**

   Run the following command from the `knowleadge_base_infra` directory:

   ```sh
   helm install my-release ./k8s/app-chart
   ```
   Helm will automatically use the `values.yaml` file in the chart directory.

## Post-Installation: Initializing MinIO Buckets

The chart will deploy the applications, but the MinIO buckets need to be created manually by applying the initialization Job.

After the `helm install` command has successfully completed and the MinIO pod is running, execute the following command:

```sh
kubectl apply -f ./k8s/app-chart/init-jobs/create-buckets.yml
```

This job will connect to your MinIO instance and create the default bucket and folder structure defined in your `values.yaml`.
