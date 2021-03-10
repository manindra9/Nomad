## Instructions to install Nomad and Deploy easytravel application
The Sock Shop Microservices Demo consists of 5 microservices which provide the customer-facing part of an e-commerce application. These services are written in several languages, including node.js, Java and 1 mongodb databases. The demo is deployable on many platforms including Docker, Kubernetes, Amazon ECS and Nomad.

The instructions below describe how you can deploy the easytravel microservices using [Nomad](https://www.nomadproject.io/).

In our case, all the easytravel microservices will be launched in Docker containers, using Nomad's Docker Driver.  

## Prerequisites
In order to deploy the easytravel demo on Nomad, you would use `git clone https://github.com/dryice-devops/Nomad.git`. In the current setup of Nomad, it requires 3 Nomad servers and 1 clien with Ports (4646,4647 & 4648) open on inbound/outbound).

## Nomad Installation
Follow the below steps to install Nomad on Server mode.

1. export NOMAD_VERSION="1.0.1"
2. curl --silent --remote-name https://releases.hashicorp.com/nomad/${NOMAD_VERSION}/nomad_${NOMAD_VERSION}_linux_amd64.zip
3. unzip nomad_${NOMAD_VERSION}_linux_amd64.zip
4. sudo chown root:root nomad
5. sudo mv nomad /usr/local/bin/
6. nomad version
7. nomad -autocomplete-install
8. complete -C /usr/local/bin/nomad nomad
9. sudo mkdir --parents /opt/nomad
10. sudo touch /etc/systemd/system/nomad.service
11. sudo mkdir --parents /etc/nomad.d
12. sudo chmod 700 /etc/nomad.d
13. sudo touch /etc/nomad.d/nomad.hcl
14. sudo touch /etc/nomad.d/server.hcl
15. sudo touch /etc/nomad.d/client.hcl
