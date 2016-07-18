FROM ssig33/ruby-imagemagick-groonga:latest
MAINTAINER whywaita <dev@whywrite.it>

RUN apk add --update bash \
    && apk add ruby-dev libxml2-dev \
    && rm -rf /var/cache/apk/*

RUN mkdir /root/groonga \
    && mkdir /root/userdata \
    && mkdir /root/.pit

RUN git clone https://github.com/ssig33/pig-media-server.git
RUN cd pig-media-server ; bundle config build.nokogiri  ; bundle install --path vendor/bundle

ADD ./pit.yaml /root/.pit/pit.yaml
ADD ./default.yaml /root/.pit/default.yaml

RUN echo '#!/bin/sh' >> /etc/periodic/15min/pms-crawl  \
    && echo '' >> /etc/periodic/15min/pms-crawl \
    && echo '*/5 * * * * cd /pig-media-server/bin/ ; ./pig-media-server crawl' >> /etc/periodic/15min/pms-crawl \
    && chmod a+x /etc/periodic/15min/pms-crawl

WORKDIR /pig-media-server/bin/
VOLUME ["/mnt/mp4"]

ENTRYPOINT ./pig-media-server crawl ; ./pig-media-server server 12345
