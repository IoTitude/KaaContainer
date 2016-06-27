# Dockerized Kaa cluster

Dockerfiles and docker-compose for setting up Kaa cluster.

## Versions

- Kaa 0.9.0
- MariaDB 5.5
- Cassandra 2.2.6
- ZooKeeper 3.4.6

## Setup

At this early point of development you need to configure the containers after 'docker-compose up -d' in the following way:

- set mariadbhost in kaa container: /usr/lib/kaa-node/conf/admin*
- set mariadbhost in kaa container: /usr/lib/kaa-node/conf/sql-dao.properties
- set zookeeperhost in kaa container: /usr/lib/kaa-node/conf/kaa-node.properties
- `service kaa-node restart`
- run 'cqlsh -f /var/lib/cassandra/cassandra.cql' in cassandra container

Cassandra still needs to be configured correctly in order to store measuring data.
