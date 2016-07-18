FROM ubuntu:14.04
MAINTAINER Mansheng Yang

RUN echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" > /etc/apt/sources.list.d/webupd8team-java.list
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys EEA14886

RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections

RUN apt-get -y update && apt-get install -y
    autoconf \
    build-essential \
    libapr1-dev \
    libcurl4-nss-dev \
    libsasl2-dev \
    libsasl2-modules \
    libsvn-dev \
    libtool \
    maven \
    oracle-java8-installer \
    python-dev \
    tar \
    wget \
  && rm -rf /var/lib/apt/lists/* && rm -rf /var/cache/oracle-jdk8-installer

ENV JAVA_HOME /usr/lib/jvm/java-8-oracle
ENV MS_VERSION=1.0.0-rc1

WORKDIR /tmp

RUN wget https://github.com/apache/mesos/archive/$MS_VERSION.tar.gz && \
  tar xf $MS_VERSION.tar.gz && \
  rm $MS_VERSION.tar.gz && \
  cd mesos-$MS_VERSION && \
  ./bootstrap && \
  mkdir build && cd build && \
  ../configure && \
  make -j8 && \
  make install && \
  cd /tmp && rm -rf mesos-$MS_VERSION

WORKDIR /

# libmesos is located in /usr/local/lib
ENV LD_LIBRARY_PATH=/usr/local/lib

CMD ["bash"]
