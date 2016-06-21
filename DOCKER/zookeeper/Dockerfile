FROM java:8

RUN apt-get update && apt-get install -y \
  net-tools wget
RUN mkdir -p /usr/src/
WORKDIR /usr/src
RUN wget http://mirror.netinch.com/pub/apache/zookeeper/zookeeper-3.4.6/zookeeper-3.4.6.tar.gz
RUN tar -xzvf zookeeper-3.4.6.tar.gz
CMD ["./zookeeper-3.4.6/bin/zkServer.sh", "start"]
