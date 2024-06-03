# Utilize the latest OpenJDK 11 base image
FROM openjdk:11-slim AS BUILD_IMAGE

# Install essential packages
RUN apt-get update && apt-get install -y --no-install-recommends maven && rm -rf /var/lib/apt/lists/*

# Clone the repository and build the project
RUN git clone https://github.com/iamreymond/CI-Jenkins.git && \
    cd CI-Jenkins && \
    git checkout PAAC_CI_Docker_ECR && \
    mvn install

# Use Tomcat 9 with JRE 11 for the application server
FROM tomcat:9-jre11

# Remove default webapps
RUN rm -rf /usr/local/tomcat/webapps/*

# Copy the built WAR file to the Tomcat webapps directory
COPY --from=BUILD_IMAGE /CI-Jenkins/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

# Expose port 8080 for the application
EXPOSE 8080

# Start Tomcat server
CMD ["catalina.sh", "run"]
