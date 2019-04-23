# DNS-over-TLS-proxy

## Background
Nowadays, some providers (such as Cloudflare) provide a DNS-over-TLS feature that could let us enhance privacy by encrypting our DNS queries.
Our applications don't handle DNS-over-TLS by default. Your task is to design and create a simple DNS to DNS-over-TLS proxy that we could use to enable our application to query a DNS-over-TLS server.

## Implementation
The script dns-over-tls-proxy.py uses the socket and ssl modules to achieve the given task. 
It creates a socket and binds it on port 12853. It can receive a TCP DNS request on this port and forward it to any DNS provider that supports DNS over TLS.

### Docker image and container
To build a docker container just run the following command

  - docker build -t dns-proxy .
  
To run the docker container 

  - docker run -d --name dns-proxy -e DNS_SERVER=1.1.1.1 -e DNS_PORT=853 -p 12853:12853 dns-proxy

To test the setup

  - dig @127.0.0.1 -p 12853 +tcp fefe.de

** The environment variables DNS_SERVER and DNS_PORT are mandatory.

### Deployment

The docker container is portable and can be deployed as a replicated service in k8s, swarm etc.

## Improvements

The following improvements can be done to this service:

* The docker image is quite big at ~900 MB. It can reduced by using a python alpine base image.
* Handles a single request at a time. 
