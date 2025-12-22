# Contributing to ISO/IEC 27560 Universal Notice Receipt Profile

Thank you for your interest in contributing to the Universal Notice Receipt Profile (UNRP) specification.

## Overview

This repository contains the ISO/IEC 27560:2025 Universal Notice Receipt Profile v1.0, which completes the Minimum Viable Consent Receipt (MVCR) work under the Anchored Notice and Consent Receipt (ANCR) framework. The profile implements a "controller-ID first, anonymous-by-default" architecture enabling regulatory capacity infrastructure for enforcement at scale.

## Participation Framework

Contributions to this specification operate under the **OPN RF-RAND IPR License** (Reciprocal Royalty-Free with opt-out to Reasonable And Non-Discriminatory terms).

### Key Principles

- **Default Royalty-Free:** All contributions are licensed RF by default
- **Reciprocity Option:** Contributors may require reciprocal licensing
- **RAND Opt-Out:** Specific claims may be withdrawn to RAND during IPR review periods
- **Transparency by Default:** Glass-box development aligned with profile architecture

## Ways to Contribute

### 1. Review and Feedback

- Open GitHub Issues for technical questions, clarifications, or suggested improvements
- Reference specific sections using the document structure in `/specification/`
- Cite relevant regulatory frameworks (Convention 108+, GDPR Article 12/13/14, EU 1725, etc.)

### 2. Implementation Experience

- Share implementation reports, conformance test results, or deployment feedback
- Submit examples demonstrating Controller Identification Records (CIR), notice.txt, or ANCR flows
- Document interoperability with GPC, TPI-R, or /.well-known/transparency endpoints

### 3. Schema and Reference Code

- `/schemas/` improvements (JSON Schema, CDDL, etc.)
- `/reference-impl/` code contributions (licensed Apache 2.0)
- `/conformance/` test cases and validation tools

### 4. Regulatory Mapping

- Cross-reference with national privacy laws (PIPEDA, Privacy Act 1988, CCPA, etc.)
- Guidance for Commonwealth coordination or cross-border transfer scenarios
- Data Protection Officer (DPO) certification and implementation playbooks

## Contribution Process

### Before Contributing

1. **Review the General Participation Agreement** (`[OPN-GPA.md](http://OPN-GPA.md)`) and IPR License (`LICENSE`)
2. **Search existing Issues** to avoid duplicate submissions
3. **Understand the Three Vectors framework:**
   - **Personal Control** (individual self-identifies, SSI/DID)
   - **Data Protection** (federated identification)
   - **Co-Regulation** (MVCR-ANCR standardized notice-and-consent with enforcement)

### Submitting Contributions

1. **Open an Issue** describing your proposed change:
   - Technical rationale and use case
   - Impact on existing implementations
   - Relevant regulatory or standards alignment

2. **Fork and create a Pull Request:**
   - Clear description linking to the Issue
   - Reference specification sections affected
   - Indicate if contribution includes Essential Claims requiring IPR disclosure

3. **IPR Disclosure:**
   - Contributors personally aware of Essential Claims must disclose during IPR review periods
   - No obligation to conduct patent searches
   - See `[OPN-GPA.md](http://OPN-GPA.md)` for Necessary Claims procedures

### Code Contributions (Reference Implementation)

Reference implementation code in `/reference-impl/` is licensed **Apache 2.0**:

- Include copyright header: `Copyright 2025 Digital Transparency Lab`
- Follow existing code structure and documentation style
- Include unit tests and conformance validation

## Terminology Precision

Critical distinctions in this framework:

- **Controller Identification Record (CIR)** — NOT "Controller Identity Record." Identification = standardized discovery; Identity = individual self-definition
- **Privacy-enabling data control** — NOT "privacy-preserving data protection." Individual control without forced identification, not corporate data safeguarding
- **Notice and Consent Receipt** — Bilateral proof of notice (2FN), not unilateral privacy policy
- **Transparency and Trust Assurance (TATA) Levels:**
  - L1: Self-assertion
  - L2: Registry verification
  - L3: Blind data notary (ANCR)
  - L4: Active state high assurance (certified DTTO)

## Governance and Coordination

- **Administered by:** Digital Transparency Lab (DTL)
- **Aligned with:** Kantara Initiative ANCR WG, ISO/IEC JTC 1/SC 27/WG 5
- **Regulatory coordination:** Global Digital Transparency Alliance (GDTA) founding work
- **Related work:** IETF Internet-Draft for `/.well-known/transparency` URI registration

## Communication Channels

- **GitHub Issues:** Technical questions, feedback, and proposed changes
- **DTL Contact:** For IPR questions, participation agreements, or certification programs
- **GDTA Membership:** {{853}}

## Code of Conduct

Contributors operate under:

- **Convention 108+ normative framework:** Treating treaty-level data protection obligations as code-of-conduct basis
- **Evidence-based technical decisions:** Logical reasoning, systematic analysis, regulatory alignment
- **Glass-box transparency:** Development processes mirror architectural principles

## License Summary

- **Specifications** (`/specification/`, `/schemas/`): OPN RF-RAND IPR License
- **Reference implementation** (`/reference-impl/`): Apache 2.0
- **Documentation**: Creative Commons Attribution 4.0 International (CC BY 4.0)

---

**Questions?** Open a GitHub Issue or contact Digital Transparency Lab.

**Thank you** for contributing to regulatory capacity infrastructure enabling individual data control at scale.
