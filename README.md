# ISO/IEC 27560-1: Universal Notice Receipt Profile

**Convention 108+ Code of Conduct | Transparency-by-Default Architecture**

[![License: OPN RF-RAND](https://img.shields.io/badge/License-OPN_RF--RAND-blue.svg)]([LICENSE.md](http://LICENSE.md))

[![Status: Working Draft](https://img.shields.io/badge/Status-WORKING_DRAFT-yellow.svg)](spec/v1.1/)

[![Version: 1.1 Draft](https://img.shields.io/badge/Version-1.1_DRAFT-orange.svg)]([CHANGELOG.md](http://CHANGELOG.md))

> **⚠️ WORKING DRAFT**: v1.1 is under active development (Q1-Q4 2026). For stable implementation, use [v1.01](spec/v1.01/).
> 

## Overview

ISO/IEC 27560-1 Universal Notice Receipt Profile provides Digital Public Transparency Infrastructure (DPTI) enabling:

- **Controller-ID First**: Machine-readable Controller Identification Records (CIR)
- **Bilateral Receipts**: Notice Receipts held by both controller and individual
- **Four Assurance Levels**: Self-assertion → Registry verification → DTTO notarization → Active state
- **Cross-Border Coordination**: Convention 108+ treaty framework (55+ countries)
- **Privacy-Enabling Architecture**: Individuals control data sharing; organizations use data with authorization

### What's New in v1.1 (Draft)

- ✅ **Privacy Preference Signals** (GPC, DNT integration)
- ✅ **Digital Transparency Risk Assurance Levels** (blind data notary, DTTO certification)
- ✅ **Global Privacy Rights Controls** (72 transparency contexts, automated APIs)
- ✅ **OAuth/UMA Integration** (authorization server alignment)
- ✅ **Consent Token Specifications** (JWT, CBOR, Verifiable Credentials)
- ✅ **AI System Transparency** (ISO/IEC 42001 coordination)
- ✅ **Private-Personal-AI** (Appendix K: individual-controlled model training)


## Implementation Status

| Feature | v1.01 | v1.1 Draft | Target |
| --- | --- | --- | --- |
| Controller Identity Record (CIR) | ✅ Stable | ✅ Enhanced | v1.01 |
| Notice Receipts (Stages 1-4) | ✅ Stable | ✅ Enhanced | v1.01 |
| Notice Event Log | ✅ Stable | ✅ Enhanced | v1.01 |
| Privacy Preference Signals | ❌ | ✅ Draft | Q1 2026 |
| TATA Levels 1-4 | ⚠️ Partial L1 | ✅ Draft | Q3 2026 |
| GPRC (72 Contexts) | ❌ | ✅ Draft | Q3 2026 |
| OAuth/UMA Integration | ❌ | ✅ Draft | Q2 2026 |
| Consent Tokens | ⚠️ Basic | ✅ Draft | Q2 2026 |
| AI System Transparency | ❌ | ✅ Draft | Q3 2026 |
| Reference Implementations | ⚠️ Python | 🚧 Multi-lang | Q1 2026 |

## Conformance Levels

### v1.01 (Stable)

- **Universal Notice Receipt Profile Conformance**: Bilateral receipts, CIR publication, Notice Event Log

### v1.1 (Draft - Q1-Q4 2026)

- **Enhanced Privacy Preference Conformance**: GPC signal handling within 48 hours
- **TATA Level 3+ Conformance**: DTTO certification or Active State API
- **GPRC Conformance**: Automated rights APIs across 72 contexts
- **AI System Transparency Conformance**: ISO/IEC 42001 coordination

## Architecture Principles

### Privacy-Enabling vs. Privacy-Preserving

|  | **Privacy-Preserving** | **Privacy-Enabling** (This Spec) |
| --- | --- | --- |
| **Control** | Organizations protect data | **Individuals control sharing** |
| **Architecture** | Encryption, anonymization | **Transparency-by-Default** |
| **Pattern** | "Trust us" (surveillance) | **"I choose to trust" (sousveillance)** |
| **Enforcement** | Post-hoc fines | **Preventive verification** |

### Controller-ID First

```
Traditional (Surveillance):
	Cookie deployed → Banner shown → "Consent" clicked → Identity obscured

Transparency-by-Default (This Spec):
	CIR published → Individual reviews → Authorization granted → Processing begins
```

## Related 0PN Lab Projects (work in Progress)

- 🧪 [**TPI-R Methodology**](https://github.com/0pn-lab/tpi-methodology): Transparency Performance Indicators (regulatory-grade)
- 🤖 [**0PN-AI**](https://github.com/0pn-lab/0pn-ai): Controller-side transparency machine (Q1-Q2 2026)
- 🔐 [**Priv8AI**](https://github.com/0pn-lab/priv8ai): Personal-side PII Principal AI (Q2-Q4 2026)
- 📦 [**Implementation Guides**](https://github.com/0pn-lab/implementation-guides): Project Kits for tiered rollout
- 🌐 [**ISO/IEC 27560-2**](https://github.com/0pn-lab/iso-27560-2-clean-ai): Clean-AI training data provenance (future)

## Contributing

0PN Lab welcomes feedback from:

- **Standards Bodies**: ISO/IEC JTC 1/SC 27 WG5, W3C, IETF, Kantara
- **Regulators**: Data Protection Authorities, Convention 108 Committee
- **Implementers**: Controllers, privacy officers, developers
- **Researchers**: Privacy, security, AI governance

See [CONTRIBUTING.md](http://CONTRIBUTING.md) for guidelines.

## Development Timeline (v1.1)

- **Q1 2026**: Privacy Preference Signal Pilot (GPC integration)
- **Q2 2026**: TATA Level 3 Protocols (DTTO certification, blind data notary)
- **Q3 2026**: GPRC Framework + Automated Rights APIs
- **Q4 2026**: Full v1.1 Integration + Certification Program

## License

This specification is published under the **OPN RF-RAND IPR License**.

See [LICENSE.md](http://LICENSE.md) for full terms.

**Summary**: Royalty-free for implementation; reasonable and non-discriminatory terms for patents.

## About 0PN Lab

**0PN Lab** (Zero spoken as 0pen-Public-Notice Lab) develops Digital Public Transparency Infrastructure (DPTI) as regulatory capacity infrastructure.

- 🌐 **Blog**: [lab.0pn.org](http://lab.0pn.org)
- 🔬 **Website**: [transparencylab.ca](http://transparencylab.ca)
- 📧 **Contact**: [standards@0pn.org](mailto:standards@0pn.org)
- 💬 **Community**: [GitHub Discussions](https://github.com/0pn-lab/iso-27560-unrp/discussions)

**Author**: Mark Lizar, @)0PN Digital Transparency Lab  - A Global Privacy Rights Company

**Chair, Convention 108 Committee Keynote**: [Beatriz de Anchorena](https://0pn.org/events/international-privacy-day-2026)

---

*Published by 0PN Lab | Convention 108+ Code of Conduct | Transparency-by-Default Architecture*
