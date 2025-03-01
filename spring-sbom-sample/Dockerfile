FROM openjdk:21-jdk-slim

WORKDIR /app

COPY target/app.jar app.jar
COPY target/bom.json bom.json

EXPOSE 8080
EXPOSE 8081

ENTRYPOINT ["java", "-jar", "app.jar"]
