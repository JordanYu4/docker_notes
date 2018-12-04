## `ELK` stack 

`ELK` stack uses `Logstash` for processing log entries, `ElasticSearch` for storage, and `Kibana` for visualizations 

- Rails app must output log entries in logstash-compatible format. Add support for logstash by adding `logstash-event` gem and configure: 
```Ruby 
config.lograge.formatter = 
Lograge::Formatters::Logstash.new
```
- ELK stack setup, `docker-compose` config: 
```Ruby
elasticsearch: 
    image: elasticserach:2.0 
    command: elasticsearch -Des.network.host=0.0.0.0

logstash: 
    image: logstash:2.0
    command: logstash -f /etc/logstash/conf.d/logstash.conf
    ports: 
        - "12201:12201/udp"
    volumes: 
        - "./logstash:/etc/logstash/conf.d"
    links: 
        - elasticsearch
    
kibana:
    image: kibana:4.2
    links: 
        - elasticsearch
    ports:
        - "5601:5601"
```  
- Mount a `logstash` config file, instructing logstash to receive entries via Graylog Extended Log Format, parse JSON-formatted log entries to extract metadata, and store in ElasticSearch host: 
```Ruby 
input {
    gelf {}
} 

filter {
    json {
        source => "short_message" 
    }
}

output {
    elasticsearch {
        hosts => "elasticsearch:9200"
    }
}
```
- Instruct docker containers to use logging drivers: 
```Rubuy
log_driver: gelf
log_opt: 
    gelf-address: udp://IP_TO_LOGSTASH_HOST:12201
```

- Options can also be specified as defaults in `DOCKER_OPTS` for docker dameon, thus applying to all containers 

#### Sources: 
- [Logging for Rails apps in Docker - Santiago Palladino](https://manas.tech/blog/2015/12/15/logging-for-rails-apps-in-docker.html)