# Spring SBOM Sample

## Application SBOM

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
java -jar target/app.jar
# Test the running application
curl http://localhost:8080 -i
# Read the SBOM
curl http://localhost:8081/actuator/sbom/application -i
```

Of course, we could also generate the SBOM separately:

```bash
mvn org.cyclonedx:cyclonedx-maven-plugin:2.9.1:makeBom
```

To get a BOM with vulnerabilities, we could set a dependency version backwards, e.g.

```bash
mvn org.cyclonedx:cyclonedx-maven-plugin:2.9.1:makeBom \
  -Djackson-bom.version=2.9.8
```

## Container SBOM

> [!NOTE]  
> We'll need Docker Scout `v1.15.0+`.

We need to copy the application's SBOM into the container image during build time.

```bash
mvn clean package
mvn org.cyclonedx:cyclonedx-maven-plugin:2.9.1:makeBom
# build the image
docker compose build
# build and run
docker compose up -d
```

We can now generate an SBOM which includes both project and image dependencies.

We can do this with `docker scout` like this:
```bash
docker scout sbom \
  spring-sbom-sample:1.0.0 \
  --format cyclonedx \
  --output target/container-sbom.json
```

And we can print CVE´s:
```bash
docker scout cves \
  spring-sbom-sample:1.0.0 \
  --format markdown \
  --output target/container-cves.md
```

or in SBOM format:

```bash
docker scout cves \
  spring-sbom-sample:1.0.0 \
  --format sbom \
  --output target/container-cves-sbom.json
```


> [!NOTE]  
> We can find details about the vulnerability databases [here](https://docs.docker.com/scout/deep-dive/advisory-db-sources/).
