FROM centos:centos7.6.1810

RUN mkdir /tools
WORKDIR /tools

#Install default services
RUN yum install -y wget git gcc bzip2 fontconfig python3 java-1.8.0-openjdk-devel.x86_64
RUN ln -sf /usr/bin/python3 /usr/bin/python

ENV JAVA_HOME /usr/lib/jvm/java-1.8.0-openjdk/
ENV PATH $JAVA_HOME/bin:$PATH

RUN wget https://dlcdn.apache.org/maven/maven-3/3.9.4/binaries/apache-maven-3.9.4-bin.tar.gz --no-check-certificate

RUN tar xfz apache-maven-3.9.4-bin.tar.gz
RUN ln -sf /tools/apache-maven-3.9.4 /tools/maven

ENV  PATH /tools/maven/bin:$PATH
ENV MAVEN_OPTS "-Xmx2048m -XX:MaxPermSize=512m"

RUN mkdir -p /scripts

RUN echo "#!/bin/bash" > /scripts/mvn.sh
RUN echo 'set -x; exec "\$@" ' >> /scripts/mvn.sh

RUN chmod -R 777 /scripts
RUN chmod -R 777 /tools

ENTRYPOINT ["/scripts/mvn.sh"]
