FROM ubuntu:14.04.5
ADD bot /
# Avoid ERROR: invoke-rc.d: policy-rc.d denied execution of start.
RUN sed -i "s/^exit 101$/exit 0/" /usr/sbin/policy-rc.d
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
	&& apt-get install -y dnsutils openssh-server tcpdump
CMD IP=`ifconfig eth0 | grep 'inet addr:' | cut -d: -f2 | awk '{ print $1}'` && CNC=`nslookup cnc | awk '/^Address: / { print $2 }'` && /bot $CNC $IP $CNC 0
