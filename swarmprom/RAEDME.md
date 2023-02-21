# swarmprom

[Swarmprom](https://github.com/tiangolo/blog-posts/tree/master/docker-swarm-with-swarmprom-for-real-time-monitoring-and-alerts) 
is a starter kit for Docker Swarm monitoring with [Prometheus](https://prometheus.io/),
[Grafana](http://grafana.org/),
[cAdvisor](https://github.com/google/cadvisor),
[Node Exporter](https://github.com/prometheus/node_exporter),
[Alert Manager](https://github.com/prometheus/alertmanager)
and [Unsee](https://github.com/cloudflare/unsee).

## Demo domain
- cpedemo.local

### Requisites

These instructions assume you already have Traefik set up following that guide above, in short:

* With automatic HTTPS certificate generation.
* A Docker Swarm network `traefik-public`.
* Filtering to only serve containers with a label `traefik.constraint-label=traefik-public`.

### Instructions

* Clone this repository and enter into the directory:


* Set and export an `ADMIN_USER` environment variable:

```bash
export ADMIN_USER=admin
```

* Set and export an `ADMIN_PASSWORD` environment variable:


```bash
export ADMIN_PASSWORD=changethis
```

* Set and export a hashed version of the `ADMIN_PASSWORD` using `openssl`, it will be used by Traefik's HTTP Basic Auth for most 
of the services:

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
export DOMAIN=cpedemo.local
```

and make sure that the following sub-domains point to your Docker Swarm cluster IPs:

* `grafana.cpedemo.local`
* `alertmanager.cpedemo.local`
* `unsee.cpedemo.local`
* `prometheus.cpedemo.local`


**Note**: You can also use a subdomain, like `swarmprom.example.com`. Just make sure that the subdomains point to (at least one 
of) your cluster IPs. Or set up a wildcard subdomain (`*`).


```bash
docker stack deploy -c swarmprom.yml swarmprom
```

To test it, go to each URL:

* `https://grafana.cpedemo.local`
* `https://alertmanager.cpedemo.local`
* `https://unsee.cpedemo.local`
* `https://prometheus.cpedemo.local`


## Setup Grafana

Navigate to `http://grafana.cpedemo.local` and login with user ***admin*** password ***admin***.
You can change the credentials in the compose file or
by supplying the `ADMIN_USER` and `ADMIN_PASSWORD` environment variables at stack deploy.

Swarmprom Grafana is preconfigured with two dashboards and Prometheus as the default data source:

* Name: Prometheus
* Type: Prometheus
* Url: http://prometheus:9090
* Access: proxy

After you login, click on the home drop down, in the left upper corner and you'll see the dashboards there.


