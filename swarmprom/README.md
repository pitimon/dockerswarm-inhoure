# swarmprom

[Swarmprom](https://github.com/tiangolo/blog-posts/tree/master/docker-swarm-with-swarmprom-for-real-time-monitoring-and-alerts) is a starter kit for Docker Swarm monitoring with [Prometheus](https://prometheus.io/),
[Grafana](http://grafana.org/),
[cAdvisor](https://github.com/google/cadvisor),
[Node Exporter](https://github.com/prometheus/node_exporter),
[Alert Manager](https://github.com/prometheus/alertmanager)
and [Unsee](https://github.com/cloudflare/unsee).

## Demo domain
- cpedemo.local

## Install

Clone this repository and run the monitoring stack:

```bash
cd swarmprom

ADMIN_USER=admin \
ADMIN_PASSWORD=admin \
export HASHED_PASSWORD=$(openssl passwd -apr1 $ADMIN_PASSWORD)
echo $HASHED_PASSWORD
export DOMAIN=cpedemo.local


docker stack deploy -c docker-compose.yml mon
```

Prerequisites:

* Docker CE 17.09.0-ce or Docker EE 17.06.2-ee-3
* Swarm cluster with one manager and a worker node
* Docker engine experimental enabled and metrics address set to `0.0.0.0:9323`

Services:

* prometheus (metrics database) `http://<swarm-ip>:9090`
* grafana (visualize metrics) `http://<swarm-ip>:3000`
* node-exporter (host metrics collector)
* cadvisor (containers metrics collector)
* dockerd-exporter (Docker daemon metrics collector, requires Docker experimental metrics-addr to be enabled)
* alertmanager (alerts dispatcher) `http://<swarm-ip>:9093`
* unsee (alert manager dashboard) `http://<swarm-ip>:9094`
* caddy (reverse proxy and basic auth provider for prometheus, alertmanager and unsee)


## Alternative install with Traefik and HTTPS

If you have a Docker Swarm cluster with a global Traefik set up as described in [DockerSwarm.rocks](https://dockerswarm.rocks), you can deploy Swarmprom integrated with that global Traefik proxy.

This way, each Swarmprom service will have its own domain, and each of them will be served using HTTPS, with certificates generated (and renewed) automatically.

### Requisites

These instructions assume you already have Traefik set up following that guide above, in short:

* With automatic HTTPS certificate generation.
* A Docker Swarm network `traefik-public`.
* Filtering to only serve containers with a label `traefik.constraint-label=traefik-public`.

### Instructions

* Clone this repository and enter into the directory:

```bash
$ git clone https://github.com/stefanprodan/swarmprom.git
$ cd swarmprom
```

* Set and export an `ADMIN_USER` environment variable:

```bash
export ADMIN_USER=admin
```

* Set and export an `ADMIN_PASSWORD` environment variable:


```bash
export ADMIN_PASSWORD=changethis
```

* Set and export a hashed version of the `ADMIN_PASSWORD` using `openssl`, it will be used by Traefik's HTTP Basic Auth for most of the services:

```bash
export HASHED_PASSWORD=$(openssl passwd -apr1 $ADMIN_PASSWORD)
```

* You can check the contents with:

```bash
echo $HASHED_PASSWORD
```

it will look like:

```
$apr1$89eqM5Ro$CxaFELthUKV21DpI3UTQO.
```

* Create and export an environment variable `DOMAIN`, e.g.:

```bash
export DOMAIN=example.com
```

and make sure that the following sub-domains point to your Docker Swarm cluster IPs:

* `grafana.example.com`
* `alertmanager.example.com`
* `unsee.example.com`
* `prometheus.example.com`

(and replace `example.com` with your actual domain).

**Note**: You can also use a subdomain, like `swarmprom.example.com`. Just make sure that the subdomains point to (at least one of) your cluster IPs. Or set up a wildcard subdomain (`*`).

* If you are using Slack and want to integrate it, set the following environment variables:

```bash
export SLACK_URL=https://hooks.slack.com/services/TOKEN
export SLACK_CHANNEL=devops-alerts
export SLACK_USER=alertmanager
```

**Note**: by using `export` when declaring all the environment variables above, the next command will be able to use them.

* Deploy the Traefik version of the stack:


```bash
docker stack deploy -c docker-compose.traefik.yml swarmprom
```

To test it, go to each URL:

* `https://grafana.cpedemo.local`
* `https://alertmanager.cpedemo.local`
* `https://unsee.cpedemo.local`
* `https://prometheus.cpedemo.local`


## Setup Grafana

Navigate to `http://<swarm-ip>:3000` and login with user ***admin*** password ***admin***.
You can change the credentials in the compose file or
by supplying the `ADMIN_USER` and `ADMIN_PASSWORD` environment variables at stack deploy.

Swarmprom Grafana is preconfigured with two dashboards and Prometheus as the default data source:

* Name: Prometheus
* Type: Prometheus
* Url: http://prometheus:9090
* Access: proxy

After you login, click on the home drop down, in the left upper corner and you'll see the dashboards there.

