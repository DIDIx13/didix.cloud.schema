```mermaid
flowchart LR
    %% External Zone
    subgraph External [External Zone]
        Internet[Internet]
    end

    %% Public Services Zone
    subgraph Public [Public Services]
        Traefik["Traefik (Reverse Proxy)"]
        OpenVPN["OpenVPN Server"]
    end

    %% Infrastructure Provisioning
    subgraph Infra [Infrastructure]
        Provision["Terraform + Ansible (Automation & Provisioning)"]
        OpenStack["OpenStack Cloud Infrastructure"]
    end

    %% Internal Services Zone
    subgraph Internal [Internal Services]
        Kubernetes["Kubernetes Cluster (Internal & Client Services)"]
        Consul["Consul (DNS & Service Discovery)"]
        Postgres["PostgreSQL (Database)"]

        %% Monitoring Stack with detailed interactions
        subgraph Monitoring [Monitoring Stack]
            Prometheus["Prometheus 
            (Metrics Collector - Pull)"]
            Grafana["Grafana
            (Dashboard - Pull)"]
            Loki["Loki
            (Log Aggregation - Push)"]
        end
    end

    %% Monitored Targets (exposed by Kubernetes applications)
    subgraph Targets [Monitored Targets]
        App1["Application Services
        (Exposing Metrics & Logs)"]
    end

    %% Connectivity between zones
    Internet -->|HTTP/S| Traefik
    Internet -->|VPN| OpenVPN

    %% Infrastructure provisioning and hosting on OpenStack
    Provision --> OpenStack
    OpenStack -->|Hosts| Traefik
    OpenStack -->|Hosts| OpenVPN
    OpenStack -->|Hosts| Consul
    OpenStack -->|Hosts| Postgres
    OpenStack -->|Hosts| Kubernetes

    %% Kubernetes interactions
    Kubernetes -->|Deploys| App1

    %% Monitoring interactions
    Prometheus -- "Scrapes metrics" --> App1
    Prometheus -- "Uses SD" --> Consul
    Grafana -- "Queries" --> Prometheus
    App1 -- "Pushes logs" --> Loki

    %% Consul Service Discovery
    Consul -->|Discovers services| Prometheus
```

## Diagram Explanation

### External Zone:

- Internet represents the external access point where client requests originate.

### Public Services Zone:

- Traefik (Reverse Proxy) manages external HTTP/S traffic routing.

- OpenVPN Server handles secure VPN connections.

### Internal Services Zone:

- Kubernetes Cluster (Internal & Client Services) hosts internal and client-deployed applications.

- Consul (DNS) provides internal DNS and service discovery.

- PostgreSQL (Database) is used by the internal services.

- Monitoring Stack is detailed into three components: Grafana (dashboards), Prometheus (metrics), and Loki (log aggregation).

### Infrastructure Provisioning:

- Terraform + Ansible automate the provisioning of the infrastructure.

- OpenStack Cloud Infrastructure hosts all the resources, including the public services and the Kubernetes cluster.

### Connectivity:

- External traffic flows from the Internet to Traefik (and via VPN to the OpenVPN Server), which then routes requests to the Kubernetes cluster and other services.

- The infrastructure components provisioned via Terraform + Ansible manage all resources on the OpenStack platform.
