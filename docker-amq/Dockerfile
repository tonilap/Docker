FROM ubuntu:14.04
MAINTAINER Toni <tonilap@gmail.com>

# Install Java
RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections \
    && apt-get update -y                              \
    && apt-get install software-properties-common -y  \
    && add-apt-repository ppa:webupd8team/java -y     \
    && apt-get update -y                              \
    && apt-get install oracle-java8-installer -y      \
    && rm -rf /var/lib/apt/lists/*                    \
    && rm -rf /var/cache/oracle-jdk8-installer

# Define JAVA_HOME variable
ENV JAVA_HOME /usr/lib/jvm/java-8-oracle

# Download ActiveMQ and install
ADD http://ftp.cixug.es/apache/activemq/5.11.1/apache-activemq-5.11.1-bin.tar.gz /tmp/
RUN cd /tmp                                             \
    && tar xf apache-activemq-5.11.1-bin.tar.gz         \
    && mv apache-activemq-5.11.1 /opt/                  \
    && ln -sf /opt/apache-activemq-5.11.1 /opt/activemq

# ActiveMQ start configuration
RUN sed -i 's/-Xms1G -Xmx1G/-Xms128m -Xmx128m/g' /opt/activemq/bin/activemq                                                            \
    && sed -i 's/COMMANDLINE_ARGS >\/dev\/null 2>\&1 &/COMMANDLINE_ARGS >\/dev\/null 2>\&1/g' /opt/apache-activemq-5.11.1/bin/activemq \
    && sed -i 's/COMMANDLINE_ARGS --pid $PID &/COMMANDLINE_ARGS --pid $PID/g' /opt/apache-activemq-5.11.1/bin/activemq

# Add activemq configuration file
ADD activemq.xml /opt/activemq/conf/

# Add activemq user and change ownership
RUN adduser --disabled-password --home=/activemq activemq      \
    && chown -R activemq:activemq /opt/apache-activemq-5.11.1/ \
    && mkdir -p /vol/www/activemq/                             \
    && chown -R activemq:activemq /vol/www/activemq/

EXPOSE 8161 61613 61616

USER activemq

CMD /opt/activemq/bin/activemq start
