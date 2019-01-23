# ExperimentArtifacts

This repository consists of all artifacts, which are needed to conduct the corresponding experiment of the Master's thesis 

## Prerequisites
In order to conduct the experiment, you need to have the following things installed in your environment:
- Java 1.8
- docker, docker-compose, docker-swarm
- Two physically separated servers, in order to avoid any interferences

## Setup
- The current repository has to be cloned on both machines
- One machine has to be declared as the experiment-orchestrator: At this server, [ContinuITy](https://github.com/ContinuITy-Project/ContinuITy) and the monitoring stack (CMR/ Prometheus) is running.
- On the second machine, the system under test is running.
- Initialize the swarm cluster by defining the experiment-orchestrator as swarm master:  `docker swarm init`
- Add the SUT server to the swarm cluster: `docker swarm join --token {YOUR CLUSTER TOKEN} {ORCHESTRATOR_SERVER_IP}:2377`
- Set the environment variables $ORCHESTRATOR_SERVER_HOSTNAME and $SUT_SERVER_HOSTNAME to the corresponding hostnames
- Start ContinuITy: `docker stack deploy --compose-file docker-compose.continuity.yml continuity`
- Start Monitoring: `docker stack deploy --compose-file docker-compose.monitoring.yml monitoring`
- Start Sock-Shop: `docker stack deploy --compose-file docker-compose.sock-shop.yml sock-shop`
- Once all services are available, please move to the directory [/experiment-conductior](/experiment-conductor)
- In [test-executions.csv](/experiment-conductor/test-executions.csv), you can define your test cases: each row defines one test case. The first item defines the modularization approach, the following items define the microservices under test.
- In [config.properties](/experiment-conductor/config.properties), please replace the IPs with yours and configure the experiment.
- Once the configuration is finished, you can start the experiment by executing: `java -jar Experiment-Conductor.java`


