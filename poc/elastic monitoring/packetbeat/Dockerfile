FROM docker.elastic.co/beats/packetbeat:6.6.1
COPY packetbeat.yml /usr/share/packetbeat/packetbeat.yml
USER root
RUN chown root:packetbeat /usr/share/packetbeat/packetbeat.yml
USER packetbeat
