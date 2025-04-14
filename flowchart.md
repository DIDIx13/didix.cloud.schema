```mermaid
flowchart TD
    %% Infrastructure provisioning via Terraform + Ansible on OpenStack
    A["Terraform + Ansible (Automation and Provisioning)"]
    B["OpenStack Cloud Infrastructure"]

    %% Non-Kubernetes Services provisioned on OpenStack
    C["Traefik (Reverse Proxy)"]
    D["OpenVPN Server"]
    E["Consul (DNS)"]
    subgraph M [Monitoring stack]
      F1["Grafana (Dashboard)"]
      F2["Prometheus (Metrics)"]
      F3["Loki (Log Aggregation)"]
    end
    G["PostgreSQL (Database)"]

    %% Kubernetes environment for internal & client services
    H["Kubernetes cluster (internal & client Services)"]
    I["Helm (Deployment manager)"]

    %% Relationships
    A --> B
    B --> C
    B --> D
    B --> E
    B --> M
    B --> G
    B --> H
    H --> I

```

## Diagram Explanation

### Infrastructure Provisioning:

- Terraform + Ansible: These tools provision and automate the creation of all resources on OpenStack infra.

- OpenStack Cloud Infrastructure: Serves as the underlying platform where both non-Kubernetes services and the Kubernetes cluster are deployed.

### Non-Kubernetes Services:
These components are provisioned directly on OpenStack via Terraform + Ansible:

- Traefik (Reverse Proxy)

- OpenVPN Server

- Consul (DNS)

- Monitoring Stack (Grafana, Prometheus, Loki)

- PostgreSQL (Database)

### Kubernetes Environment:
Internal services and client applications are deployed within:

- Kubernetes Cluster (Internal & Client Services)

- Helm (Deployment Manager): Used for managing application deployments within the Kubernetes cluster.
