# Dockerized Kaa cluster

Dockerfiles and docker-compose for setting up Kaa cluster.

## Versions

- Kaa 0.9.0
- MariaDB 5.5
- Cassandra 2.2.6
- ZooKeeper 3.4.6

## Setup

At this early point of development you need to configure the containers after 'docker-compose up -d' in the following way:

- `service kaa-node restart`
- `docker exec kaacontainer_cassandra_1 cqlsh -f /var/lib/cassandra/cassandra.cql`

Cassandra still needs to be configured correctly in order to store measuring data.
