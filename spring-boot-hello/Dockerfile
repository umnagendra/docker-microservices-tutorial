FROM ubuntu:latest

RUN apt-get update                      \
&& apt-get -y install openjdk-8-jdk     \
&& apt-get -y install git               \
&& mkdir -p /src                        \
&& cd /src                              \
&& git clone https://github.com/buzypi/spring-boot-hello.git    \
&& cd spring-boot-hello                 \
&& ./mvnw clean install                 \
&& apt-get -y remove --purge git        \
&& apt -y autoremove

WORKDIR /src/spring-boot-hello/target

CMD ["java", "-jar", "./spring-boot-hello-0.0.1-SNAPSHOT.jar"]

