FROM ubuntu:latest as Git
RUN apt-get update && \
    apt-get install -y git
WORKDIR /app
RUN git clone http://github.com/jishnuk25/vprofile.git

FROM maven:3.9.2-eclipse-temurin-8-alpine as maven
COPY --from=Git /app/vprofile /app
WORKDIR /app
RUN mvn clean install
RUN ls -ltr /app/target/*.war

FROM tomcat:8-jre11
RUN rm -rf /usr/local/tomcat/webapps/*
COPY --from=maven /app/vprofile/target/*.war /usr/local/tomcat/webapps/ROOT.war
EXPOSE 8080
CMD ["catalina.sh", "run"]
WORKDIR /usr/local/tomcat
VOLUME /usr/local/tomcat/webapps