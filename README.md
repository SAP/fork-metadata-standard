[![REUSE status](https://api.reuse.software/badge/github.com/SAP/fork-metadata-standard)](https://api.reuse.software/info/github.com/SAP/fork-metadata-standard)

# Fork Metadata Standard

## About this project

_In a nutshell:_

The Fork Metadata Standard (FMS) defines a structured, platform-agnostic format for documenting the origin of a forked open-source project.

_In detail:_

Development teams may fork or copy open source projects and continue using this fork internally at an organization and independently from the upstream project. A popular reason is the requirement to add a certain feature to the project that can't be integrated into the upstream project for business reasons. Once such forks are created, several challenges arise, especially in the areas software bill of materials (SBOM) management and vulnerability management.

New regulations in these areas, for instance the EU Cyber Resilience Act (CRA), have been introduced and will come into effect shortly. They require an end-to-end SBOM and vulnerability management solution for all products that are made available on the affected markets. To our knowledge, there is no dedicated best practice or standard to be able to record, track and analyze the status and provenance/pedigree of organization-internal forks of an open source project.

Therefore, we propose the introduction of a new "Fork Metadata Standard" (FMS) to be able to record all required aspects of such forks, mainly the original project and the upstream synchronization status. This data can then be reused by SBOM management systems to track the provenance/pedigree of a software component used in a product or service.

## Specification

Please see [fork-metadata-standard.md](fork-metadata-standard.md) for the detailed specification.

## Roadmap

The specification is still in early stages. We plan to make dedicated releases, add examples, GitHub Pages and more details in the next weeks and months. Stay tuned!

## Support, Feedback, Contributing

This project is open to feature requests/suggestions, bug reports etc. via [GitHub issues](https://github.com/SAP/fork-metadata-standard/issues). Contribution and feedback are encouraged and always welcome. For more information about how to contribute, the project structure, as well as additional contribution information, see our [Contribution Guidelines](CONTRIBUTING.md).

## Security / Disclosure

If you find any bug that may be a security problem, please follow our instructions at [in our security policy](https://github.com/SAP/fork-metadata-standard/security/policy) on how to report it. Please do not create GitHub issues for security-related doubts or problems.

## Code of Conduct

We as members, contributors, and leaders pledge to make participation in our community a harassment-free experience for everyone. By participating in this project, you agree to abide by its [Code of Conduct](https://github.com/SAP/.github/blob/main/CODE_OF_CONDUCT.md) at all times.

## Licensing

Copyright 2025 SAP SE or an SAP affiliate company and fork-metadata-standard contributors. Please see our [LICENSE](LICENSE) for copyright and license information. Detailed information including third-party components and their licensing/copyright information is available [via the REUSE tool](https://api.reuse.software/info/github.com/SAP/fork-metadata-standard).
