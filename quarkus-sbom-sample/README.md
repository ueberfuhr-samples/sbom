# Quarkus SBOM Sample

## Application SBOM

Quarkus provides a [specific tooling for SBOMs](https://quarkus.io/guides/cyclonedx) , because it enhances dependency management tools like Maven or Gradle by its own dependency resolver. So, the default resolving mechanism of Maven or Gradle is not sufficient to generate a complete SBOM.

To generate an SBOM for this Quarkus project, we need to run the following command:

```bash
mvn quarkus:dependency-sbom
```

## Container SBOM

> [!NOTE]  
> We'll need Docker Scout `v1.15.0+`.

We need to copy the application's SBOM into the container image during build time.

```bash
mvn clean package
mvn quarkus:dependency-sbom
# build the image
docker compose build
# build and run
docker compose up -d
```

We can now generate an SBOM which includes both project and image dependencies.

We can do this with `docker scout` like this:
```bash
docker scout sbom \
  quarkus-sbom-sample:1.0.0 \
  --format cyclonedx \
  --output target/container-sbom.json
```

And we can print CVE´s:
```bash
docker scout cves \
  quarkus-sbom-sample:1.0.0 \
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

