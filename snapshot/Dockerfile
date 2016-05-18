FROM ubuntu:14.04

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update \
	&& apt-get install -y --no-install-recommends \
		default-jre-headless \
	&& rm -rf /var/lib/apt/lists/*

RUN buildDeps=' \
    openjdk-7-jdk \
    maven \
    git \
	' \
	&& set -x \
	&& apt-get update \
	&& apt-get install -y --no-install-recommends $buildDeps \
	&& rm -rf /var/lib/apt/lists/* \
  && cd /usr/src \
  && git clone -b master https://github.com/andrewgaul/s3proxy.git \
  && cd /usr/src/s3proxy \
  && mvn package \
  && mkdir /usr/local/s3proxy \
  && cp /usr/src/s3proxy/target/s3proxy /usr/local/s3proxy \
  && rm -rf /root/.m2 \
  && rm -rf /usr/src/s3proxy \
	&& apt-get purge -y --auto-remove $buildDeps

ENV PATH /usr/local/s3proxy:$PATH
ADD s3proxy.conf /usr/local/s3proxy/s3proxy.conf

RUN mkdir -p /data
WORKDIR /data

EXPOSE 8080

CMD ["s3proxy", "--properties", "/usr/local/s3proxy/s3proxy.conf"]
