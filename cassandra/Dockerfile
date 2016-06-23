FROM cassandra:2.2.6
# Install Java Native Access
RUN apt-get update && apt-get install -y \
  libjna-java

# Setup database for Kaa
COPY ./files/cassandra.cql /var/lib/cassandra
# How to run after cassandra is running?
# RUN cqlsh -f /var/lib/cassandra/cassandra.cql
