# AKS Store Demo + Promethius Operator + ArgoCD

Link to application: [Elastisys Demo Website](http://20.62.242.140/) (If not responding cluster might be turned off, please contact Frans Sjöström on +46762675134 or email fransjostrom@gmail.com). The cluster is turned off manualy when not used to save on expenses.

This project is based on [Getting started with Azure Kubernetes Service presets](https://github.com/Azure-Samples/aks-store-demo) and has been modified to use ArgoCD and the Promethius operator. 

Currently ArgoCD only has runs the base application. But in future work it could also handle the promethius operator.

The Promethius operator is not configured to scrape any metrics from the application exept the default *kube-state-metrics*

The start and admin page has been modified so that anyone that accesses them can navigate between the diffrent services like Grafana, Promethius, ArgoCD and Alertmanager. 

## Security Notice
This project is not in a secure state. 

**Future security fixes:**

- Make container run as users and not root. 
> Note: There where a considerate push to fix this using assets on the internet including [How to stop running containers as root - Elastisys](https://elastisys.com/howto-stop-running-containers-as-root-in-kubernetes/). But The applications where configured without this in mind and therefore the effort was postponed.
- Make the Admin website require credentials 
- Isolate promethius operator tools like grafana and also ArgoCD to only be reachable by authenticaded users inside isolated secure network. 
- Setup container registry scanner for vulnarbilities


## Architecture

The application has the following services: 

| Namespace: default |
| Service | Description |
| --- | --- |
| `makeline-service` | This service handles processing orders from the queue and completing them (Golang) |
| `order-service` | This service is used for placing orders (Javascript) |
| `product-service` | This service is used to perform CRUD operations on products (Rust) |
| `store-front` | Web app for customers to place orders (Vue.js) |
| `store-admin` | Web app used by store employees to view orders in queue and manage products (Vue.js) | 
| `virtual-customer` | Simulates order creation on a scheduled basis (Rust) |
| `virtual-worker` | Simulates order completion on a scheduled basis (Rust) |
| `mongodb` | MongoDB instance for persisted data |
| `rabbitmq` | RabbitMQ for an order queue |

![Logical Application Architecture Diagram](assets/demo-arch.png)
> Note: In this diagram ArgoCD and the promethius operator is not included.  

## Run the app on Azure Kubernetes Service (AKS)

To learn how to depoy this app on AKS, see [Quickstart: Deploy an Azure Kubernetes Service (AKS) cluster using Azure CLI](https://learn.microsoft.com/azure/aks/learn/quick-kubernetes-deploy-cli).

> Note: The above article shows a simplified version of the store app with some services removed. For the full application, you can use the `aks-store-all-in-one.yaml` file in this repo.

To set up ArgoCD follow this tutorial: [ArgoCD quickstart](https://techcommunity.microsoft.com/t5/apps-on-azure-blog/getting-started-with-gitops-argo-and-azure-kubernetes-service/ba-p/3288595)

To set upp the Promethius operator follow this tutorial[Promethius operator quickstart](https://techcommunity.microsoft.com/t5/apps-on-azure-blog/getting-started-with-gitops-argo-and-azure-kubernetes-service/ba-p/3288595)

## Run the app locally

The application can also be run locally for faster development iterations using Docker Compose. (**This will not set up Promethius operator and ArgoCD**)

> **IMPORTANT**: You must have [Docker Desktop](https://www.docker.com/products/docker-desktop) installed to run this app locally.

To run this app locally:

Clone the repo to your development computer and navigate to the directory:

```console
git clone https://github.com/Azure-Samples/aks-store-demo.git
cd aks-store-demo
```

Start the app using `docker compose`. For example:

```bash
docker compose up
```

To stop the app, you can hit the `CTRL+C` key combination in the terminal window where the app is running.


## Future improvments 
- Add DNS-names for the static ip adresses
- Use spot instances to save on cost when scaling up cluster
- Implement proper logging
- Configure Istio for AB-testing 
- Create Observability with Jeager 
- Consider to use Ingress insted of current azure Loadbalancers

## Additional Resources

- AKS Documentation. https://learn.microsoft.com/azure/aks
- Kubernetes Learning Path. https://azure.microsoft.com/resources/kubernetes-learning-path 
