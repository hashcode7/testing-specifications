<!--
Sync Impact Report

- Version change: unset -> 0.1.0
- Modified principles:
	- [PRINCIPLE_1_NAME] (Library-First) -> Library-First
	- [PRINCIPLE_2_NAME] (Agent-Driven Integrations) -> Agent-Driven Integrations
	- [PRINCIPLE_3_NAME] (Test-First) -> Test-First (NON-NEGOTIABLE)
	- [PRINCIPLE_4_NAME] (Reproducible Environments) -> Reproducible Environments & Config
	- [PRINCIPLE_5_NAME] (Security & Privacy) -> Security, Privacy & Compliance
- Added sections: Technology Constraints, Development Workflow
- Removed sections: none
- Templates requiring updates:
	- .specify/templates/plan-template.md ⚠ pending
	- .specify/templates/spec-template.md ⚠ pending
	- .specify/templates/tasks-template.md ⚠ pending
	- .specify/templates/commands/*.md ⚠ pending
- Follow-up TODOs:
	- TODO(RATIFICATION_DATE): original adoption date not recorded; please supply.
-->

# Speckit Testing Specifications Constitution

## Core Principles

### Library-First
All core functionality MUST be implemented as small, well-documented, and independently testable libraries. Libraries MUST expose clear, minimal public APIs and include automated unit tests. Rationale: modular, reusable components improve testability and reduce coupling.

### Agent-Driven Integrations
Agent implementation and integrations MUST follow an agent-first design: use the `langchain` Python framework for agent orchestration and decision logic. The system MUST be provider-agnostic: primary LLM integrations target AWS LLM services, and local LLM support via Ollama MUST be configurable. Rationale: standardizing on `langchain` ensures consistent agent patterns while preserving flexibility for providers.

### Test-First (NON-NEGOTIABLE)
Testing is mandatory and MUST follow a red-green-refactor workflow. New features or changes MUST include tests before implementation: unit tests for libraries, integration tests for agent/provider interactions, and end-to-end tests for user-facing behaviors. Rationale: prevents regressions and documents expected behavior.

### Reproducible Environments & Config
All developer and CI environments MUST be reproducible. Use containerization (Docker) or pinned virtual environments. Configuration for LLM providers MUST be explicit and separated from code: support an `aws` provider configuration and an `ollama` provider configuration toggled via environment or config file. Secrets MUST be stored in secure secret stores (e.g., AWS Secrets Manager) and never committed. Rationale: reproducibility reduces "works on my machine" issues and enables reliable CI.

### Security, Privacy & Compliance
Data handling MUST minimize sensitive data sent to LLMs. Personally identifiable information (PII) MUST be redacted or explicitly authorized. Access controls and audit logging MUST be enforced for any production LLM keys or datasets. Rationale: protect user data and comply with regulatory constraints.

## Technology Constraints

This project targets Python 3.10+ and the `langchain` framework for agent development. LLM provider support:
- Primary cloud provider: AWS LLM services (configured via `AWS_*` environment variables or config files).
- Optional local provider: Ollama (configured via `OLLAMA_HOST`/`OLLAMA_PORT` or local socket). The codebase MUST include a clear provider abstraction allowing runtime selection.

Dependency management MUST use `pyproject.toml`/Poetry or `requirements.txt` with pinned versions for CI reproducibility. Container images used by CI MUST pin base image digests.

## Development Workflow

- All changes MUST be made via pull requests with at least one approving reviewer who is a project maintainer.
- CI pipelines MUST run unit tests, integration tests (including an isolated LLM provider mock), linting, and security scans before merge.
- Feature branches MUST reference an issue and include a changelog entry when behavior changes.
- Commits MUST follow the ` Conventional Commits ` style to enable automated changelogs.

## Governance

Amendments to this constitution require a documented proposal (a PR) that describes the change, the rationale, and the migration plan. A change MUST be approved by at least two maintainers. The maintainer group MAY appoint a steward to adjudicate disputes.

Versioning policy:
- Follow semantic versioning for the constitution: MAJOR for incompatible governance changes, MINOR for new principles or material additions, PATCH for editorial clarifications.

Compliance checks:
- New PRs MUST reference relevant constitution principles. The CI pipeline SHOULD flag non-compliant changes and require remediation before merge.

**Version**: 0.1.0 | **Ratified**: TODO(RATIFICATION_DATE): original adoption date unknown | **Last Amended**: 2026-03-06
