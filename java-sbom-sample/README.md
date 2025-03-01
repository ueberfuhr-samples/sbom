# Simple Java Sample

This sample is a simple Maven project that builds a JAR.
We have the choice between two alternatives to generate BOMs.

> [!WARNING]
> In this project, we use Log4j version `2.13.1`, which already has vulnerabilities.

## From Developers Perspective

As a developer, we could configure the [`pom.xml`](pom.xml) file to bind the [CycloneDX](https://github.com/CycloneDX/cyclonedx-maven-plugin) or [SPDX](https://github.com/spdx/spdx-maven-plugin) Maven plugins to the build lifecycle. In the given sample, there is a profile for each BOM format configured.

We have to build the project with the profiles activated:

```bash
mvn clean install -Pcyclonedx,spdx
```

This will install the JAR and the BOMs separately into the local Maven repository.

## From DevOps Perspective

We could reach the same without any modification of the `pom.xml` file. For this, we just invoke the plugin goals via command line:

```bash
# generate CycloneDX SBOM
mvn org.cyclonedx:cyclonedx-maven-plugin:2.9.1:makeBom
# generate SPDX SBOM
mvn org.spdx:spdx-maven-plugin:1.0.0-RC1:createSPDX
```

Of course, Maven would also allow to install those files into a Maven repository.

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
  java-sbom-sample:1.0.0 \
  --format cyclonedx \
  --output target/container-sbom.json
```

And we can print CVE´s:
```bash
docker scout cves \
  java-sbom-sample:1.0.0 \
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
