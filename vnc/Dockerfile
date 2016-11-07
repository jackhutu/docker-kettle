FROM ubuntu:14.04
MAINTAINER Jack Hu <hello@jackhu.top>

ENV HOME /root

RUN apt-get update \
  && apt-get install -y supervisor \
    xfce4 xfce4-goodies \
    x11vnc xvfb \
  && apt-get autoclean \
  && apt-get autoremove \
  && rm -rf /var/lib/apt/lists/*

WORKDIR /root

ADD startup.sh ./
ADD supervisord.conf ./

EXPOSE 5900
EXPOSE 22

ENTRYPOINT ["./startup.sh"]