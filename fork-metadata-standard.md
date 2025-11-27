# Fork Metadata Standard

## Introduction

Existing solutions or possible workarounds as outlined before do not fully address the need for a standardized, platform-agnostic, and comprehensive way to document fork metadata. Therefore, we decided to choose a dedicated fork metadata file as solution to address fork provenance. We call this solution the "Fork Metadata Standard" (FMS). It is designed to combine the strengths of REUSE and SPDX (machine-readability and license compliance) with a dedicated structure for fork-specific information, offering a more complete and flexible solution.

## Solution Specification

The Fork Metadata Standard (FMS) defines a structured, platform-agnostic format for documenting the origin of a forked open-source project. It requires a `FORK.yaml` (or `FORK.json`) file in the root directory of the repository or the subfolder of a vendored and modified component, supplemented by human-readable information in the `README.md` and compliance with license requirements. In case of multiple forks residing in the same sub-folder, the respective `FORK.yaml` files (as well as other applicable files, such as `README.md` or `CHANGELOG.md`) are to be placed in individual folders within the hierarchy containing the forks, to separate the files.

### Principles

- Platform-Agnostic: Works with any version control system, programming language, or package manager.
- Simplicity: Minimal overhead for implementation in new or existing forks.
- Transparency: Clearly documents the original project’s details and the fork’s purpose.
- Machine-Readability: Uses structured YAML/JSON and SPDX identifiers for automation.
- License Compliance: Aligns with open-source license requirements for attribution.
- Traceability: Required metadata (e.g. `FORK.yaml`, enhanced `README.md`) can be packaged in build outputs to ensure that the metadata is always available even if the source repository is unavailable.

### Repository Structure

The fork’s repository should include:

- `FORK.yaml`: A structured file containing metadata about the original project and the fork.
- `README.md` or `README.ORGANIZATION_NAME.md`: A section summarizing the fork’s origin and purpose.
- Optional: A `LICENSES/` directory for additional licenses (REUSE-compliant) and a `CHANGELOG.md` for documenting changes.
- Source Files: SPDX-compliant copyright and license headers in modified and new files.

### FORK.yaml Format

The FORK.yaml file contains the core metadata in YAML format (JSON is an optional alternative). The structure is as follows:

```yaml
fork:
  upstream_project:
    name: "<Name of the upstream (original) project, human-readable, not to be used for automation, mandatory>"
    repository: "<URL of the upstream (original) repository, if applicable including the subfolder e.g. in case of mono-repos, mandatory>"
    branch: "<Used branch for the fork, mandatory if available>"
    license: "<SPDX license identifier, e.g., MIT, GPL-3.0-or-later, mandatory>"
    authors: "<Name(s) or organization of the original authors (no need to mention all authors, limit the number to 3 or use a generic term like 'project authors'), mandatory>"
    homepage: "<URL of the project’s homepage, optional>"
    purl: "<Package URL (https://github.com/package-url/purl-spec), without version, qualifiers or subpaths, use the package distribution channel (e.g. npm, pypi etc.) as type, only fallback to source repository (github, gitlab etc.) if no package distribution channel exists, mandatory>"
  details:
    name: "<Name of the fork, mandatory>"
    purpose: "<Brief description of the fork’s purpose, mandatory>"
    changes: "<Summary of main changes or new features, mandatory, detailed changelog to be maintained on CHANGELOG.md of the fork>"
    maintainer: "<Name or contact of the fork’s maintainer, mandatory>"
    created: "<Fork creation date, ISO 8601, e.g. 2025-05-21, mandatory>"
  upstream_sync:
    status: "<enum: [actively-synchronized, one-time-fork, abandoned], mandatory>"
    version: "<Version of the last upstream pull (for easier analysis in case of vulnerabilities), mandatory>"
    commit_hash: "<Commit hash of the last upstream pull (for automation purposes), mandatory>"
    purl: "<Package URL (https://github.com/package-url/purl-spec) of the fork itself, without version, qualifiers or subpaths, optional>"
    last_sync: "<Date of last upstream pull, ISO 8601, e.g. 2025-05-21, optional>"
spdx_version: "3.0"  # SPDX major version for license identifier and file headers
```

Example:

```yaml
fork:
  original_project:
    name: "ExampleProject"
    repository: "https://github.com/example/exampleproject"
    branch: "main"
    license: "Apache-2.0"
    authors: "Example Team"
    homepage: "https://exampleproject.org"
    purl: "pkg:npm/example/exampleproject"
  details:
    name: "MyFork"
    purpose: "Adaptation for specific hardware"
    changes: "Optimized I/O performance"
    maintainer: "Max Mustermann <max.mustermann@example.com>"
    created: "2025-05-21"
  upstream_sync:
    status: "actively-synchronized"
    version: "v1.1"
    commit_hash: "9f0a3cce8ed24807abfba3b1ca49061a7fbc4e3f"
    last_sync: "2025-06-12"
spdx_version: "3.0"
```

### README Integration

The README of the project must include a section (e.g., "Fork Information") summarizing key details from `FORK.yaml` in natural language. Adding this information to the original `README.md` might create merge conflicts after upstream synchronizations. Thus, if development teams expect such a situation (e.g., because the `README.md` is changed often), they can create their own `README.ORGANIZATION_NAME.md` (e.g., `README.SAP.md`) file to add the fork information and potentially other fork-specific information (e.g., build instructions).

Example:

```markdown
This is a fork of [ExampleProject](https://github.com/example/exampleproject) (version v2.0.0).  
**Purpose**: Adaptation for specific hardware.  
**License**: Apache-2.0 (see `LICENSE`).  
**Maintainer**: Max Mustermann <max@example.com>.  
See `FORK.yaml` for details.
```

### License and Copyright

- License Retention: The original project’s `LICENSE` file must be retained, as required by its license.
- REUSE Compliance: Source files should include SPDX headers (e.g. SPDX-License-Identifier: Apache-2.0) for license and copyright information.
- Example Header:

```javascript
/* SPDX-License-Identifier: Apache-2.0

- Copyright (c) 2023 Example Team
- Copyright (c) 2025 Max Mustermann
 */
 ```

### Maintenance

- Upstream Sync: Development teams must update the `upstream_sync` field and its attributes in `FORK.yaml` when pulling changes from the original project. They also need to ensure that other metadata such as the repository link and the project homepage are still valid after an upstream sync.
- Changelog: A `CHANGELOG.md` should document differences from the original project. Development teams are advised to follow [established best practices](https://keepachangelog.com/) for format and content.

### Tooling

- REUSE Tool: Development teams can use `reuse lint` to validate license compliance. Please note that for a full REUSE compliance, development teams do not only need to add license headers to new or modified files, but also (mass) annotate all files that originate from the upstream project. While this can be done easily using a `REUSE.toml` file, a full REUSE compliance is not mandatory for FMS.
- CI/CD and Open Source Compliance: Development teams may integrate checks to ensure `FORK.yaml` is present and correctly formatted. The `FORK.yaml` and related files (such as `README.md` and `CHANGELOG.md`) should be part of software packages that are built from the fork.

### Example Repository

```
myfork/
├── FORK.yaml          # Fork metadata
├── LICENSE            # Original project’s license
├── README.md          # Includes fork information
├── LICENSES/          # Optional: Additional licenses
├── src/               # Source files with SPDX headers (keeping the original file/directory structure)
└── CHANGELOG.md       # Optional: Documents changes
```
