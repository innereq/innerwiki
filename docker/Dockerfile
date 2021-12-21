FROM debian:11-slim as builder

RUN echo "deb-src http://deb.debian.org/debian bullseye main" >> /etc/apt/sources.list \
    && apt-get update \
    && apt-get build-dep -y ikiwiki

WORKDIR /usr/src/ikiwiki

COPY . .

RUN perl Makefile.PL \
    && make \
    && make pure_install

FROM debian:11-slim

RUN apt-get update && apt-get upgrade -y \
    && apt-get install -y --no-install-recommends \
        git nginx-light fcgiwrap tcc \
        # ikiwiki
        gettext \
        libtext-markdown-discount-perl \
        libhtml-scrubber-perl \
        libhtml-template-perl \
        libhtml-parser-perl \
        liburi-perl \
        libxml-simple-perl \
        libtimedate-perl \
        libcgi-formbuilder-perl \
        libcgi-session-perl \
        libmail-sendmail-perl \
        libcgi-pm-perl \
        libyaml-libyaml-perl \
        libjson-perl \
        librpc-xml-perl \
        graphviz \
        libimage-magick-perl \
        libtext-csv-perl \
        liblwpx-paranoidagent-perl \
        pwgen \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

COPY --from=builder /usr/local /usr/local
COPY docker/nginx.conf /etc/nginx/sites-available/default
COPY docker/entrypoint.sh /opt/entrypoint.sh

WORKDIR /var/lib/ikiwiki

ENTRYPOINT ["/bin/bash", "/opt/entrypoint.sh"]
