# Probably from this file: https://github.com/Cisco-Talos/clamav/blob/main/Dockerfile
FROM clamav/clamav:0.105.1-7

COPY clamav.conf /tmp/
RUN cat /tmp/clamav.conf >> /etc/clamav/clamd.conf
