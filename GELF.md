## GELF 

Graylog Extended Log format 

- Avoids shortcomings of plain syslog: 
    - Limited to 1024 byte length - not enough for large payloads like backtraces 
    - Data types not specified in structured syslog (e.g. number versus string)
    - RFCs are strict (request for comments?), but too many syslog dialects exist to parse all of them 
    - No compression 
- GELF can be sent via UDP or TCP 
    - Chunking 
- Compression 
    - Tradeoff between CPU load time and network bandwidth 
    - GZIP protocol by default 



#### Sources: 
- [Graylog docs](http://docs.graylog.org/en/2.0/pages/gelf.html)