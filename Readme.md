# Nifi Prometheus Reporter [![Build Status](https://travis-ci.org/mkjoerg/nifi-prometheus-reporter.svg?branch=master)](https://travis-ci.org/mkjoerg/nifi-prometheus-reporter)

A reporting task in Nifi which is capable of sending monitoring statistics as 
prometheus metrics to a prometheus pushgateway. After this, the Prometheus
server scrapes the metrics from the pushgateway. 
## Modified Points from forked branch
This version collects metrics from SubProcessor Group, Processor, Processor Connections too. Orginal version only collects root level Processor Group metrics.

 
## Getting Started

For setting up the requirements there is a docker-compose file in docker/prometheus, that sets up the Pushgateway, the Prometheus server and a Grafana server.
After starting the docker containers nifi needs to be downloaded and the ReportingTask has to be copied into the lib directory.

To setup the test environment have docker-compose and docker installed: [see link](https://docs.docker.com/compose/install/)
```sh
docker-compose up -d
```
This will bootstrap: 
* A Prometheus server that runs under: http://localhost:9090
* A Pushgateway that runs under: http://localhost:9091
* A Grafana instance that runs under: http://localhost:3000
* A Nifi instance, containing the reporting task under: http://localhost:8080/nifi

A sample dashboard can be found here: [Sample Dashboard](https://grafana.com/dashboards/3294)

After setting up a simple flow and the ReportingTask, the flow can be started and the results should be visible in the Grafana dashboard.

## Docs

See the docs for more details:

1. [Configuration](docs/Configuration.md) 

### Prerequisites

To test or use the PrometheusReportingTask the following systems should be 
setup and running.
* Running Prometheus instance
* Running Prometheus Pushgateway instance
* Running Nifi instance

The tools can be setup with Docker or manually.

### Install to running Nifi instance
First download the [current release](https://github.com/mkjoerg/nifi-prometheus-reporter/releases) and then
copy the nar file into your Nifi lib folder. (Most times under __/opt/nifi/<nifi-version>/lib__)

After this, just restart Nifi.

### Limitations
The Reporting Task can't send custom metrics from processors to the Pushgateway. If you want 
something like this, you have to setup your own processor, that can read FlowFiles, generate custom metrics
and send them to a Pushgateway. Because this is such a custom thing, it can't be done with this Reporting Task
and it is also not the scope of this project.

### Build it yourself

The project can be build with maven as the standard fashion of building 
nifi-processor-bundles. Following snippet shows the entire setup with pre-installed Nifi:



```sh
# Clone project
git clone https://github.com/mkjoerg/nifi-prometheus-reporter.git
# Move into cloned dir
cd nifi-prometheus-reporter

# Build project
mvn clean install
```
The previously built .nar archive has to be copied into the nifi/lib directory 
and can be used after a restart of nifi.
```sh
# Copy .nar into Nifi's lib folder
cp nifi-prometheus-nar/target/nifi-prometheus-nar-1.9.2.nar NIFI_HOME/lib/nifi-prometheus-nar-1.9.2.nar

# Start nifi
NIFI_HOME/bin/nifi.sh start
# Or restart if already running
NIFI_HOME/bin/nifi.sh restart


## Authors

* **Matthias Jörg** - *Initial work* - [mkjoerg](https://github.com/mkjoerg)
* **Daniel Seifert** - *Initial work* - [Daniel-Seifert](https://github.com/Daniel-Seifert)
