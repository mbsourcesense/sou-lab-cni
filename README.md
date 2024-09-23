# Vagrant Monitoring Project with Prometheus, Grafana, and HAProxy

Hi, here's a brief readme to introduce you to my project:

This project sets up a monitoring environment using **Vagrant** with two Virtual Machines. The environment uses **Prometheus** and **Grafana** for monitoring and visualization, and **HAProxy** as a reverse proxy to expose these services.

## Project Overview

The project provisions two VMs using Vagrant(see more details - Vagrantfile):
- **VM 1 (`soube2`)**: This VM hosts **Prometheus** and **Grafana**, forming the backend of the monitoring setup.
- **VM 2 (`soufe1`)**: This VM hosts **HAProxy**, functioning as a reverse proxy for both Prometheus and Grafana.

### Brief Services Description
1. **Prometheus**: A monitoring and alerting toolkit.
2. **Grafana**: A web-based analytics visualization tool to view Prometheus metrics with dashboards.
3. **HAProxy**: In this case a reverse proxy used to routes traffic to the appropriate backend services, Prometheus and Grafana in our case.

## Key Features

### Ansible Role - sou_podman
The setup is automated using **sou_podman**, an ansible role that manages the installation and configuration of Podman, Prometheus, Grafana, and HAProxy. 
The role executes the following tasks:
- Installing **Podman** (used to manage containers on both VMs).
- Configuring **Prometheus** and **Grafana** on the backend VM, soube2.
- Setting up **HAProxy** to route traffic on the proxy VM, soufe1.
(/sou-lab-cni/sou_podman/tasks for details about tasks)

### Templates 
- **Prometheus**: The configuration file `prometheus.yml` is generated using a Jinja2 template, they all are, allowing easy modifications. 
- **Grafana**: Configuration such as `grafana.ini` and data source setup are managed through templates, simplifying the deployment.(Inserted prometheus_datasource.yml to have one example of data source)
- **HAProxy**: The `haproxy.cfg` file is also generated using a template, ensuring proper routing for Prometheus and Grafana. It could work as a load balancer too, but here we just have one vm for frontend and one for backend. An improvement could be using certificates. #todo.

## How to test this

### Prerequisites
- **Vagrant and VirtualBox**: Make sure you have these installed to manage the VMs.
- **Ansible**: The provisioning playbooks are executed using Ansible. Be sure to have it installed.

###HTTPS Available with SSL certificate

This project requires SSL certificates for HTTPS configuration. You can generate self-signed certificates using the following commands:

# Generate a self-signed certificate and key
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout haproxy.key -out haproxy.crt

# Combine the certificate and key into a single .pem file
cat haproxy.crt haproxy.key > haproxy.pem

Once generated, place the haproxy.pem file in the appropriate location, in sou_podman/files/haproxy.pem

### Running the Project

1. Clone this repository https://github.com/mbsourcesense/sou-lab-cni.git.
2. Run `vagrant up` to spin up the two VMs.
3. After the provisioning is complete, you can access the services via reverse proxy at:

        Grafana (HTTP):        http://192.168.57.22:8080/grafana
        Grafana (HTTPS):       https://192.168.57.22/grafana

        Prometheus (HTTP):     http://192.168.57.22:8080/prometheus
        Prometheus (HTTPS):    https://192.168.57.22/prometheus

        HAProxy Stats (HTTP):  http://192.168.57.22:8404/haproxy?stats
        HAProxy Stats (HTTPS): https://192.168.57.22:9443/haproxy?stats

Thank you for reading this, 
Manuel ;)
