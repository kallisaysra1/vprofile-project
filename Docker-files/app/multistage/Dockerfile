#FROM openjdk:11 AS BUILD_IMAGE
#RUN apt update && apt install maven -y
#RUN git clone https://github.com/kallisaysra1/vprofile-project.git
#RUN cd vprofile-project && git checkout docker && mvn install
#FROM tomcat:9-jre11
#RUN rm -rf /usr/local/tomcat/webapps/*
#COPY --from=BUILD_IMAGE vprofile-project/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war
#EXPOSE 8080
#CMD ["catalina.sh", "run"]

#######################
# Stage 1: Build Image
FROM openjdk:11 AS build_image
RUN apt update && apt install -y maven git

# Retry git clone command
RUN git clone https://github.com/kallisaysra1/vprofile-project.git || \
    (sleep 5 && git clone https://github.com/kallisaysra1/vprofile-project.git)

WORKDIR vprofile-project
RUN git checkout docker && mvn install

# Stage 2: Tomcat Image
FROM tomcat:9-jre11
RUN rm -rf /usr/local/tomcat/webapps/*
COPY --from=build_image /vprofile-project/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war
EXPOSE 8080
CMD ["catalina.sh", "run"]
