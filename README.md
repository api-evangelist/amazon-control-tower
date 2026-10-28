# Amazon Control Tower

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

AWS Control Tower provides the easiest way to set up and govern a secure, multi-account AWS environment based on best practices. It establishes a landing zone with pre-configured governance and guardrails, enabling organizations to maintain compliance and manage accounts at scale. With over 750 preconfigured controls, it automates account creation, OU registration, and compliance enforcement across the entire AWS organization.

**Areas:** AWS · Compliance · Governance · Landing Zone · Multi-Account · Security · Control

## Amazon Control Tower on APIs.io

Everything profiled for Amazon Control Tower is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/amazon-control-tower/](https://apis.io/providers/amazon-control-tower/)** — or go straight to one of the APIs or properties below.

### APIs (4)

- [Amazon Control Tower Baselines API](https://apis.io/apis/amazon-control-tower/amazon-control-tower-baselines-api/) — Operations for applying and managing baselines on organizational units
- [Amazon Control Tower Controls API](https://apis.io/apis/amazon-control-tower/amazon-control-tower-controls-api/) — Operations for enabling, disabling, and managing guardrail controls on organizational units
- [Amazon Control Tower Landing Zones API](https://apis.io/apis/amazon-control-tower/amazon-control-tower-landing-zones-api/) — Operations for managing AWS Control Tower landing zones
- [Amazon Control Tower Tags API](https://apis.io/apis/amazon-control-tower/amazon-control-tower-tags-api/) — Operations for tagging AWS Control Tower resources

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### Arazzo workflows (7)

Multi-step workflows that chain operations into a business task.

- [AWS Control Tower Create Landing Zone and Confirm](https://apis.io/arazzos/amazon-control-tower/amazon-control-tower-create-landing-zone-workflow/)
- [AWS Control Tower Disable Control and Confirm](https://apis.io/arazzos/amazon-control-tower/amazon-control-tower-disable-control-workflow/)
- [AWS Control Tower Enable Baseline and Confirm](https://apis.io/arazzos/amazon-control-tower/amazon-control-tower-enable-baseline-workflow/)
- [AWS Control Tower Enable Control and Confirm](https://apis.io/arazzos/amazon-control-tower/amazon-control-tower-enable-control-workflow/)
- [AWS Control Tower Update Enabled Baseline and Confirm](https://apis.io/arazzos/amazon-control-tower/amazon-control-tower-update-enabled-baseline-workflow/)
- [AWS Control Tower Update Enabled Control and Confirm](https://apis.io/arazzos/amazon-control-tower/amazon-control-tower-update-enabled-control-workflow/)
- [AWS Control Tower Update Landing Zone and Confirm](https://apis.io/arazzos/amazon-control-tower/amazon-control-tower-update-landing-zone-workflow/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Amazon Control Tower Agentic Access](https://apis.io/agentic-access/amazon-control-tower/amazon-control-tower-agentic-access/)

#### JSON Schema (43)

The data models the API reads and writes.

- [Baseline](https://apis.io/schemas/amazon-control-tower/baseline/)
- [BaselineOperation](https://apis.io/schemas/amazon-control-tower/baseline-operation/)
- [ControlOperation](https://apis.io/schemas/amazon-control-tower/control-operation/)
- [ControlOperationSummary](https://apis.io/schemas/amazon-control-tower/control-operation-summary/)
- [CreateLandingZoneRequest](https://apis.io/schemas/amazon-control-tower/create-landing-zone-request/)
- [CreateLandingZoneResponse](https://apis.io/schemas/amazon-control-tower/create-landing-zone-response/)
- [DeleteLandingZoneResponse](https://apis.io/schemas/amazon-control-tower/delete-landing-zone-response/)
- [DisableBaselineResponse](https://apis.io/schemas/amazon-control-tower/disable-baseline-response/)
- [DisableControlResponse](https://apis.io/schemas/amazon-control-tower/disable-control-response/)
- [EnableBaselineRequest](https://apis.io/schemas/amazon-control-tower/enable-baseline-request/)
- [EnableBaselineResponse](https://apis.io/schemas/amazon-control-tower/enable-baseline-response/)
- [EnableControlRequest](https://apis.io/schemas/amazon-control-tower/enable-control-request/)
- [EnableControlResponse](https://apis.io/schemas/amazon-control-tower/enable-control-response/)
- [EnabledBaseline](https://apis.io/schemas/amazon-control-tower/enabled-baseline/)
- [EnabledBaselineParameter](https://apis.io/schemas/amazon-control-tower/enabled-baseline-parameter/)
- [EnabledBaselineSummary](https://apis.io/schemas/amazon-control-tower/enabled-baseline-summary/)
- [EnabledControl](https://apis.io/schemas/amazon-control-tower/enabled-control/)
- [EnabledControlParameter](https://apis.io/schemas/amazon-control-tower/enabled-control-parameter/)
- [EnabledControlSummary](https://apis.io/schemas/amazon-control-tower/enabled-control-summary/)
- [GetBaselineOperationResponse](https://apis.io/schemas/amazon-control-tower/get-baseline-operation-response/)
- [GetBaselineResponse](https://apis.io/schemas/amazon-control-tower/get-baseline-response/)
- [GetControlOperationResponse](https://apis.io/schemas/amazon-control-tower/get-control-operation-response/)
- [GetEnabledBaselineResponse](https://apis.io/schemas/amazon-control-tower/get-enabled-baseline-response/)
- [GetEnabledControlResponse](https://apis.io/schemas/amazon-control-tower/get-enabled-control-response/)
- [GetLandingZoneOperationResponse](https://apis.io/schemas/amazon-control-tower/get-landing-zone-operation-response/)
- …and 18 more published on APIs.io

#### JSON Structure (43)

Data models expressed in JSON Structure.

- [Baseline Operation Structure](https://apis.io/json-structure/amazon-control-tower/baseline-operation-structure/)
- [Baseline Structure](https://apis.io/json-structure/amazon-control-tower/baseline-structure/)
- [Control Operation Structure](https://apis.io/json-structure/amazon-control-tower/control-operation-structure/)
- [Control Operation Summary Structure](https://apis.io/json-structure/amazon-control-tower/control-operation-summary-structure/)
- [Create Landing Zone Request Structure](https://apis.io/json-structure/amazon-control-tower/create-landing-zone-request-structure/)
- [Create Landing Zone Response Structure](https://apis.io/json-structure/amazon-control-tower/create-landing-zone-response-structure/)
- [Delete Landing Zone Response Structure](https://apis.io/json-structure/amazon-control-tower/delete-landing-zone-response-structure/)
- [Disable Baseline Response Structure](https://apis.io/json-structure/amazon-control-tower/disable-baseline-response-structure/)
- [Disable Control Response Structure](https://apis.io/json-structure/amazon-control-tower/disable-control-response-structure/)
- [Enable Baseline Request Structure](https://apis.io/json-structure/amazon-control-tower/enable-baseline-request-structure/)
- [Enable Baseline Response Structure](https://apis.io/json-structure/amazon-control-tower/enable-baseline-response-structure/)
- [Enable Control Request Structure](https://apis.io/json-structure/amazon-control-tower/enable-control-request-structure/)
- [Enable Control Response Structure](https://apis.io/json-structure/amazon-control-tower/enable-control-response-structure/)
- [Enabled Baseline Parameter Structure](https://apis.io/json-structure/amazon-control-tower/enabled-baseline-parameter-structure/)
- [Enabled Baseline Structure](https://apis.io/json-structure/amazon-control-tower/enabled-baseline-structure/)
- [Enabled Baseline Summary Structure](https://apis.io/json-structure/amazon-control-tower/enabled-baseline-summary-structure/)
- [Enabled Control Parameter Structure](https://apis.io/json-structure/amazon-control-tower/enabled-control-parameter-structure/)
- [Enabled Control Structure](https://apis.io/json-structure/amazon-control-tower/enabled-control-structure/)
- [Enabled Control Summary Structure](https://apis.io/json-structure/amazon-control-tower/enabled-control-summary-structure/)
- [Get Baseline Operation Response Structure](https://apis.io/json-structure/amazon-control-tower/get-baseline-operation-response-structure/)
- [Get Baseline Response Structure](https://apis.io/json-structure/amazon-control-tower/get-baseline-response-structure/)
- [Get Control Operation Response Structure](https://apis.io/json-structure/amazon-control-tower/get-control-operation-response-structure/)
- [Get Enabled Baseline Response Structure](https://apis.io/json-structure/amazon-control-tower/get-enabled-baseline-response-structure/)
- [Get Enabled Control Response Structure](https://apis.io/json-structure/amazon-control-tower/get-enabled-control-response-structure/)
- [Get Landing Zone Operation Response Structure](https://apis.io/json-structure/amazon-control-tower/get-landing-zone-operation-response-structure/)
- …and 18 more published on APIs.io

#### JSON-LD (1)

Linked-data contexts for the provider's vocabulary.

- [Amazon Control Tower Context](https://apis.io/jsonld/amazon-control-tower/amazon-control-tower-context/)

#### Vocabulary (1)

The provider's domain terms, defined.

- [AWS Control Tower Vocabulary](https://apis.io/vocabularies/amazon-control-tower/amazon-control-tower-vocabulary/)

#### Examples (43)

Example requests and responses.

- [Baseline Example](https://apis.io/examples/amazon-control-tower/baseline-example/)
- [Baseline Operation Example](https://apis.io/examples/amazon-control-tower/baseline-operation-example/)
- [Control Operation Example](https://apis.io/examples/amazon-control-tower/control-operation-example/)
- [Control Operation Summary Example](https://apis.io/examples/amazon-control-tower/control-operation-summary-example/)
- [Create Landing Zone Request Example](https://apis.io/examples/amazon-control-tower/create-landing-zone-request-example/)
- [Create Landing Zone Response Example](https://apis.io/examples/amazon-control-tower/create-landing-zone-response-example/)
- [Delete Landing Zone Response Example](https://apis.io/examples/amazon-control-tower/delete-landing-zone-response-example/)
- [Disable Baseline Response Example](https://apis.io/examples/amazon-control-tower/disable-baseline-response-example/)
- [Disable Control Response Example](https://apis.io/examples/amazon-control-tower/disable-control-response-example/)
- [Enable Baseline Request Example](https://apis.io/examples/amazon-control-tower/enable-baseline-request-example/)
- [Enable Baseline Response Example](https://apis.io/examples/amazon-control-tower/enable-baseline-response-example/)
- [Enable Control Request Example](https://apis.io/examples/amazon-control-tower/enable-control-request-example/)
- [Enable Control Response Example](https://apis.io/examples/amazon-control-tower/enable-control-response-example/)
- [Enabled Baseline Example](https://apis.io/examples/amazon-control-tower/enabled-baseline-example/)
- [Enabled Baseline Parameter Example](https://apis.io/examples/amazon-control-tower/enabled-baseline-parameter-example/)
- [Enabled Baseline Summary Example](https://apis.io/examples/amazon-control-tower/enabled-baseline-summary-example/)
- [Enabled Control Example](https://apis.io/examples/amazon-control-tower/enabled-control-example/)
- [Enabled Control Parameter Example](https://apis.io/examples/amazon-control-tower/enabled-control-parameter-example/)
- [Enabled Control Summary Example](https://apis.io/examples/amazon-control-tower/enabled-control-summary-example/)
- [Get Baseline Operation Response Example](https://apis.io/examples/amazon-control-tower/get-baseline-operation-response-example/)
- [Get Baseline Response Example](https://apis.io/examples/amazon-control-tower/get-baseline-response-example/)
- [Get Control Operation Response Example](https://apis.io/examples/amazon-control-tower/get-control-operation-response-example/)
- [Get Enabled Baseline Response Example](https://apis.io/examples/amazon-control-tower/get-enabled-baseline-response-example/)
- [Get Enabled Control Response Example](https://apis.io/examples/amazon-control-tower/get-enabled-control-response-example/)
- [Get Landing Zone Operation Response Example](https://apis.io/examples/amazon-control-tower/get-landing-zone-operation-response-example/)
- …and 18 more published on APIs.io

#### Collections (6)

Postman and Open Collection request collections.

- [AWS Control Tower API](https://apis.io/collections/amazon-control-tower/open-amazon-control-tower/)
- [AWS Control Tower Baselines API](https://apis.io/collections/amazon-control-tower/open-amazon-control-tower-baselines-api/)
- [AWS Control Tower Baselines Controls API](https://apis.io/collections/amazon-control-tower/open-amazon-control-tower-controls-api/)
- [AWS Control Tower Baselines Landing Zones API](https://apis.io/collections/amazon-control-tower/open-amazon-control-tower-landing-zones-api/)
- [AWS Control Tower Baselines Tags API](https://apis.io/collections/amazon-control-tower/open-amazon-control-tower-tags-api/)
- [AWS Control Tower API](https://apis.io/collections/amazon-control-tower/postman-amazon-control-tower/)

#### Governance rules (2)

Spectral rulesets for the provider's contracts.

- [Amazon Control Tower — jsonschema rules](https://apis.io/rules/amazon-control-tower/amazon-control-tower-jsonschema-spectral-rules/)
- [Amazon Control Tower API Rules](https://apis.io/rules/amazon-control-tower/amazon-control-tower-spectral-rules/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Amazon Control Tower Authentication](https://apis.io/security/amazon-control-tower/amazon-control-tower-authentication/)
- [Amazon Control Tower Domain Security](https://apis.io/security/amazon-control-tower/amazon-control-tower-domain-security/)
- [Amazon Control Tower Trust Center](https://apis.io/security/amazon-control-tower/amazon-control-tower-trust-center/)
- [Amazon Control Tower Vulnerability Disclosure](https://apis.io/security/amazon-control-tower/amazon-control-tower-vulnerability-disclosure/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Amazon Control Tower Plans Pricing](https://apis.io/plans/amazon-control-tower/amazon-control-tower-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Amazon Control Tower Rate Limits](https://apis.io/rate-limits/amazon-control-tower/amazon-control-tower-rate-limits/)

#### FinOps (1)

What the API costs to run, by operation.

- [Amazon Control Tower Finops](https://apis.io/finops/amazon-control-tower/amazon-control-tower-finops/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://aws.amazon.com/controltower/)
- [Documentation](https://docs.aws.amazon.com/controltower/)
- [Pricing](https://aws.amazon.com/controltower/pricing/)
- [Support](https://aws.amazon.com/premiumsupport/)
- [Blog](https://aws.amazon.com/blogs/mt/category/management-tools/aws-control-tower/)
- [Terms of service](https://aws.amazon.com/service-terms/)
- [Privacy policy](https://aws.amazon.com/privacy/)
- [GitHub](https://github.com/aws)
- [Status](https://health.aws.amazon.com/health/status)

## API Evangelist rating

**Kin Score 46.0** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 71.4 |
| Contract quality | 27.9 |
| Contract governance | 27.3 |
| Operational transparency | 36.8 |
| Developer ergonomics | 53.6 |
| Access clarity | 53.9 |

**Agent readiness 30.2** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | verified |
| Rate Limit Signal | derived (partial credit) |
| Event Surface Described | — |
| Agent Skills | — |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 35.3** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/amazon-control-tower/](https://apis.io/providers/amazon-control-tower/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Amazon Control Tower: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/amazon-control-tower/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
