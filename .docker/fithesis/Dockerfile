FROM ubuntu:16.04

ENV DEBIAN_FRONTEND noninteractive

RUN apt update \
	&& apt install -y software-properties-common build-essential ca-certificates wget \
	&& add-apt-repository ppa:jonathonf/texlive-2016 \
	&& apt update \
	&& apt install -y biber texlive-full xzdec \
	&& tlmgr init-usertree \
	&& tlmgr install fithesis

RUN tlmgr install lh \
	&& mktextfm larm1000 \
	&& mktexmf larm1000

ENV GOSU_VERSION 1.10
RUN set -x \
	&& dpkgArch="$(dpkg --print-architecture | awk -F- '{ print $NF }')" \
	&& wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch" \
	&& wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch.asc" \
	&& export GNUPGHOME="$(mktemp -d)" \
	&& gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
	&& gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu \
	&& rm -r "$GNUPGHOME" /usr/local/bin/gosu.asc \
	&& chmod +x /usr/local/bin/gosu \
	&& gosu nobody true

ADD entrypoint /usr/local/bin/
RUN chmod a+x /usr/local/bin/entrypoint

VOLUME /doc
WORKDIR /doc

ENTRYPOINT ["/usr/local/bin/entrypoint"]
