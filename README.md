# SBOM Samples

[![CI Build (Java Sample)](https://github.com/ueberfuhr-samples/sbom/actions/workflows/ci-java.yml/badge.svg)](https://github.com/ueberfuhr-samples/sbom/actions/workflows/ci-java.yml)
[![CI Build (Spring Sample)](https://github.com/ueberfuhr-samples/sbom/actions/workflows/ci-spring.yml/badge.svg)](https://github.com/ueberfuhr-samples/sbom/actions/workflows/ci-spring.yml)
[![CI Build (Quarkus Sample)](https://github.com/ueberfuhr-samples/sbom/actions/workflows/ci-quarkus.yml/badge.svg)](https://github.com/ueberfuhr-samples/sbom/actions/workflows/ci-quarkus.yml)

## See the Code

This repository contains samples for

- [A simple java project](java-sbom-sample)
- [A simple Spring Boot sample](spring-sbom-sample)
- [A simple Quarkus sample](quarkus-sbom-sample)

If you want to learn about SBOMs in general you could also
find some [documentation](docs/README.md).

## See the Results

We can create a pull request in this repository, which will lead to automatically running pipelines.
This includes creating a container image with SBOM and get vulnerabilities directly printed to the PR's comments.
We can find a [sample PR](https://github.com/ueberfuhr-samples/sbom/pull/1) in this repository.

We could also download the generated artifacts directly.

### Download the application SBOM from GitHub Packages

> [!NOTE]
> The application's SBOMs are stored into a GitHub Packages repository. It is public, but GitHub allows access only via
> a personal access token (PAT). So first, we need to create one with the `read:packages` permission.

We put the access token into our local Maven installation's `settings.xml` file like this:

```xml

<settings
  xmlns="http://maven.apache.org/SETTINGS/1.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 https://maven.apache.org/xsd/settings-1.0.0.xsd">

  <servers>
    <server>
      <!-- The id is referenced from the command line invocation below -->
      <id>github</id>
      <username>{YOUR-GITHUB-USERNAME}</username>
      <password>{YOUR-GITHUB-ACCESSTOKEN}</password>
    </server>
  </servers>
</settings>
```

Then, we can download the SBOM with this command:

```bash
# Download the latest SBOM for the Simple Java Project into the current working directory
mvn dependency:get \
  -Dmaven.repo.local=./local-repo \
  -DremoteRepositories=github::::https://maven.pkg.github.com/ueberfuhr-samples/sbom/ \
  -Dartifact=com.samples.sbom:java-sbom-sample:LATEST:json:cyclonedx \
  && cp ./local-repo/com/samples/sbom/**/*-cyclonedx.json . \
  && rm -r ./local-repo
   
# for Spring Boot: -Dartifact=com.samples.sbom:spring-sbom-sample:LATEST:json:cyclonedx
# for Quarkus:     -Dartifact=com.samples.sbom:quarkus-sbom-sample:LATEST:json:cyclonedx
```

### Pull the container image from DockerHub

The container image created by GitHub actions
has [provenance attestations](https://docs.docker.com/build/metadata/attestations/slsa-provenance/), which we could
print to the console using

```bash
docker buildx imagetools inspect ralfueberfuhr/java-sbom-sample
# for Spring Boot: ralfueberfuhr/spring-sbom-sample
# for Quarkus:     ralfueberfuhr/quarkus-sbom-sample
```

We can also create an SBOM from the container image that was pushed to DockerHub:

```bash
docker scout sbom \
  ralfueberfuhr/java-sbom-sample \
  --format cyclonedx \
  --output container-sbom.json

# create vulnerabilities report
docker scout cves \
  ralfueberfuhr/java-sbom-sample \
  --format markdown \
  --output container-cves.md

# for Spring Boot: ralfueberfuhr/spring-sbom-sample
# for Quarkus:     ralfueberfuhr/quarkus-sbom-sample
```
