FROM registry.access.redhat.com/ubi8

ENV LATEST_STABLE_RELEASE v1.8.0

RUN yum update -y && \
    yum install java-1.8.0-openjdk-devel git wget autoconf automake gcc-c++ make bzip2 curl unzip zlib-devel libtool -y && \
    git clone git://github.com/google/protobuf.git  && \
    cd protobuf  && \
    git checkout v3.6.0 && \
    git submodule update --init --recursive && \
   ./autogen.sh  && \
   ./configure && \
   make && \
   make install && \
   ldconfig && \
   cd .. && \
   rm -rf protobuf && \
   export LD_LIBRARY_PATH=/usr/local/lib  && \
   mkdir -p  /root/.gradle/caches/modules-2/files-2.1/com.google.protobuf/protoc/3.6.0/  && \
   cp /usr/local/bin/protoc /root/.gradle/caches/modules-2/files-2.1/com.google.protobuf/protoc/3.6.0/protoc-3.6.0-linux-ppcle_64.exe  && \
   git clone https://github.com/apache/incubator-geode.git && \
   cd incubator-geode && \
   git checkout rel/${LATEST_STABLE_RELEASE} && \
   sed -i '37d' geode-protobuf-messages/build.gradle \
   && ./gradlew build -Dskip.tests=true -xjavadoc \
   && ls /incubator-geode | grep -v geode-assembly | xargs rm -rf \
   && rm -rf /root/.gradle/ \
   && rm -rf /incubator-geode/geode-assembly/build/distributions/ \
   && rm -rf /usr/share/locale/* /usr/local/lib/libprot*  \
   && rm -rf /usr/local/bin/protoc  \
   && yum remove -y wget autoconf automake gcc-c++ make git zlib-devel 

ENV GEODE_HOME /incubator-geode/geode-assembly/build/install/apache-geode
ENV PATH $PATH:$GEODE_HOME/bin
EXPOSE  8080 10334 40404 1099 7070
CMD ["gfsh"]
