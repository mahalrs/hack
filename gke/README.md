# GKE Best Practices

## Secure load balancer to application traffic

By default, the traffic between load balancer and backend services is served over HTTP. Encrypt traffic from the load balancer to backend Pods using the ports[].appProtocol field.

Since the load balancer does not verify the certificate used by backend Pods, you need to ensure the certificate used on the backend Pods is valid.

Ref: [Secure a Gateway](https://cloud.google.com/kubernetes-engine/docs/how-to/secure-gateway#load-balancer-tls)

## Create and assign service accounts to deployments

The cluster assigns default service account to pods by default. We should use `kind: ServiceAccount` resource to assign service accounts for each deployment with principle of least privilege.

Infra team should create requested service accounts which can be attached to deployments using `kind: ServiceAccount` resource by application teams.

## Restrict access to GKE Cluster

GitHub Actions shouldn't have access to GKE cluster (secure cluster with authorized networks access enabled). This means GitHub actions can't access the GKE cluster. To deploy resources to GKE cluster, you can use GitHub Actions for CI; build, test, and push artifacts (docker images + helm charts) to Artifact Registry. Then, you can use Cloud Build or ArgoCD/FluxCD to deploy resources from within the VPC.
