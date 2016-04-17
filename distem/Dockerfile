FROM debian:jessie
MAINTAINER Cristian Ruiz <cristian.ruiz@inria.fr>

RUN apt-get update && apt-get install -y openssh-server
RUN apt-get update && apt-get install -y libnet-ifconfig-wrapper-perl kmod vim wget
# Adding Distem
RUN echo "deb http://distem.gforge.inria.fr/deb ./ \
         \n deb-src http://distem.gforge.inria.fr/deb ./"  >> /etc/apt/sources.list

RUN apt-get update
RUN apt-get build-dep -y --force-yes distem
RUN apt-get update && apt-get install -y git rake

WORKDIR /tmp

RUN git clone https://gforge.inria.fr/git/distem/distem.git

WORKDIR /tmp/distem/

RUN rake snapshot
RUN dpkg -i ../distem*_amd64.deb || apt-get install -q -y --force-yes -f
WORKDIR /

# setting up keys
RUN ssh-keygen -P '' -f /root/.ssh/id_rsa
RUN wget http://public.nancy.grid5000.fr/~cruizsanabria/insecured_key.pub
RUN cat insecured_key.pub /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys

RUN echo "Host *\
          \n StrictHostKeyChecking no \
	  \n UserKnownHostsFile=/dev/null" > /root/.ssh/config

RUN mkdir /var/run/sshd
CMD ["/usr/sbin/sshd", "-D"]
