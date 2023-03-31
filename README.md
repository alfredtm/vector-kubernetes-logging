# Vector Kubernetes Logging

![Vector-Elasti Logo](/assets/image.png)



[Vector](https://vector.dev/) is a high-performance, flexible, and extensible logging solution for Kubernetes clusters. This project provides a seamless, easy-to-deploy logging solution designed to handle logs from multiple sources within a Kubernetes cluster and deliver them to Elasticsearch.

## Features

- Selectively collect logs from Kubernetes containers
- Enrich logs with Kubernetes metadata
- Parse JSON data into structured objects
- Forward logs to Elasticsearch efficiently

## Prerequisites

- A running Kubernetes Cluster (For testing, [Kind](https://kind.sigs.k8s.io/) is highly recommended)
- An Elasticsearch instance
- A deployment method for the [Vector Helm chart](https://artifacthub.io/packages/helm/vector/vector) + values file (e.g., [ArgoCD](https://argo-cd.readthedocs.io/en/stable/user-guide/helm/), [Helm CLI](https://helm.sh/docs/intro/install/), [Helmsman](https://github.com/Praqma/helmsman))

## Quick Start

1. **Set up your Kubernetes Cluster and Elasticsearch instance:**
   Make sure you have a running Kubernetes cluster and an Elasticsearch instance ready to receive logs.

2. **Modify the Vector chart values:** 
    ```yaml
    elasticDefault:
      type: elasticsearch
      inputs: ["k8s_logs"]
      api_version: auto
      #  ElasticSearch credentials 
      auth: 
        user: ""
        password: ""
        strategy: "basic"
      mode: "data_stream"
      data_stream:
        sync_fields: false 
        auto_routing: false
        # Data stream name 
        dataset: ""
        namespace: "ds"
      buffer:
        type: disk
        max_size: 268435488
      endpoints: [""]  

    elasticJSON:
      type: elasticsearch
      inputs: ["json_parse"]
      api_version: auto
      # ElasticSearch credentials 
      auth: 
        user: ""
        password: ""
        strategy: "basic"
      mode: "data_stream"
      data_stream:
        sync_fields: false 
        auto_routing: false
        # Data stream name
        dataset: ""
        namespace: "ds"
      buffer:
        type: disk
        max_size: 268435488
      endpoints: [""] 
    ```

3. **Deploy Vector:**
   Choose your preferred deployment method ([ArgoCD](https://argo-cd.readthedocs.io/en/stable/user-guide/helm/), [Helm CLI](https://helm.sh/docs/intro/install/), [Helmsman](https://github.com/Praqma/helmsman)) and deploy the Vector Helm chart with the provided values file.

4. **Verify the setup:**
   Check the logs in Elasticsearch to ensure they are being collected and parsed by Vector as expected.

## Documentation 
For more detailed information on deploying and configuring Vector in your Kubernetes cluster, please refer to the [official Vector documentation](https://vector.dev/docs/).