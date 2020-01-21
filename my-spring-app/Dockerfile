FROM ubuntu:latest

RUN apt-get update                      \
&& apt-get -y install openjdk-8-jdk     \
&& apt-get -y install git               \
&& mkdir -p /src                        \
&& cd /src                              \
&& git clone https://github.com/buzypi/spring-jpa.git    \
&& cd spring-jpa                        \
&& ./mvnw clean install                 \
&& apt-get -y remove --purge git        \
&& apt -y autoremove

WORKDIR /src/spring-jpa/target

CMD ["java", "-jar", "./demo-0.0.1-SNAPSHOT.jar"]

