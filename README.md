# fanapcampus-elastikibana
This document describes how to deploy a single-node Elastic Stack (Elasticsearch and Kibana) using Docker Compose.

**Requirements:**

* Docker installed and running

## Getting Started:

1. Clone this project.
2. Start the Elastic Stack:
    ``` bash
    docker compose up -d
    ``` 
3. Access Kibana UI:
    * Open http://localhost:5601 in your web browser.


## Description

### Services:

**elasticsearch**

* **Image:** elasticsearch:8.0.1
* **Container Name:** elasticsearch
* **Environment Variables:**
    * `discovery.type=single-node`: Disables cluster discovery for a single-node deployment.
    * `xpack.security.enabled=false`: Disables Xpack security for development purposes.
* **Ports:**
    * `9200:9200`: Maps the container port 9200 (Elasticsearch) to the host port 9200.
* **Healthcheck:**
    * This configuration defines a health check for the Elasticsearch container.
    * The check uses `curl` to access Elasticsearch on port 9200.
    * It retries the check 5 times with a 30-second interval and a 10-second timeout if it fails initially.
* **Volumes:**
    * `elasticsearch-data:/usr/share/elasticsearch/data`: Mounts a local volume named `elasticsearch-data` to the container's `/usr/share/elasticsearch/data` directory. This persists Elasticsearch data.
* **Networks:**
    * `elastic`: Connects the container to the `elastic` network.
* **Resources:**
    * `limits.memory: 1g`: Sets a memory limit of 1 gigabyte for the Elasticsearch container.

**kibana**

* **Image:** kibana:8.0.1
* **Container Name:** kibana
* **Depends On:**
    * `elasticsearch`: Kibana depends on Elasticsearch being available before starting.
* **Volumes:**
    * `kibana-data:/usr/share/kibana/data`: Mounts a local volume named `kibana-data` to the container's `/usr/share/kibana/data` directory. This persists Kibana data.
* **Ports:**
    * `5601:5601`: Maps the container port 5601 (Kibana) to the host port 5601.
* **Networks:**
    * `elastic`: Connects the container to the `elastic` network.

### Volumes:

**elasticsearch-data**

* **Driver:** local
* This volume persists Elasticsearch data on the host machine.

**kibana-data**

* **Driver:** local
* This volume persists Kibana data on the host machine.

### Networks:

**elastic**

* **Driver:** bridge
* **IPAM:**
    * This configures a bridge network named `elastic` with a subnet of 172.28.0.0/16. This allows the containers to communicate with each other.

## Important Notes:

* This configuration is for a development environment and disables security features.
* Consider using environment variables for sensitive information in production.
* Refer to the official Elasticsearch and Kibana documentation for detailed configuration options.
