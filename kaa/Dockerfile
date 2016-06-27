FROM ubuntu:14.04

# Install add-apt-repository from software-properties-common and other packages
RUN apt-get update && apt-get install -y \
  ca-certificates \
  curl \
  software-properties-common \
  wget

# Install Java.
RUN \
  echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
  add-apt-repository -y ppa:webupd8team/java && \
  apt-get update && \
  apt-get install -y oracle-java8-installer && \
  rm -rf /var/lib/apt/lists/* && \
  rm -rf /var/cache/oracle-jdk8-installer

RUN wget https://github.com/kaaproject/kaa/releases/download/v0.9.0/kaa-deb-0.9.0.tar.gz
RUN tar -xvf kaa-deb-*.tar.gz
RUN dpkg -i deb/kaa-node.deb

# Set nosql db to cassandra
RUN sed -i '3s/.*/nosql_db_provider_name=cassandra/' /etc/kaa-node/conf/nosql-dao.properties
# Set mariadb host
RUN sed -i "s/\(jdbc_url *= *\).*/\1jdbc\:mysql\:failover\:\/\/mariadb\:3306\/kaa/" /usr/lib/kaa-node/conf/admin-dao.properties
RUN sed -i "s/\(jdbc_host_port *= *\).*/\1mariadb\:3306/" /usr/lib/kaa-node/conf/sql-dao.properties
# Set zookeeper host
RUN sed -i "s/\(zk_host_port_list *= *\).*/\1zookeeper\:2181/" /usr/lib/kaa-node/conf/kaa-node.properties

EXPOSE 22 8080 9888 9889 9997 9999

RUN service kaa-node start
RUN /bin/bash
