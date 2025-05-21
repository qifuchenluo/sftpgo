FROM ubuntu:22.04

RUN mkdir /tools
WORKDIR /tools

#Install default services
#RUN yum clean all
RUN apt-get update
RUN apt-get install -y wget
RUN apt-get install -y git
RUN apt-get install -y gcc
RUN apt-get install -y bzip2 fontconfig
RUN apt-get install -y diffutils
RUN apt-get install -y python3

RUN ln -s /usr/bin/python3 /usr/bin/python

#RUN apt-get install -y java-1.8.0-openjdk java-1.8.0-openjdk-devel
#ENV JAVA_HOME /etc/alternatives/jre
RUN apt-get install -y openjdk-11-jdk 
ENV JAVA_HOME  /usr/lib/jvm/java-11-openjdk-amd64 
ENV PATH $JAVA_HOME/bin:$PATH

#Download and install JDK8 from AWS s3's docker-assets 
#RUN wget https://s3.eu-central-1.amazonaws.com/docker-assets/dist/jdk-8u101-linux-x64.rpm
#RUN rpm -i jdk-8u101-linux-x64.rpm
#ENV JAVA_HOME /usr/java/latest
#ENV  PATH $JAVA_HOME/bin:$PATH



ADD https://www.apache.org/dist/maven/maven-${MAVEN_MAJOR_VER}/${MAVEN_VER}/binaries/apache-maven-${MAVEN_VER}-bin.tar.gz.sha512 /tools
ADD http://www.apache.org/dist/maven/maven-${MAVEN_MAJOR_VER}/${MAVEN_VER}/binaries/apache-maven-${MAVEN_VER}-bin.tar.gz /tools
RUN sha512sum  apache-maven-${MAVEN_VER}-bin.tar.gz | cut -f 1 -d " " > tmp.sha1

RUN cat apache-maven-${MAVEN_VER}-bin.tar.gz.sha512 | cut -f 1 -d " " > tmp.sha1.download

RUN diff -w tmp.sha1 tmp.sha1.download

RUN tar xfz apache-maven-${MAVEN_VER}-bin.tar.gz
RUN ln -sf /tools/apache-maven-${MAVEN_VER} /tools/maven

ENV  PATH /tools/maven/bin:$PATH
ENV MAVEN_OPTS "-Xmx2048m -XX:MaxPermSize=512m"

# Setup gosu for easier command execution
RUN useradd -ms /bin/bash builder
RUN usermod -g root builder
RUN mkdir -p /scripts

RUN chmod -R 777 /scripts
RUN chmod -R 777 /tools
