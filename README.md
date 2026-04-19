# Amazon Control Tower

AWS Control Tower provides the easiest way to set up and govern a secure, multi-account AWS environment based on best practices. It establishes a landing zone with pre-configured governance and guardrails, enabling organizations to maintain compliance and manage accounts at scale. With over 750 preconfigured controls, it automates account creation, OU registration, and compliance enforcement across the entire AWS organization.

## APIs

### AWS Control Tower API

The AWS Control Tower API provides programmatic access to manage landing zones, organizational units, accounts, controls (guardrails), and baselines within your AWS environment, enabling automated governance at scale. Supports operations for landing zone lifecycle, control enablement/disablement, and OU baseline registration.

- **Base URL:** https://controltower.amazonaws.com
- **Documentation:** https://docs.aws.amazon.com/controltower/latest/APIReference/Welcome.html
- **OpenAPI:** [openapi/amazon-control-tower-openapi.yml](openapi/amazon-control-tower-openapi.yml)

**Tags:** Governance, Landing Zone, Multi-Account, Controls, Baselines

## Artifacts

| Type | Count | Location |
|------|-------|----------|
| OpenAPI Specs | 1 | [openapi/](openapi/) |
| JSON Schemas | 43 | [json-schema/](json-schema/) |
| JSON Structures | 43 | [json-structure/](json-structure/) |
| Examples | 43 | [examples/](examples/) |
| JSON-LD Contexts | 1 | [json-ld/](json-ld/) |
| Spectral Rules | 1 | [rules/](rules/) |
| Vocabulary | 1 | [vocabulary/](vocabulary/) |
| Naftiko Capabilities | 2 | [capabilities/](capabilities/) |

## Features

- **Landing Zone Management** — Create, configure, update, reset, and delete AWS Control Tower landing zones programmatically via API, automating multi-account environment setup.
- **Controls (Guardrails) Library** — Over 750 preconfigured controls (guardrails) covering security, operations, and compliance. Enable or disable controls on organizational units via API.
- **Baseline Registration** — Apply and manage baselines on organizational units (OUs) to register them with AWS Control Tower and enforce standard configurations programmatically.
- **Multi-Account Governance** — Automate creation of AWS accounts with built-in governance, policies, and security controls through integration with AWS Organizations.
- **Compliance Enforcement** — Deploy preventive, detective, and proactive controls to enforce compliance standards including CIS, NIST, PCI-DSS, HIPAA, and SOC 2.
- **Audit and Logging** — Centralized audit logging to Amazon S3 and AWS CloudTrail integration for full visibility into API calls and governance actions.
- **Third-Party Integrations** — Seamlessly integrate third-party security, compliance, and ITSM tools at scale to enhance your AWS multi-account environment.

## Use Cases

- **Multi-Account Environment Setup** — Quickly set up a secure, well-architected multi-account AWS environment with landing zone configuration completed in under 30 minutes.
- **Compliance Automation** — Deploy preconfigured controls to enforce regulatory compliance standards such as PCI-DSS, HIPAA, NIST, and SOC 2 across all accounts.
- **Account Vending** — Automate provisioning of new AWS accounts with built-in security policies, IAM roles, and governance configurations using Account Factory.
- **OU Governance** — Programmatically register organizational units with Control Tower baselines and apply targeted controls for department-specific governance.
- **Risk and Posture Management** — Continuously monitor compliance posture across all accounts and receive alerts when controls are violated or drift is detected.

## Integrations

- **AWS Organizations** — Native integration with AWS Organizations for multi-account structure, OU management, and account creation within a Control Tower landing zone.
- **AWS Service Catalog** — Account Factory integration through AWS Service Catalog for self-service account provisioning with pre-approved configurations.
- **AWS CloudTrail** — All Control Tower API calls are logged to AWS CloudTrail for audit trails, security investigations, and compliance reporting.
- **AWS Config** — Detective controls are implemented using AWS Config rules to continuously evaluate resource compliance within managed accounts.
- **AWS Security Hub** — Integrate Control Tower findings with AWS Security Hub for centralized security posture management and cross-account visibility.
- **AWS CloudFormation** — Launch landing zones and enable controls using CloudFormation templates and resource providers for infrastructure-as-code governance.
- **Terraform** — Community-supported Terraform providers for managing Control Tower landing zones, controls, and account factory configurations.

## Resources

- [Portal](https://aws.amazon.com/controltower/)
- [Documentation](https://docs.aws.amazon.com/controltower/)
- [Getting Started](https://aws.amazon.com/controltower/getting-started/)
- [Pricing](https://aws.amazon.com/controltower/pricing/)
- [FAQ](https://aws.amazon.com/controltower/faqs/)
- [Blog](https://aws.amazon.com/blogs/mt/category/management-tools/aws-control-tower/)
- [GitHub Organization](https://github.com/aws)
- [Console](https://console.aws.amazon.com/controltower/)
- [Support](https://aws.amazon.com/premiumsupport/)
- [Terms of Service](https://aws.amazon.com/service-terms/)
- [Privacy Policy](https://aws.amazon.com/privacy/)

## Maintainers

- **Kin Lane** — kin@apievangelist.com
