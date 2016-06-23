FROM ubuntu:14.04

#RUN apt-get update && apt-get install -y \
#  software-properties-common
#RUN add-apt-repository ppa:webupd8team/java
#RUN apt-get update && apt-get install -y \
#  oracle-java8-installer
#RUN mkdir -p /usr/src
#WORKDIR /usr/src
#COPY files/ .
#RUN tar -zxvf jdk-8*.tar.gz
#RUN echo "JAVA_HOME=/usr/src/jdk1.8.0_92" >> ~/.bashrc
#RUN echo "export PATH=$JAVA_HOME/bin:$PATH" >> ~/.bashrc
#RUN export JAVA_HOME=/usr/src/jdk1.8.0_92 && export PATH=$JAVA_HOME/bin:$PATH && echo "$JAVA_HOME" && java -version
#RUN ./jdk1.8.0_92/bin/java -version
#ENV JAVA_HOME="/usr/src/jdk1.8.0_92"
#RUN echo $JAVA_HOME

RUN apt-get update && apt-get install -y \
  software-properties-common nano

# Install Java.
RUN \
  echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
  add-apt-repository -y ppa:webupd8team/java && \
  apt-get update && \
  apt-get install -y oracle-java8-installer && \
  rm -rf /var/lib/apt/lists/* && \
  rm -rf /var/cache/oracle-jdk8-installer

RUN java -version
RUN apt-get update && apt-get install -y \
  wget ca-certificates curl
RUN wget https://github.com/kaaproject/kaa/releases/download/v0.9.0/kaa-deb-0.9.0.tar.gz \
&& tar -xvf kaa-deb-*.tar.gz && dpkg -i deb/kaa-node.deb \
&& sed -i '3s/.*/nosql_db_provider_name=cassandra/' /etc/kaa-node/conf/nosql-dao.properties
#RUN sed -i '39s/.*/jdbc_url=jdbc:mysql:failover://mariadb:3306/kaa/' /etc/kaa-node/conf/admin-dao.properties

EXPOSE 22 8080 9888 9889 9997 9999 465
#transport_public_interface=localhost=YOUR_PUBLIC_INTERFACE

RUN service kaa-node start
RUN /bin/bash

#&& cqlsh -f /etc/kaa-node/conf/cassandra.cql
