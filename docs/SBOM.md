# Software Bill of Materials (SBOMs)

## What is it?

A Software Bill of Materials (SBOM) is a detailed inventory of all components that make up a software application, including open-source libraries, proprietary code, dependencies, and version numbers. It is essentially a "bill of materials" (like in manufacturing) but for software, providing transparency into what’s inside an application.

The key components of an SBOM are:
1. _Component Name_ – Name of each software package or library.
2. _Version Information_ – Specific version of each component.
3. _Supplier Information_ – Who developed or provided the component.
4. _License Details_ – Open-source or proprietary licensing terms.
5. _Dependencies_ – Other software components that a package relies on.
6. _Security Vulnerabilities_ – Known risks associated with components (if available).

## Why do we need them?

Software Bill of Materials (SBOMs) play a crucial role in [DORA (Digital Operational Resilience Act)](DORA.md) compliance by improving transparency, security, and risk management in financial institutions' software supply chains.

DORA does not explicitly prescribe the use of SBOMs. However, its requirements around ICT risk management, third-party risk, and resilience testing strongly align with the benefits that SBOMs provide.

While SBOMs are not mandated, they can significantly help organizations meet DORA’s compliance requirements. Given the increasing focus on software transparency and cybersecurity, SBOMs are becoming an industry best practice and could be referenced in future regulatory guidance or audits under DORA.

Here’s how SBOMs help organizations meet DORA requirements.

### Enhanced ICT Risk Management

SBOMs...
- ... provide detailed visibility into all software components, including open-source and third-party libraries.
- ... identify vulnerabilities and outdated components that could pose cybersecurity risks.
- ... support proactive risk assessments by tracking dependencies and potential security flaws.

### Faster Incident Response & Recovery

SBOMs...
- ... enable rapid identification of affected software during a cyberattack or security incident.
- ... improve patch management by quickly finding and updating vulnerable components
- ... reduce downtime and financial losses by facilitating a structured response.

### Compliance with Resilience Testing

SBOMs...
- ... facilitate security audits and penetration testing by mapping software components.
- ... help organizations simulate cyberattack scenarios and test system resilience.
- ... ensure continuous monitoring of software supply chain risks.

### Strengthening Third-Party Risk Management

SBOMs...
- ... provide transparency into third-party software dependencies, reducing supply chain risks.
- ... ensure ICT service providers comply with secure coding practices and vulnerability management.
- ... help organizations enforce vendor security policies by requiring SBOMs from suppliers.

### Supporting Regulatory Reporting & Documentation

SBOMs...
- ... serve as a detailed record of software components for regulatory audits.
- ... help organizations provide accurate and up-to-date security reports to regulators.
- ... ensure compliance with cybersecurity reporting obligations by tracking vulnerabilities systematically.

### Key Takeaways:
SBOMs...
- ... enhance cybersecurity posture and reduce risks from software vulnerabilities.
- ... enable faster response to incidents and regulatory inquiries.
- ... can be integrated into financial institutions' ICT risk management frameworks to streamline DORA compliance.

## How to implement them?

Implementing an SBOM effectively requires choosing the right tools, integrating it into your development workflow, and ensuring ongoing security monitoring. Here’s how to do it:
1. Choose the right SBOM format
2. Use automated SBOM generation tools
3. Integrate SBOM Generation into CI/CD Pipelines
4. Use SBOMs for Security & Compliance
5. Share and Distribute SBOMs Securely

### Choose the right SBOM format

There are three widely accepted formats for SBOMs:

| Feature               | CycloneDX                                                | SPDX (System Package Data Exchange)                                                              | SWID Tags                                                             |
|-----------------------|----------------------------------------------------------|--------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------|
| Notes                 | recommended                                              |                                                                                                  | not a full-fledged SBOM format                                        |
| Primary focus         | application security, dependency management              | license compliance, intellectual property tracking                                               | software asset management, inventory tracking                         |
| Developed by          | OWASP (open-source)                                      | Linux Foundation (open-source), now [ISO/IEC 5962:2021](https://www.iso.org/standard/81870.html) | [ISO/IEC 19770-2:2015](https://www.iso.org/standard/65666.html)       |
| Use Case              | software supply chain security, vulnerability management | license compliance, legal documentation, software provenance                                     | tracking installed software on devices and IT infrastructure          |
| Best for              | DevSecOps, SBOM automation, security analysis            | open-source licensing, compliance audits, legal use cases                                        | enterprise software tracking, asset management, government compliance |
| Data coverage         | vulnerabilities, component integrity, security risks     | licenses, copyrights, software origins                                                           | software installations, updates, patches, and ownership               |
| Supported media types | JSON, XML, Protobuf                                      | data model based on RDF, but serializations for JSON, YAML, XML, ...                             | XML, JSON (SWIDJS), CBOR (IoT use cases)                              |
| Documentation         | https://cyclonedx.org/docs/1.6/                          | https://spdx.github.io/spdx-spec/v3.0.1/                                                         | https://standards.iso.org/iso/19770/-2/2015/schema.xsd                |

A financial institution may use ...
- ... CycloneDX to track vulnerabilities in third-party software.
- ... SPDX to ensure legal compliance with software licenses.
- ... SWID to maintain an IT asset inventory for audits and updates.

We can find more details in
- [SBOM Standard Formats (German article)](https://scribesecurity.com/de/sbom/standard-formats/)
- [Guidelines for SWID Tags (NIST)](https://csrc.nist.gov/pubs/ir/8060/final)

### Use automated SBOM generation tools

Use automated tools to generate SBOMs, such as:

- Open Source Tools:
  - [Syft (by Anchore)](https://github.com/anchore/syft)
  - [SPDX tools](https://pypi.org/project/spdx-tools/)
  - [CycloneDX CLI](https://github.com/CycloneDX/cyclonedx-cli)
  - [OWASP Dependency-Track](https://dependencytrack.org/)
- Integrated Solutions
  - [Spring Boot](https://spring.io/blog/2024/05/24/sbom-support-in-spring-boot-3-3)
  - [Quarkus](https://quarkus.io/guides/cyclonedx)
  - [Docker Scout](https://docs.docker.com/scout/)
- Commercial Solutions:
  - [Snyk](https://docs.snyk.io/snyk-cli/commands/sbom)
  - [JFrog Xray](https://jfrog.com/help/r/jfrog-security-documentation/xray-sbom-report)
  - [Black Duck (Synopsys)](https://www.blackduck.com/blog/building-sbom-with-black-duck.html)
  - [GitHub Dependency Graph](https://docs.github.com/en/rest/dependency-graph/sboms)

### Integrate SBOM Generation into CI/CD Pipelines
Automate SBOM creation at build time.
- Use tools like GitHub Actions, GitLab CI, Jenkins, or Azure DevOps to ensure SBOMs are updated dynamically.
- Store SBOMs in artifact repositories (e.g., JFrog Artifactory, Harbor). Storing them separately (not packaging them into the artifacts directly) allows easy retrieval without downloading the whole artifact.

### Use SBOMs for Security & Compliance
- Vulnerability Management: Compare SBOM data with vulnerability databases (e.g., NVD, OSV).
- License Compliance: Ensure software adheres to open-source licensing policies.
- Incident Response: Quickly identify affected components during security breaches.

### Share and Distribute SBOMs Securely
- Store SBOMs in a secure repository.
- Share them using APIs or packaged in software artifacts.
- Ensure cryptographic integrity using digital signatures.
