# Convention 108+ DPV Extension Specification

**Version**: 0.1 (Draft)

**Date**: 2025-12-29

**Status**: Working Draft

**Editor**: Mark Lizar, Digital Transparency Lab

**Contributors**: Mark Lizar
**Turtle Vocabulary**: [Sample](https://github.com/Digital-Transparency-Lab/iso27560-1_unrp/blob/9794fb9b9b61cc0db8a778af8be8688807e09902/specifications/Convention%20108%2B%20DPV%20Extension%20-%20RDF/Turtle%20Vocabulary%20-%20Sample)

---

## Abstract

The Convention 108+ extension (`conv108`) extends the W3C Data Privacy Vocabulary (DPV) to provide concepts for the Council of Europe's Convention 108+ (Modernised Convention for the Protection of Individuals with regard to the Processing of Personal Data). This extension enables machine-readable representation of Convention 108+ legal bases, rights, principles, and transparency requirements, providing semantic interoperability between Convention 108+, ISO/IEC 27560-1 Universal Notice Receipt Profile, and DPCat cataloging infrastructure.

**Namespace**: [`https://w3id.org/dpv/legal/coe/convention108plus#`](https://w3id.org/dpv/legal/coe/convention108plus#)

**Prefix**: `conv108`

**Normative References**: Convention 108+ (CoE Treaty 223), EU Regulation 2018/1725, ISO/IEC 27560-1

---

## 1. Introduction

Convention 108+ is a ratifiable international treaty (55+ jurisdictions) providing universal data protection principles. Unlike GDPR (EU/EEA regional regulation), Convention 108+ serves as the normative framework for ISO/IEC 27560-1 Universal Notice Receipt Profile and enables "Commonwealth Effect" coordinated enforcement.

**Key Distinctions from GDPR**:

- **Scope**: International treaty vs regional regulation
- **Architecture**: Controller-ID first (Article 8.2) vs processing-concurrent transparency
- **Implementation**: Technical standards via Article 11 code of conduct provisions
- **Infrastructure**: Public registries (EU 2018/1725 Article 31) and logging (Article 88)

**Integration Points**:

- **ISO/IEC 27560-1**: CIR fields, notice event types, legal basis codes
- **DPCat**: Semantic cataloging of processing activities
- **W3C DPV**: Interoperability with GDPR and other legal extensions

---

## 2. Legal Bases

### 2.1 Core Legal Bases (Article 5.2)

Convention 108+ Article 5.2 defines six legal bases for processing personal data.

### A5-2-a: Consent

**URI**: `conv108:A5-2-a`

**Type**: `dpv:LegalBasis`

**Label**: Consent

**Definition**: Legal basis where the individual has freely given specific, informed consent to the processing of their personal data for specified purposes.

**Source**: Convention 108+ Article 5.2.a

**ISO/IEC 27560-1 Code**: `consent`

**Variants**:

- `conv108:A5-2-a-explicit`: Explicit consent (special category processing)
- `conv108:A5-2-a-informed`: Informed consent with Article 8 transparency
- `conv108:A5-2-a-specific`: Purpose-specific consent

### A5-2-b: Contract

**URI**: `conv108:A5-2-b`

**Type**: `dpv:LegalBasis`

**Label**: Contract

**Definition**: Legal basis where processing is necessary for the performance of a contract to which the individual is party, or to take steps at the request of the individual prior to entering into a contract.

**Source**: Convention 108+ Article 5.2.b

**ISO/IEC 27560-1 Code**: `contract`

**Variants**:

- `conv108:A5-2-b-performance`: Contract performance
- `conv108:A5-2-b-pre-contractual`: Pre-contractual steps

### A5-2-c: Legal Obligation

**URI**: `conv108:A5-2-c`

**Type**: `dpv:LegalBasis`

**Label**: Legal Obligation

**Definition**: Legal basis where processing is required by law to which the controller is subject.

**Source**: Convention 108+ Article 5.2.c

**ISO/IEC 27560-1 Code**: `legal_obligation`

### A5-2-d: Legitimate Interest

**URI**: `conv108:A5-2-d`

**Type**: `dpv:LegalBasis`

**Label**: Legitimate Interest

**Definition**: Legal basis where processing is necessary for the purposes of legitimate interests pursued by the controller or a third party, provided such interests are not overridden by the interests or fundamental rights of the individual.

**Source**: Convention 108+ Article 5.2.d

**ISO/IEC 27560-1 Code**: `legitimate_interest`

**Variants**:

- `conv108:A5-2-d-controller`: Controller's legitimate interest
- `conv108:A5-2-d-third-party`: Third party's legitimate interest

### A5-2-e: Vital Interest

**URI**: `conv108:A5-2-e`

**Type**: `dpv:LegalBasis`

**Label**: Vital Interest

**Definition**: Legal basis where processing is necessary to protect the vital interests of the individual or another person.

**Source**: Convention 108+ Article 5.2.e

**ISO/IEC 27560-1 Code**: `vital_interest`

**Variants**:

- `conv108:A5-2-e-data-subject`: Individual's vital interest
- `conv108:A5-2-e-other-person`: Another person's vital interest

### A5-2-f: Public Interest

**URI**: `conv108:A5-2-f`

**Type**: `dpv:LegalBasis`

**Label**: Public Interest

**Definition**: Legal basis where processing is necessary for the performance of a task carried out in the public interest or in the exercise of official authority.

**Source**: Convention 108+ Article 5.2.f

**ISO/IEC 27560-1 Code**: `public_interest`

**Variants**:

- `conv108:A5-2-f-public-interest`: Public interest task
- `conv108:A5-2-f-official-authority`: Official authority exercise

---

### 2.2 Special Category Legal Bases (Article 6.2)

Convention 108+ Article 6 regulates processing of special categories of personal data (sensitive data).

### A6-2-a through A6-2-j

Special category legal bases parallel GDPR Article 9 structure:

- **A6-2-a**: Explicit consent
- **A6-2-b**: Employment/social security
- **A6-2-c**: Vital interests protection
- **A6-2-d**: Legitimate activities (foundations/associations)
- **A6-2-e**: Data manifestly made public
- **A6-2-f**: Legal claims
- **A6-2-g**: Substantial public interest
- **A6-2-h**: Preventive/occupational medicine
- **A6-2-i**: Public health
- **A6-2-j**: Research/statistics

**Pattern**: Each extends `dpv:LegalBasis` with special category designation

---

### 2.3 Data Transfer Legal Bases (Article 14)

### Transfer with Adequate Safeguards

**URI**: `conv108:A14-adequate-safeguards`

**Type**: `dpv:DataTransferLegalBasis`

**Label**: Transfer with Adequate Safeguards

**Definition**: Legal basis for cross-border transfer where adequate safeguards ensure continued protection of personal data.

**Source**: Convention 108+ Article 14

**Transfer Mechanisms**:

- `conv108:BilateralTreaty`: Bilateral/multilateral treaty
- `conv108:InternalRules`: Legally binding internal rules
- `conv108:SpecificSafeguards`: Specific safeguards (contracts, codes of conduct, certification)

**Novel Requirement**: All transfers require surveillance risk disclosure per ISO/IEC 27560-1 Section 6.1.2

---

## 3. Principles (Article 5.4)

Convention 108+ Article 5.4 establishes processing principles.

### ProportionalityPrinciple

**URI**: `conv108:ProportionalityPrinciple`

**Type**: `dpv:Principle`

**Label**: Proportionality Principle

**Definition**: Personal data must be processed in a manner that is proportionate to the legitimate purposes pursued.

**Source**: Convention 108+ Article 5.4.a

### PurposeLimitationPrinciple

**URI**: `conv108:PurposeLimitationPrinciple`

**Type**: `dpv:Principle`

**Label**: Purpose Limitation Principle

**Definition**: Personal data must be collected for explicit, specified, and legitimate purposes and not processed in a way incompatible with those purposes.

**Source**: Convention 108+ Article 5.4.b

### DataQualityPrinciple

**URI**: `conv108:DataQualityPrinciple`

**Type**: `dpv:Principle`

**Label**: Data Quality Principle

**Definition**: Personal data must be adequate, relevant, and not excessive in relation to purposes; accurate and kept up to date.

**Source**: Convention 108+ Article 5.4.c, 5.4.d

### RetentionPrinciple

**URI**: `conv108:RetentionPrinciple`

**Type**: `dpv:Principle`

**Label**: Retention Principle

**Definition**: Personal data must be kept in a form permitting identification for no longer than necessary for the purposes.

**Source**: Convention 108+ Article 5.4.e

### SecurityPrinciple

**URI**: `conv108:SecurityPrinciple`

**Type**: `dpv:Principle`

**Label**: Security Principle

**Definition**: Personal data must be protected by appropriate security measures against unauthorized access, alteration, disclosure, or destruction.

**Source**: Convention 108+ Article 7

---

## 4. Rights (Article 9)

Convention 108+ Article 9 consolidates individual rights.

### A9-1-a: Right to Information

**URI**: `conv108:A9-1-a`

**Type**: `dpv:DataSubjectRight`

**Label**: Right to Information

**Definition**: Right to be informed of controller identity, processing purposes, legal basis, recipients, and retention period.

**Source**: Convention 108+ Article 9.1.a

**Parallel**: GDPR Articles 13-14

**Notice Concepts**:

- `conv108:ControllerIdentificationNotice`: Article 8.2 controller disclosure
- `conv108:ProcessingPurposeNotice`: Purpose disclosure
- `conv108:LegalBasisNotice`: Legal basis disclosure

### A9-1-b: Right of Access

**URI**: `conv108:A9-1-b`

**Type**: `dpv:DataSubjectRight`

**Label**: Right of Access

**Definition**: Right to obtain confirmation of processing and access to personal data being processed.

**Source**: Convention 108+ Article 9.1.b

**Parallel**: GDPR Article 15

### A9-1-c: Right to Rectification

**URI**: `conv108:A9-1-c`

**Type**: `dpv:DataSubjectRight`

**Label**: Right to Rectification

**Definition**: Right to obtain rectification of inaccurate personal data without undue delay.

**Source**: Convention 108+ Article 9.1.c

**Parallel**: GDPR Article 16

### A9-1-d: Right to Erasure

**URI**: `conv108:A9-1-d`

**Type**: `dpv:DataSubjectRight`

**Label**: Right to Erasure

**Definition**: Right to obtain erasure of personal data without undue delay where processing is unlawful or no longer necessary.

**Source**: Convention 108+ Article 9.1.d

**Parallel**: GDPR Article 17

### A9-1-e: Right to Restriction

**URI**: `conv108:A9-1-e`

**Type**: `dpv:DataSubjectRight`

**Label**: Right to Restriction of Processing

**Definition**: Right to obtain restriction of processing in specified circumstances.

**Source**: Convention 108+ Article 9.1.e

**Parallel**: GDPR Article 18

### A9-1-f: Right to Object

**URI**: `conv108:A9-1-f`

**Type**: `dpv:DataSubjectRight`

**Label**: Right to Object

**Definition**: Right to object to processing on grounds relating to individual's particular situation.

**Source**: Convention 108+ Article 9.1.f

**Parallel**: GDPR Article 21

### A9-1-g: Right to Data Portability

**URI**: `conv108:A9-1-g`

**Type**: `dpv:DataSubjectRight`

**Label**: Right to Data Portability

**Definition**: Right to receive personal data in structured, commonly used, machine-readable format and transmit to another controller.

**Source**: Convention 108+ Article 9.1.g

**Parallel**: GDPR Article 20

### A9-2: Right to Withdraw Consent

**URI**: `conv108:A9-2`

**Type**: `dpv:DataSubjectRight`

**Label**: Right to Withdraw Consent

**Definition**: Right to withdraw consent at any time without detriment.

**Source**: Convention 108+ Article 9.2

**Parallel**: GDPR Article 7.3

---

## 5. Transparency Requirements (Article 8)

### Article 8.1: Transparency Obligation

**URI**: `conv108:A8-transparency`

**Type**: `dpv:Principle`

**Label**: Transparency Obligation

**Definition**: Obligation to provide clear, accessible information about processing to enable individuals to exercise rights.

**Source**: Convention 108+ Article 8.1

### Article 8.2: Controller Identification Before Processing

**URI**: `conv108:A8-2-controller-identification`

**Type**: `dpv:Principle`

**Label**: Controller Identification Before Processing

**Definition**: Requirement that controller identity and legal basis be disclosed BEFORE personal data processing begins.

**Source**: Convention 108+ Article 8.2

**Novel**: Enables Controller-ID first architecture

**Implementation Concepts**:

### ControllerIdentificationRecord (CIR)

**URI**: `conv108:ControllerIdentificationRecord`

**Type**: `dpv:Record`

**Label**: Controller Identification Record

**Definition**: Machine-readable record of controller identity, processing purposes, legal basis, and contact information, discoverable before processing.

**Source**: Convention 108+ Article 8.2, ISO/IEC 27560-1 Section 5

**Implementation**: `/.well-known/notice.txt`

**Required CIR Fields** (ISO/IEC 27560-1):

- `controller_name`
- `controller_contact`
- `processing_purposes`
- `lawful_basis`
- `data_categories`
- `retention_period`
- `recipients` (if applicable)
- `cross_border_transfers` (if applicable)

### ControllerRegistration

**URI**: `conv108:ControllerRegistration`

**Type**: `dpv:OrganisationalMeasure`

**Label**: Controller Registration

**Definition**: Registration of controller in public registry per EU 2018/1725 Article 31, enabling public transparency.

**Source**: EU Regulation 2018/1725 Article 31

---

## 6. Technical Transparency Mechanisms (Article 11)

### Article 11: Code of Conduct

Convention 108+ Article 11 enables technical transparency standards as codes of conduct.

### TechnicalTransparencyStandard

**URI**: `conv108:TechnicalTransparencyStandard`

**Type**: `dpv:OrganisationalMeasure`

**Label**: Technical Transparency Standard

**Definition**: Code of conduct implementing technical transparency requirements (ISO/IEC 27560-1 profile).

**Source**: Convention 108+ Article 11, ISO/IEC 27560-1

### NoticeReceiptMechanism

**URI**: `conv108:NoticeReceiptMechanism`

**Type**: `dpv:TechnicalMeasure`

**Label**: Notice Receipt Mechanism

**Definition**: Bilateral proof-of-notice mechanism providing cryptographically verifiable receipt to both controller and individual.

**Source**: ISO/IEC 27560-1 Section 6.2 (ANCR/MVCR)

**Implementation**: Anchored Notice & Consent Receipt (ANCR)

### NoticeEventLog

**URI**: `conv108:NoticeEventLog`

**Type**: `dpv:TechnicalMeasure`

**Label**: Notice Event Log

**Definition**: Append-only log recording all transparency state changes (notice issued, consent granted, material change, etc.).

**Source**: EU Regulation 2018/1725 Article 88, ISO/IEC 27560-1 Appendix B

**Event Types** (ISO/IEC 27560-1):

- `notice_issued`
- `consent_granted`
- `consent_withdrawn`
- `material_change`
- `rights_exercised`
- `data_breach`
- `cross_border_transfer`

---

## 7. Derivative Digital Transparency Controls

These controls operationalize Convention 108+ Articles 7-8 through ISO/IEC 27560-1 implementation.

### ControllerIDBeforeInference

**URI**: `conv108:ControllerIDBeforeInference`

**Type**: `dpv:TechnicalMeasure`

**Label**: Controller ID Before Inference

**Definition**: Technical control preventing personal data processing or device identification before controller disclosure.

**Source**: Convention 108+ Article 8.2, ISO/IEC 27560-1 Section 5.3

**Rationale**: Prevents "recursive consent trap" where identification required before transparency

### BilateralNoticeProof

**URI**: `conv108:BilateralNoticeProof`

**Type**: `dpv:TechnicalMeasure`

**Label**: Bilateral Notice Proof

**Definition**: Cryptographically verifiable proof-of-notice provided to both controller and individual with synchronized records.

**Source**: ISO/IEC 27560-1 Section 6.2 (ANCR/MVCR)

**Rationale**: Enables "architecture I choose to trust" vs "trust us" paradigm

### NoticeEventLogAccess

**URI**: `conv108:NoticeEventLogAccess`

**Type**: `dpv:TechnicalMeasure`

**Label**: Notice Event Log Access

**Definition**: Individual access to notice event log showing all transparency state changes for their data.

**Source**: EU Regulation 2018/1725 Article 88, ISO/IEC 27560-1 Appendix B

**Rationale**: Enables verification of transparency obligations

### AutonomousConsentWithdrawal

**URI**: `conv108:AutonomousConsentWithdrawal`

**Type**: `dpv:TechnicalMeasure`

**Label**: Autonomous Consent Withdrawal

**Definition**: Mechanism enabling individuals to withdraw consent autonomously without controller dependency.

**Source**: Convention 108+ Article 9.2, ISO/IEC 27560-1 Section 6.3

**Rationale**: Prevents controller lock-in for consent withdrawal

### ActiveStateTransparency

**URI**: `conv108:ActiveStateTransparency`

**Type**: `dpv:TechnicalMeasure`

**Label**: Active State Transparency

**Definition**: Real-time signaling of processing active state for individual monitoring and control.

**Source**: ISO/IEC 27560-1 TATA Level 4

**Rationale**: Enables real-time transparency for high-assurance contexts

### RightsDerogationNotice

**URI**: `conv108:RightsDerogationNotice`

**Type**: `dpv:Notice`

**Label**: Rights Derogation Notice

**Definition**: Notice requirement when rights are limited or derogated under Convention 108+ Article 9.4.

**Source**: Convention 108+ Article 9.4

### ProcessorTransparencyObligation

**URI**: `conv108:ProcessorTransparencyObligation`

**Type**: `dpv:OrganisationalMeasure`

**Label**: Processor Transparency Obligation

**Definition**: Processor obligation to support controller transparency requirements through technical measures.

**Source**: Convention 108+ Article 7, ISO/IEC 27560-1 Section 5.4

### CrossBorderSurveillanceDisclosure

**URI**: `conv108:CrossBorderSurveillanceDisclosure`

**Type**: `dpv:Notice`

**Label**: Cross-Border Surveillance Disclosure

**Definition**: Requirement to disclose surveillance risks for cross-border transfers.

**Source**: Convention 108+ Article 14, ISO/IEC 27560-1 Section 6.1.2

---

## 8. Compliance Status

### Conv108Lawfulness

**URI**: `conv108:Conv108Lawfulness`

**Type**: `dpv:ComplianceStatus`

**Label**: Convention 108+ Lawfulness

**Definition**: Status indicating lawfulness or compliance with Convention 108+.

**Status Values**:

- `conv108:Conv108Compliant`: Compliant with Convention 108+
- `conv108:Conv108NonCompliant`: Non-compliant with Convention 108+
- `conv108:Conv108ComplianceUnknown`: Compliance status unknown

---

## 9. ISO/IEC 27560-1 Integration

### CIR Field Mappings

| DPV Concept | ISO/IEC 27560-1 CIR Field |
| --- | --- |
| `conv108:A5-2-a` | `lawful_basis: "consent"` |
| `conv108:A5-2-b` | `lawful_basis: "contract"` |
| `conv108:A5-2-c` | `lawful_basis: "legal_obligation"` |
| `conv108:A5-2-d` | `lawful_basis: "legitimate_interest"` |
| `conv108:A5-2-e` | `lawful_basis: "vital_interest"` |
| `conv108:A5-2-f` | `lawful_basis: "public_interest"` |
| `conv108:ControllerIdentificationRecord` | `/.well-known/notice.txt` |
| `conv108:NoticeEventLog` | `notice_event_log_url` |
| `conv108:CrossBorderSurveillanceDisclosure` | `surveillance_risks` |

### Notice Event Type Mappings

| DPV Event | ISO/IEC 27560-1 event_type |
| --- | --- |
| Notice issued | `notice_issued` |
| Consent granted | `consent_granted` |
| Consent withdrawn | `consent_withdrawn` |
| Material change | `material_change` |
| Rights exercised | `rights_exercised` |
| Data breach | `data_breach` |
| Cross-border transfer | `cross_border_transfer` |

---

## 10. Examples

### Example 1: Consent Legal Basis with CIR

```
@prefix conv108: <https://w3id.org/dpv/legal/coe/convention108plus#> .
@prefix dpv: <https://w3id.org/dpv#> .

ex:Processing1 a dpv:PersonalDataHandling ;
    dpv:hasLegalBasis conv108:A5-2-a ;
    dpv:hasNotice ex:CIR1 .

ex:CIR1 a conv108:ControllerIdentificationRecord ;
    dpv:hasController ex:Controller1 ;
    dpv:hasPurpose ex:Purpose1 ;
    dpv:hasPersonalData ex:EmailAddress .
```

### Example 2: Cross-Border Transfer with Surveillance Risk Disclosure

```
ex:Transfer1 a dpv:DataTransfer ;
    dpv:hasLegalBasis conv108:A14-adequate-safeguards ;
    dpv:hasNotice ex:SurveillanceNotice1 .

ex:SurveillanceNotice1 a conv108:CrossBorderSurveillanceDisclosure ;
    conv108:surveillanceRisk "Third country subject to mass surveillance laws" ;
    conv108:recipientJurisdiction "US" .
```

### Example 3: Notice Receipt Mechanism with Event Log

```
ex:NoticeReceipt1 a conv108:BilateralNoticeProof ;
    conv108:anchorHash "sha256:abc123..." ;
    conv108:timestamp "2025-12-29T14:00:00Z" ;
    conv108:noticeEventLogURL "https://example.com/.well-known/notice-log" .
```

---

## 11. References

**Normative**:

- Council of Europe Convention 108+ (CoE Treaty 223)
- EU Regulation 2018/1725
- ISO/IEC 27560-1:2025 Universal Notice Receipt Profile
- W3C Data Privacy Vocabulary (DPV)

**Informative**:

- GDPR DPV Extension v2.2
- DPCat Specification
- Convention 108+ Explanatory Report

---

## Appendix A: Concept Index

[Complete alphabetical index of all concepts with URIs]

---

## Appendix B: Legal Basis × Rights Mapping

[Mapping table showing which rights apply to each legal basis]

---

## Appendix C: Change Log

**Version 0.1** (2025-12-29):

- Initial draft specification
- Core legal bases (Article 5)
- Rights framework (Article 9)
- Transparency requirements (Article 8)
- Transfer mechanisms (Article 14)
- Derivative digital transparency controls
- ISO/IEC 27560-1 integration

---

**Status**: This is a working draft for community review. Feedback welcome via Digital Transparency Lab or W3C DPVCG channels.

**Mapping**: 
