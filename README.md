# Pulsar Viewer

[![Mayhem for API](https://mayhem4api.forallsecure.com/api/v1/api-target/mayhemheroes/pulsar-viewer/badge/icon.svg?scm_branch=main)](https://mayhem4api.forallsecure.com/mayhemheroes/pulsar-viewer/latest-job?scm_branch=main)

This application provides simple REST api to see messages stored in Apache Pulsar topics. There is also simple html page to call that REST api.

REST API documentation is available [openapi.yml](openapi.yml) file.

## Installation

For simple start up you can use Docker image [https://hub.docker.com/repository/docker/osomahe/pulsar-viewer](https://hub.docker.com/repository/docker/osomahe/pulsar-viewer).

Release notes can be found at [releases.md](releases.md).

Environment variables:

* **PULSAR_SERVICE_URL** - default "pulsar://localhost:6650" url to connect to Apache Pulsar instance
* **PULSAR_TLS_TRUST_CERT** - not set by default, used for transport encryption using tLS certificate e.g. `/pulsar/certs/ca.cert.pem`
* **PULSAR_TLS_CERT_FILE** - not set by default, path for client certificate for TLS authorization `/pulsar/certs/pulsar-source-app.cert.pem`
* **PULSAR_TLS_KEY_FILE** - not set by default, path for client key to certificate for TLS authorization `/pulsar/certs/pulsar-source-app.key-pk8.pem`
* PULSAR_ADMIN_SERVICE_HTTP_URL - default "http://localhost:8080" url for admin to connect to `https://pulsarhostname:8443`
* PULSAR_ADMIN_TLS_CERT_FILE - not set by default, path for admin certificate for TLS authorization `/pulsar/certs/admin.cert.pem`
* PULSAR_ADMIN_TLS_KEY_FILE - not set by default, path for admin key to certificate for TLS authorization `/pulsar/certs/admin.key-pk8.pem`
* PULSAR_DEFAULT_READER - default "pulsar-viewer" name used for reader name
* PULSAR_HEALTH_TOPIC - default "non-persistent://public/default/health-check" topic used for health checking of readiness probe
* PULSAR_MAX_MESSAGES - Maximum number of messages read from pulsar to handle single request
* PULSAR_DEFAULT_MAX_MESSAGE_AGE_SECONDS - When "from" is not specified. Reader will look for messages no older than by default 3600s

Examples:
```bash
docker run -d --name pulsar-viewer -p 8080:8080 -e PULSAR_SERVICE_URL="pulsar://pulsarhostname:6650" ghcr.io/osomahe/pulsar-viewer
```

### Health checks

* Liveness probe - `/q/health/live`
* Readiness probe - `/q/health/ready`

## Maintainers

This project was developed with support of companies [HP Tronic](http://www.hptronic.cz/) and [Osomahe](https://www.osomahe.com/).


## Development

Build Docker image

Start Apache Pulsar instance.
```bash
docker run -d --rm --name pulsar --net hpt -p 6650:6650 \
-v $(pwd)/development/pulsar/conf:/pulsar/conf:ro \
apachepulsar/pulsar:2.8.0 bin/pulsar standalone -nfw
```

Start Pulsar Source App to create some test messages.
```bash
docker run -d --rm --name source-app --net hpt -p 8082:8080 \
-e PULSAR_SERVICE_URL=pulsar://pulsar:6650 osomahe/pulsar-source-app:0.4.0
```
