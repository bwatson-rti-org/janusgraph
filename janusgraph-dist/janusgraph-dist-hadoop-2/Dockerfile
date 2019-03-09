FROM openjdk:8

ARG server_zip
ADD ${server_zip} /var
ADD ./jmxremote.password /etc/cassandra/jmxremote.password
ADD ./jmxremote.password /etc/java-8-openjdk/management/jmxremote.password

RUN apt-get update && apt-get install -y curl && \
	apt-get update -y && apt-get install -y zip && \
	server_base=`basename ${server_zip} .zip` && \
	unzip -q /var/${server_base}.zip -d /var && \
	ln -s /var/${server_base} /var/janusgraph && \
	chmod -R 777 /var/${server_base}/bin && chmod -R 777 /var/${server_base}/elasticsearch && \
	chmod -R 777 /var/${server_base}/conf/cassandra && chmod -R 777 /var/${server_base}/elasticsearch/bin && \
	chmod 400 /etc/cassandra/jmxremote.password && \
	groupadd janus && useradd -m -g janus janus -p janusg123 && \
	chown -R janus:janus /var/${server_base}/elasticsearch/bin && \	
	chown -R janus:janus /var/${server_base}/conf/cassandra && \
	chown -R janus:janus /etc && \	
	chown janus:janus /etc/cassandra/jmxremote.password && \
	chown -R janus:janus /var
	
WORKDIR /var/janusgraph

ENTRYPOINT ["./bin/janusgraph.sh", "-v", "start"]

VOLUME ["/var/janusgraph-dist-hadoop-2/data"] 

USER janus

EXPOSE 7000/tcp 7001/tcp 7199/tcp 8182/tcp 9042/tcp 9160/tcp 9200/tcp 18045/tcp 