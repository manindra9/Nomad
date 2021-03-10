## Instructions to install Nomad and Deploy easytravel application
The Sock Shop Microservices Demo consists of 5 microservices which provide the customer-facing part of an e-commerce application. These services are written in several languages, including node.js, Java and 1 mongodb databases. The demo is deployable on many platforms including Docker, Kubernetes, Amazon ECS and Nomad.

The instructions below describe how you can deploy the easytravel microservices using [Nomad](https://www.nomadproject.io/).

In our case, all the easytravel microservices will be launched in Docker containers, using Nomad's Docker Driver.  

## Prerequisites
In order to deploy the easytravel demo on Nomad, you would use `git clone https://github.com/dryice-devops/Nomad.git`. In the current setup of Nomad, it requires 3 Nomad servers and 1 clien with Ports (4646,4647 & 4648) open on inbound/outbound).

## Nomad Installation
Follow the below steps to install Nomad on Server/Client mode.

1. export NOMAD_VERSION="1.0.1"
2. curl --silent --remote-name https://releases.hashicorp.com/nomad/${NOMAD_VERSION}/nomad_${NOMAD_VERSION}_linux_amd64.zip
3. unzip nomad_${NOMAD_VERSION}_linux_amd64.zip
4. sudo chown root:root nomad
5. sudo mv nomad /usr/local/bin/
6. nomad version
7. nomad -autocomplete-install
8. complete -C /usr/local/bin/nomad nomad
9. sudo mkdir --parents /opt/nomad
10. sudo touch /etc/systemd/system/nomad.service ( paste the content of nomad.service from repo)
11. sudo mkdir --parents /etc/nomad.d
12. sudo chmod 700 /etc/nomad.d
13. sudo touch /etc/nomad.d/nomad.hcl   ( paste the content of nomad.hcl from repo)
14. sudo touch /etc/nomad.d/server.hcl  ( paste the content of server.hcl from repo)
15. sudo touch /etc/nomad.d/client.hcl  ( paste the content of client.hcl from repo)
16. sudo systemctl enable nomad
17. sudo systemctl start nomad
18. sudo systemctl status nomad

by now, we will see the nomad service is up and running on both server & client nodes.

# Nomad Server Join to a Cluster
Update the server.hcl with below configuration on Nomad server nodes and restart the nomad service.
server {  
	server_join {    
		retry_join = [ "<ip of Nomad Server>:4648"]    
		retry_interval = "5s"  
	}  
        enabled = true
}
Or alternatively use the below command t o join Nomad server to a Cluster.
nomad server join <ip>:4648

# Nomad Cient Join to a Cluster
Update the client.hcl with below configuration on Nomad server nodes and restart the nomad service.
client {
  enabled = true
  servers = ["<IP>:4647"]
}
Or alternatively use the below command t o join Nomad server to a Cluster.
nomad node config -update-servers <IP>:4647
  
# Validate the Nomad Setup

Run the below command to see the status of Nomad Servers:
`nomad server members`
and the output should look like below
ame                                Address      Port  Status  Leader  Protocol  Build  Datacenter  Region
ip-10-5-12-22.ec2.internal.global   10.5.12.22   4648  alive   false   2         1.0.1  dc1         global
ip-10-5-12-43.ec2.internal.global   10.5.12.43   4648  alive   false   2         1.0.1  dc1         global
ip-10-5-13-127.ec2.internal.global  10.5.13.127  4648  alive   true    2         1.0.1  dc1         global
