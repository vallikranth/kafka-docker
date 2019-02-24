FROM openjdk:11-jre-slim

ARG kafka_version=2.1.1
ARG scala_version=2.12

ENV KAFKA_VERSION=$kafka_version \
    SCALA_VERSION=$scala_version \
    KAFKA_HOME=/opt/kafka \
    GLIBC_VERSION=$glibc_version

ENV PATH=${PATH}:${KAFKA_HOME}/bin

COPY download-kafka.sh start-kafka.sh start-connect-distributed.sh /tmp/

RUN apt-get update && apt-get install -y bash curl jq wget procps\
 && chmod a+x /tmp/*.sh \
 && mv /tmp/start-kafka.sh /tmp/start-connect-distributed.sh /usr/bin \
 && sync && /tmp/download-kafka.sh \
 && tar xfz /tmp/kafka_${SCALA_VERSION}-${KAFKA_VERSION}.tgz -C /opt \
 && rm /tmp/kafka_${SCALA_VERSION}-${KAFKA_VERSION}.tgz \
 && ln -s /opt/kafka_${SCALA_VERSION}-${KAFKA_VERSION} /opt/kafka \
 && rm -rf /tmp/* /var/lib/apt/lists/*
 
VOLUME ["/kafka"]

ENV PATH "$PATH:$KAFKA_HOME/bin"

# Use "exec" form so that it runs as PID 1 (useful for graceful shutdown)
CMD ["start-kafka.sh"]
