FROM docker:dind

RUN apk add --update bash supervisor curl vim jq unzip net-tools ca-certificates wget ip6tables
RUN sed -i 's/\/bin\/ash/\/bin\/bash/g' /etc/passwd

RUN wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub
RUN wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.28-r0/glibc-2.28-r0.apk
RUN apk add glibc-2.28-r0.apk

RUN mkdir -p /etc/supervisor.d/
RUN sed -i 's/;nodaemon=false/nodaemon=true/g' /etc/supervisord.conf
COPY nomad.ini /etc/supervisor.d/

RUN curl -o /tmp/nomad.zip -L -s https://releases.hashicorp.com/nomad/0.10.0/nomad_0.10.0_linux_amd64.zip && \
    unzip -d /usr/local/bin/ /tmp/nomad.zip

RUN mkdir -p /etc/nomad.d
COPY nomad.hcl /etc/nomad.d/server.hcl

RUN mkdir -p /opt/cni/bin && \
    curl -o /tmp/cni.tar.gz -L https://github.com/containernetworking/plugins/releases/download/v0.8.1/cni-plugins-linux-amd64-v0.8.1.tgz && \
    tar -xzf /tmp/cni.tar.gz -C /opt/cni/bin


RUN curl -o /tmp/consul.zip -L -s https://releases.hashicorp.com/consul/1.6.1/consul_1.6.1_linux_amd64.zip && \
    unzip -d /usr/local/bin/ /tmp/consul.zip

RUN mkdir -p /etc/consul.d
COPY consul.hcl /etc/consul.d/server.hcl

ENTRYPOINT ["/usr/bin/supervisord"]
CMD ["-c", "/etc/supervisord.conf"]