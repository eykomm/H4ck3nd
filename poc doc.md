# POC Documentation

## ELK Stack with Metricbeat for Monitoring

- Pull images

`> docker pull docker.elastic.com/elasticsearch/elasticsearch:6.6.1`

`> docker pull docker.elastic.com/kibana/kibana:6.6.1`

`> docker pull docker.elastic.com/beats/metricbeats:6.6.1`

- Create network

`> docker network create elastic`

- Create Metric Beat yml

```
metricbeat.modules:

#------------------------------- System Module -------------------------------
- module: system
  metricsets: ["cpu", "load", "filesystem", "fsstat", "memory", "network", "process", "core", "diskio", "socket"]
  period: 5s
  enabled: true
  processes: ['.*']

  cpu.metrics:  ["percentages"]
  core.metrics: ["percentages"]

#------------------------------- Docker Module -------------------------------
- module: docker
  metricsets: ["container", "cpu", "diskio", "healthcheck", "info", "memory", "network"]
  hosts: ["unix:///var/run/docker.sock"]
  enabled: true
  period: 5s

#-------------------------- Elasticsearch output ------------------------------
output.elasticsearch:
  #username: "metricbeat_anonymous_user"
  #password: ""
  hosts: ["elasticsearch:9200"]

setup.kibana:
  host: "kibana:5601"

#============================== Dashboards =====================================
# These settings control loading the sample dashboards to the Kibana index. Loading
# the dashboards is disabled by default and can be enabled either by setting the
# options here, or by using the `-setup` CLI flag.
setup.dashboards.enabled: true

logging.level: warning
logging.to_files: true
logging.to_syslog: false
logging.files:
  path: /var/log/metricbeat
  name: metricbeat.log
  keepfiles: 2
  permissions: 0644

```

- Start container

`> docker run --detach --net=elastic --name=elasticsearch -e "discovery.type=single-node" -p 9200:9200 -p 9300:9300 docker.elastic.co/elasticsearch/elasticsearch:6.6.1`

`> docker run --detach --net=elastic --name=kibana -p 5601:5601 docker.elastic.co/kibana/kibana:6.6.1`

`> docker run --detach --net=elastic --name=metric -u root -v /Users/lars/Hackspace/Docker/metricbeat/metricbeat.yml:/usr/share/metricbeat/metricbeat.yml -v /var/run/docker.sock:/var/run/docker.sock docker.elastic.co/beats/metricbeat:6.6.1`

### Add Packetbeat for Network Monitoring

- Pull image

`> docker pull docker.elastic.com/beats/packetbeat:6.6.1`

- Create packetbeat.yml

```

```

- Create Dockerfile:

```
FROM docker.elastic.co/beats/packetbeat:6.6.1
COPY packetbeat.yml /usr/share/packetbeat/packetbeat.yml
USER root
RUN chown root:packetbeat /usr/share/packetbeat/packetbeat.yml
USER packetbeat
```
 
- build custom images

`> docker build -t <image_name> .`

- run container

`> docker run -d --name packet --net=elastic --cap-add=NET_ADMIN <image_name>`
