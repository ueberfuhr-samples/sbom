# Spring SBOM Sample

Spring Boot [directly supports SBOM generation](https://spring.io/blog/2024/05/24/sbom-support-in-spring-boot-3-3) since
version `3.3`. This includes

- A managed dependency of the CycloneDX Plugin for [Maven](https://github.com/CycloneDX/cyclonedx-maven-plugin)
  and [Gradle](https://github.com/CycloneDX/cyclonedx-gradle-plugin). This plugin generates a CycloneDX SBOM during the
  Maven Build Lifecyle that is packaged into the JAR.
- An actuator endpoint to read the SBOM from the running application.

To generate and read the SBOM for this project, simply run

```bash
# Build the application
mvn clean package
# Run the application
java -jar target/spring-sbom-sample-0.0.1-SNAPSHOT.jar
# Read the SBOM
curl http://localhost:8080/actuator/sbom/application
```
