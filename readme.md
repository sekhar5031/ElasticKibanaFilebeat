Elasticsearch is the distributed search and analytics engine at the heart of the Elastic Stack. Logstash and Beats facilitate collecting, 
aggregating, and enriching your data and storing it in Elasticsearch. Kibana enables you to interactively explore, visualize, and share insights into 
your data and manage and monitor the stack. Elasticsearch is where the indexing, search, and analysis magic happens.
Elasticsearch offers speed and flexibility to handle data in a wide variety of use cases:

Add a search box to an app or website
Store and analyze logs, metrics, and security event data
Use machine learning to automatically model the behavior of your data in real time
Use Elasticsearch as a vector database to create, store, and search vector embeddings
Automate business workflows using Elasticsearch as a storage engine
Manage, integrate, and analyze spatial information using Elasticsearch as a geographic information system (GIS)
Store and process genetic data using Elasticsearch as a bioinformatics research tool


Filebeat is a lightweight shipper for forwarding and centralizing log data. Installed as an agent on your servers, Filebeat monitors the log files or 
locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.

Here’s how Filebeat works: When you start Filebeat, it starts one or more inputs that look in the locations you’ve specified for log data. For each 
log that Filebeat locates, Filebeat starts a harvester. Each harvester reads a single log for new content and sends the new log data to libbeat, 
which aggregates the events and sends the aggregated data to the output that you’ve configured for Filebeat.



Kibana is a data visualization and exploration tool used for log and time-series analytics, application monitoring, and operational intelligence use 
cases. It offers powerful and easy-to-use features such as histograms, line graphs, pie charts, heat maps, and built-in geospatial support.


filebeat.yaml
---
filebeat.inputs:
  - type: docker
    enabled: true
    containers:
      path: /var/lib/docker/containers
      stream: stdout
      ids:
        - "*"
filebeat.config.modules:
  path: ${path.config}/modules.d/*.yml
  reload.enabled: false
setup.template.settings:
  index.number_of_shards: 1
output.elasticsearch:
  hosts:
    - 172.31.35.160:9200
processors:
  - add_host_metadata:
      when.not.contains.tags: forwarded
  - add_cloud_metadata: null
  - add_docker_metadata: null
  - add_kubernetes_metadata: null



kibana.yaml
-----------

server.port: 5601
server.host: "localhost"
elasticsearch.hosts: ["http://localhost:9200"]




elasticsearch.yaml
-------------------
node.name: node-1
path.data: D:\ElasticKibanaFilebeat\ooo\log
path.logs: D:\ElasticKibanaFilebeat\ooo\data
network.host: localhost
http.port: 9200
discovery.type: single-node
