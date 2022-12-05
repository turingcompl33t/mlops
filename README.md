## Machine Learning Operations

This repository contains experiments in data infrastructure management and machine learning operations.

### Tools / Technologies

This section describes the tools and technologies of interest.

**Compute Substrate**

A means by which to manage both long-running services and ephemeral tasks implemented via Docker containers.

Candidates:
- [Docker Compose](https://docs.docker.com/compose/)
- [Docker Swarm](https://docs.docker.com/engine/swarm/)
- [Vanilla Upstream K8s](https://kubernetes.io/)
- [Rancher RKE](https://rancher.com/docs/rke/latest/en/)
- [MicroK8s](https://microk8s.io/)

**Compute Substrate Management**

A means by which to programmatically (e.g. HTTP) start new services and tasks implemented via Docker containers.

Candidates:
- ???

**Object Storage**

Data storage for image data.

Requirements:
- Scalable, distributed object storage.

Candidates:
- [SeaweedFS](https://github.com/seaweedfs/seaweedfs)
- [Ceph](https://ceph.io/en/)
- [MinIO](https://min.io/)
- [HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html)

**Relational Database System**

Persistent storage for MLOps metadata.

Candidates:
- [PostgreSQL](https://www.postgresql.org/)
- [MySQL](https://www.mysql.com/)

**Orchestration**

Coordination mechanism among services that comprise the MLOps pipeline.

Candidates:
- [Kafka](https://kafka.apache.org/) (with manually-configured topics for service coordination)
- [RabbitMQ](https://www.rabbitmq.com/) (as a generic message broker)
- [Airflow](https://airflow.apache.org/) (with [`KubernetesPodOperator`](https://airflow.apache.org/docs/apache-airflow-providers-cncf-kubernetes/stable/operators.html))

**Labeling Tool**

Labeling tool for images.

Candidates:
- [CVAT](https://cvat.org/)
- [Label Studio](https://labelstud.io/)

**Image Selection**

Manually / automatically select relevant segments of an "image stream" to cut down the total volume of stored data and focus efforts on relevant data only.

Candidates:
- ???

**Ephemeral Task Management**

Receive requests for ephemeral tasks and subsequently launch them on the compute substrate.

Candidates:
- [Celery](https://docs.celeryq.dev/en/stable/getting-started/introduction.html)
