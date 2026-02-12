# W3C DPV Implementation Report: GPC, MyTerms, and 72 Digital Transparency Control Contexts

# Executive Summary

This informative report demonstrates how W3C Data Privacy Vocabulary (DPV) provides the semantic foundation for implementing:

**Global Privacy Control (GPC)** automated signals per ISO/IEC 27560:2025-1 privacy_choices field

**MyTerms** preference matching architecture with Controller-ID first disclosure

**72 Digital Transparency Control Contexts** (3 vectors × 4 TATA levels × 6 legal bases)

DPV serves as the machine-readable vocabulary layer enabling standardized expression of privacy concepts across automated signals, individual preferences, and controller disclosures.

## Purpose and Scope

**Purpose**: Provide W3C DPV community with reference implementation showing vocabulary application to ISO/IEC 27560 Universal Notice Receipt Profile infrastructure

**Scope**:

- DPV vocabulary mapping to ISO/IEC 27560:2025-1 fields
- GPC signal expression using DPV terms
- MyTerms preference file structure with DPV semantics
- 72 context enumeration using DPV legal basis, purpose, and processing terms

**Audience**: W3C DPV Working Group, ISO/IEC JTC 1/SC 27/WG 5, privacy engineers, regulatory technology implementers

**Related W3C DPV Work**: This report aligns with ongoing W3C DPV community work on Convention 108+ legal model profile (Issue 414: "Convention 108+ legal model profile is initially started - feedback welcome" at [https://github.com/w3c/dpv/issues/414](https://github.com/w3c/dpv/issues/414)). The 72 Digital Transparency Control Contexts framework presented here provides implementation guidance for Convention 108+ Article 5 legal bases within the ISO/IEC 27560 Universal Notice Receipt Profile infrastructure.

---

## Section 1: DPV Mapping to ISO/IEC 27560 PII Processing Records

### 1.1 Core Field Mappings

**ISO/IEC 27560 Field** → **W3C DPV Term**

- `lawful_basis` → `dpv:LegalBasis` (with subclasses: `dpv:Consent`, `dpv:Contract`, `dpv:LegitimateInterest`, etc.)
- `processing_purposes` → `dpv:Purpose` (with hierarchy: `dpv:ServiceProvision`, `dpv:Marketing`, `dpv:Analytics`, etc.)
- `pii_categories` → `dpv:PersonalData` (with taxonomy: `dpv:Identifying`, `dpv:Sensitive`, etc.)
- `privacy_choices` → `dpv:Right` exercised (specifically `dpv:OptOut`, `dpv:Object`, `dpv:WithdrawConsent`)
- `third_party_disclosure` → `dpv:Recipient` with `dpv:ThirdParty` role
- `retention_period` → `dpv:StorageDuration`
- `jurisdiction` → `dpv:Location` with legal jurisdiction context

### 1.2 Example: PII Processing Record with DPV Annotations

```json
{
  "pii_processing": {
    "lawful_basis": "legitimate_interests",
    "@context": "[https://w3id.org/dpv#](https://w3id.org/dpv#)",
    "dpv:legalBasis": "dpv:LegitimateInterest",
    "processing_purposes": ["analytics", "fraud_prevention"],
    "dpv:purpose": ["dpv:Analytics", "dpv:FraudPreventionAndDetection"],
    "privacy_choices": [{
      "choice_type": "opt_out",
      "choice_mechanism": "global_privacy_control",
      "dpv:right": "dpv:OptOut",
      "dpv:exerciseMethod": "dpv:AutomatedDecision"
    }]
  }
}
```

### 1.3 Benefits of DPV Integration

**Semantic interoperability**: Different systems can exchange privacy information with shared vocabulary

**Automated reasoning**: Privacy policy validators can check consistency using DPV ontology relationships

**Regulatory alignment**: DPV terms map to GDPR, Convention 108+, CCPA legal concepts

**Multilingual support**: DPV provides translations enabling international deployment

---

## Section 2: GPC Implementation with DPV Vocabulary

### 2.1 GPC Signal Expression Using DPV

**GPC signal components expressed in DPV terms**:

```
:gpc_signal_1 a dpv:Right ;
  dpv:hasRight dpv:OptOut ;
  dpv:hasRightExerciseActivity [
    dpv:exerciseMethod dpv:AutomatedDecision ;
    dpv:exerciseSource "user_agent" ;
    dpv:exerciseTime "2026-01-08T04:48:21Z"^^xsd:dateTime ;
  ] ;
  dpv:hasScope dpv:SellDataToThirdParties ;
  dpv:hasJurisdiction dpv-loc:US-CA .
```

### 2.2 Multi-Lawful-Basis GPC Handling with DPV

**Scenario 1: GPC when lawful basis = Consent**

```
:processing_activity_1 a dpv:PersonalDataProcessing ;
  dpv:hasLegalBasis dpv:Consent ;
  dpv:hasConsentNotice :consent_notice_1 ;
  dpv:hasWithdrawalMethod [
    dpv:exerciseMethod dpv:AutomatedDecision ;
    dpv:mechanism "global_privacy_control" ;
  ] .
```

**Scenario 2: GPC when lawful basis = Legitimate Interest**

```
:processing_activity_2 a dpv:PersonalDataProcessing ;
  dpv:hasLegalBasis dpv:LegitimateInterest ;
  dpv:hasObjectionProcess [
    dpv:exerciseMethod dpv:AutomatedDecision ;
    dpv:mechanism "global_privacy_control" ;
    dpv:processingImpact "cease_non_essential_processing" ;
  ] .
```

### 2.3 DPV Extensions for GPC-Specific Concepts

**Proposed new terms for DPV vocabulary**:

- `dpv:GlobalPrivacyControl` (subclass of `dpv:AutomatedDecision`)
- `dpv:DoNotSellOrShare` (subclass of `dpv:OptOut`)
- `dpv:SignalSource` (property indicating browser, extension, wallet)
- `dpv:SignalValue` (property for actual signal received, e.g., "Sec-GPC: 1")

---

## Section 3: MyTerms Preference Files with DPV Semantics

### 3.1 MyTerms Architecture Overview

**MyTerms preference matching requires**:

1. **Individual publishes preferences** (personal preference file with DPV-structured terms)
2. **Controller publishes notice.txt** (Controller-ID disclosure with DPV-annotated processing)
3. **Browser matches preferences** (automated comparison using DPV vocabulary)
4. **ANCR receipt generated** (bilateral proof using DPV terms for verifiable transparency)

### 3.2 Individual MyTerms Preference File Structure

```json
{
  "@context": "[https://w3id.org/dpv#](https://w3id.org/dpv#)",
  "preferences": {
    "acceptable_legal_bases": [
      {"dpv:legalBasis": "dpv:Consent", "required_withdrawal_method": "autonomous"},
      {"dpv:legalBasis": "dpv:Contract", "scope": "essential_only"}
    ],
    "restricted_purposes": [
      {"dpv:purpose": "dpv:Advertising", "preference": "opt_out"},
      {"dpv:purpose": "dpv:Profiling", "preference": "opt_out"},
      {"dpv:purpose": "dpv:PersonalisedAdvertising", "preference": "opt_out"}
    ],
    "restricted_recipients": [
      {"dpv:recipient": "dpv:ThirdParty", "restriction": "no_cross_border_transfer"},
      {"dpv:recipient": "dpv:DataBroker", "restriction": "prohibited"}
    ],
    "maximum_retention": {
      "dpv:storageDuration": "P365D",
      "exception": ["legal_obligation"]
    },
    "required_rights": [
      "dpv:DataPortability",
      "dpv:Access",
      "dpv:Erasure",
      "dpv:AutomatedWithdrawal"
    ],
    "required_transparency": {
      "controller_identification": "before_pii_collection",
      "notice_receipt": "bilateral_proof",
      "event_log": "append_only_queryable"
    }
  },
  "signature": "<cryptographic_signature>"
}
```

### 3.3 Controller notice.txt with DPV Annotations

```json
{
  "@context": "[https://w3id.org/dpv#](https://w3id.org/dpv#)",
  "controller_identification_record": {
    "cir_id": "CIR-CA-ACME-2026",
    "controller_name": "Acme Corporation",
    "dpv:hasJurisdiction": "dpv-loc:CA",
    "processing_activities": [
      {
        "activity_id": "service_delivery",
        "dpv:legalBasis": "dpv:Contract",
        "dpv:purpose": ["dpv:ServiceProvision", "dpv:CustomerManagement"],
        "dpv:personalData": ["dpv:Contact", "dpv:Account"],
        "dpv:recipient": ["dpv:DataController"],
        "dpv:storageDuration": "P730D",
        "dpv:hasRight": ["dpv:Access", "dpv:Rectification", "dpv:Erasure"]
      },
      {
        "activity_id": "analytics",
        "dpv:legalBasis": "dpv:LegitimateInterest",
        "dpv:purpose": "dpv:Analytics",
        "dpv:personalData": ["dpv:BehaviouralData"],
        "dpv:hasRight": ["dpv:Object", "dpv:OptOut"],
        "dpv:automatedSignalHonored": ["global_privacy_control"]
      }
    ]
  }
}
```

### 3.4 Browser Preference Matching Logic Using DPV

**Matching algorithm**:

1. **Parse individual preferences** using DPV vocabulary
2. **Query controller notice.txt** for DPV-annotated processing activities
3. **Check compatibility**:
    - Are individual's restricted purposes present in controller's processing?
    - Are individual's restricted recipients present in controller's disclosure?
    - Does controller's retention exceed individual's maximum?
    - Does controller honor automated signals individual requires?
4. **Generate compatibility report** with DPV-structured results
5. **Present choice** to individual with transparency evidence

**Example compatibility check**:

```python
def check_preference_compatibility(individual_prefs, controller_notice):
    conflicts = []
    
    # Check restricted purposes
    for restricted in individual_prefs['restricted_purposes']:
        for activity in controller_notice['processing_activities']:
            if restricted['dpv:purpose'] in activity['dpv:purpose']:
                if restricted['preference'] == 'opt_out':
                    if 'dpv:OptOut' not in activity.get('dpv:hasRight', []):
                        conflicts.append({
                            'type': 'purpose_conflict',
                            'purpose': restricted['dpv:purpose'],
                            'individual_preference': 'opt_out',
                            'controller_support': 'not_supported'
                        })
    
    # Check restricted recipients
    for restricted in individual_prefs['restricted_recipients']:
        for activity in controller_notice['processing_activities']:
            if restricted['dpv:recipient'] in activity['dpv:recipient']:
                if restricted['restriction'] == 'prohibited':
                    conflicts.append({
                        'type': 'recipient_conflict',
                        'recipient': restricted['dpv:recipient'],
                        'restriction': 'prohibited',
                        'controller_practice': 'disclosure_occurs'
                    })
    
    return {
        'compatible': len(conflicts) == 0,
        'conflicts': conflicts,
        'negotiation_possible': can_negotiate(conflicts)
    }
```

### 3.5 MyTerms + DPV Benefits

**Standardized vocabulary**: Both individuals and controllers use same DPV terms, enabling precise matching

**Machine-readable negotiation**: Browser can automatically determine compatibility without human interpretation

**Regulatory alignment**: DPV terms map to legal requirements, ensuring MyTerms preferences express legally recognizable rights

**Interoperability**: MyTerms files portable across browsers, consent wallets, personal data stores

---

## Section 4: 72 Digital Transparency Control Contexts with DPV Enumeration

### 4.1 Framework Structure

**72 Contexts = 3 Vectors × 4 TATA Levels × 6 Legal Bases**

**Three Vectors of Personal Data Control**:

1. **Personal Control** (SSI/DID paradigm) - Individual self-identifies
2. **Data Protection** (Federated identification) - Third parties identify individual
3. **Co-Regulation** (ISO/IEC 27560 MVCR-ANCR) - Standardized notice-and-consent with regulatory enforcement

**Four TATA Levels** (Transparency and Trust Assurance):

- **Level 1**: Self-Assertion (controller self-certifies)
- **Level 2**: Registered Controller (registrar verification)
- **Level 3**: ANCR and Signaling (cryptographic signatures)
- **Level 4**: Active State High Assurance (real-time signaling)

**Six Legal Bases** (Convention 108+ Article 5):

- Consent
- Contract
- Legal Obligation
- Legitimate Interest
- Vital Interest
- Public Interest

> **Note**: This enumeration aligns with W3C DPV Issue 414 work on Convention 108+ legal model profile. The 72 context framework demonstrates how these six legal bases operate across different control vectors and assurance levels in practice.
> 

### 4.2 DPV Vocabulary for Context Enumeration

**Vector expression in DPV**:

```
:personal_control_vector a dpv:DataControlParadigm ;
  rdfs:label "Personal Control Vector" ;
  rdfs:comment "Individual self-identifies and manages own data (SSI/DID paradigm)" ;
  dpv:hasIdentificationMethod dpv:SelfIdentification ;
  dpv:hasControlAuthority dpv:DataSubject .

:data_protection_vector a dpv:DataControlParadigm ;
  rdfs:label "Data Protection Vector" ;
  rdfs:comment "Federated identification where third parties identify individuals" ;
  dpv:hasIdentificationMethod dpv:ThirdPartyIdentification ;
  dpv:hasControlAuthority dpv:DataController .

:co_regulation_vector a dpv:DataControlParadigm ;
  rdfs:label "Co-Regulation Vector" ;
  rdfs:comment "ISO/IEC 27560 MVCR-ANCR profile: standardized notice-and-consent with regulatory enforcement" ;
  dpv:hasIdentificationMethod dpv:VerifiedIdentification ;
  dpv:hasControlAuthority dpv:SharedAuthority ;
  dpv:hasStandard "ISO/IEC 27560:2025-1" .
```

**TATA Level expression in DPV**:

```
:tata_level_1 a dpv:AssuranceLevel ;
  rdfs:label "TATA Level 1: Self-Assertion" ;
  dpv:hasVerificationMethod dpv:SelfCertification ;
  dpv:hasAuditRequirement "none" .

:tata_level_2 a dpv:AssuranceLevel ;
  rdfs:label "TATA Level 2: Registered Controller" ;
  dpv:hasVerificationMethod dpv:RegistrarVerification ;
  dpv:hasAuditRequirement "registrar_audit" .

:tata_level_3 a dpv:AssuranceLevel ;
  rdfs:label "TATA Level 3: ANCR and Signaling" ;
  dpv:hasVerificationMethod dpv:CryptographicSignature ;
  dpv:hasAuditRequirement "blind_notary_timestamp" .

:tata_level_4 a dpv:AssuranceLevel ;
  rdfs:label "TATA Level 4: Active State High Assurance" ;
  dpv:hasVerificationMethod dpv:RealtimeVerification ;
  dpv:hasAuditRequirement "certified_notarization" .
```

**Legal Basis enumeration** (already in DPV):

```
dpv:Consent a dpv:LegalBasis .
dpv:Contract a dpv:LegalBasis .
dpv:LegalObligation a dpv:LegalBasis .
dpv:LegitimateInterest a dpv:LegalBasis .
dpv:VitalInterest a dpv:LegalBasis .
dpv:PublicInterest a dpv:LegalBasis .
```

### 4.3 Context Enumeration Matrix

**Each of the 72 contexts can be expressed as a unique combination**:

```
:context_1 a dpv:DataProcessingContext ;
  dpv:hasVector :personal_control_vector ;
  dpv:hasAssuranceLevel :tata_level_1 ;
  dpv:hasLegalBasis dpv:Consent ;
  dpv:contextIdentifier "PC-L1-CON" .

:context_2 a dpv:DataProcessingContext ;
  dpv:hasVector :personal_control_vector ;
  dpv:hasAssuranceLevel :tata_level_1 ;
  dpv:hasLegalBasis dpv:Contract ;
  dpv:contextIdentifier "PC-L1-CTR" .

# ... (70 more contexts)

:context_72 a dpv:DataProcessingContext ;
  dpv:hasVector :co_regulation_vector ;
  dpv:hasAssuranceLevel :tata_level_4 ;
  dpv:hasLegalBasis dpv:PublicInterest ;
  dpv:contextIdentifier "CR-L4-PUB" .
```

### 4.4 Context-Specific Privacy Controls with DPV

**Example: Personal Control Vector, TATA Level 3, Consent Basis**

```json
{
  "@context": "[https://w3id.org/dpv#](https://w3id.org/dpv#)",
  "context_id": "PC-L3-CON",
  "dpv:hasVector": "personal_control_vector",
  "dpv:hasAssuranceLevel": "tata_level_3",
  "dpv:hasLegalBasis": "dpv:Consent",
  "controls": {
    "identification": {
      "method": "self_identification",
      "dpv:hasIdentifier": "did:example:123456789",
      "controller_knows_identity": false,
      "individual_controls_disclosure": true
    },
    "consent_management": {
      "dpv:hasConsentRecord": "ANCR_signed_receipt",
      "withdrawal_method": "autonomous_deletion",
      "cryptographic_proof": true,
      "blind_notary_timestamp": true
    },
    "transparency_requirements": {
      "controller_id_first": true,
      "notice_receipt_bilateral": true,
      "notice_event_log": "append_only",
      "tpi_assessment": "level_3_required"
    },
    "rights_exercise": {
      "dpv:hasRight": ["dpv:Access", "dpv:Erasure", "dpv:DataPortability"],
      "exercise_method": "consent_token_revocation",
      "verification": "cryptographic_signature"
    }
  }
}
```

**Example: Co-Regulation Vector, TATA Level 2, Legitimate Interest Basis**

```json
{
  "@context": "[https://w3id.org/dpv#](https://w3id.org/dpv#)",
  "context_id": "CR-L2-LI",
  "dpv:hasVector": "co_regulation_vector",
  "dpv:hasAssuranceLevel": "tata_level_2",
  "dpv:hasLegalBasis": "dpv:LegitimateInterest",
  "controls": {
    "identification": {
      "method": "controller_identification_record",
      "registrar_verified": true,
      "dpv:hasControllerIdentity": "CIR-CA-ACME-2026"
    },
    "legitimate_interest_assessment": {
      "balancing_test_published": true,
      "dpv:hasPurpose": ["dpv:FraudPreventionAndDetection", "dpv:Analytics"],
      "proportionality_documented": true
    },
    "objection_mechanism": {
      "dpv:hasRight": "dpv:Object",
      "automated_signals_honored": ["global_privacy_control"],
      "processing_ceases": "unless_compelling_grounds"
    },
    "transparency_requirements": {
      "notice_receipt_generated": true,
      "registrar_verification": "controller_registry_entry",
      "tpi_assessment": "level_2_required"
    }
  }
}
```

### 4.5 Benefits of DPV-Structured 72 Contexts

**Regulatory clarity**: Each context maps to specific legal obligations and technical controls

**Implementation guidance**: Engineers know exactly which controls to implement for each context

**Audit trail**: Notice Event Logs track context transitions (e.g., consent withdrawn → legitimate interest basis)

**Automated compliance checking**: Systems can verify correct controls are in place for declared context

**Interoperability**: Different privacy management platforms can exchange context information using shared DPV vocabulary

---

## Section 5: Integration Architecture

### 5.1 System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                    Individual (PII Principal)                    │
│                                                                   │
│  ┌───────────────┐  ┌──────────────┐  ┌────────────────────┐  │
│  │  MyTerms      │  │  GPC Signal  │  │  Consent Wallet    │  │
│  │  Preferences  │  │  (Browser)   │  │  (ANCR Storage)    │  │
│  │  (DPV)        │  │  (DPV)       │  │  (DPV)             │  │
│  └───────┬───────┘  └──────┬───────┘  └────────┬───────────┘  │
│          │                  │                    │               │
└──────────┼──────────────────┼────────────────────┼──────────────┘
           │                  │                    │
           ▼                  ▼                    ▼
┌─────────────────────────────────────────────────────────────────┐
│              Browser / Personal Data Platform                     │
│                                                                   │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  DPV-Based Preference Matching Engine                     │  │
│  │  - Parse MyTerms (DPV vocabulary)                         │  │
│  │  - Query notice.txt (DPV annotations)                     │  │
│  │  - Check GPC signal (DPV rights expression)              │  │
│  │  - Determine context (72 contexts via DPV)               │  │
│  │  - Generate compatibility report                          │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                   │
└───────────────────────────────┬───────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│                     Controller (Data Controller)                  │
│                                                                   │
│  ┌──────────────────────┐    ┌──────────────────────────────┐  │
│  │  Controller-ID        │    │  ISO/IEC 27560 PII           │  │
│  │  Record (DPV)         │◄───┤  Processing Records (DPV)    │  │
│  │  - notice.txt         │    │  - lawful_basis (DPV)        │  │
│  │  - CIR with DPV       │    │  - privacy_choices (DPV)     │  │
│  │    annotations        │    │  - purposes (DPV)            │  │
│  └───────────┬───────────┘    └──────────┬───────────────────┘  │
│              │                            │                       │
│              ▼                            ▼                       │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  Notice Event Log (Append-Only)                          │   │
│  │  - DPV-annotated events                                  │   │
│  │  - Context transitions (72 contexts)                     │   │
│  │  - GPC signal receipt (DPV rights)                       │   │
│  │  - MyTerms negotiation outcomes                          │   │
│  └─────────────────────────────────────────────────────────┘   │
│                              │                                    │
│                              ▼                                    │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  ANCR Generator                                           │   │
│  │  - Bilateral receipt with DPV terms                       │   │
│  │  - Cryptographic signature                                │   │
│  │  - Notice Receipt delivered to individual                 │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                   │
└───────────────────────────────┬───────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│              Transparency Assessment (TPI-R)                      │
│                                                                   │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  DPV-Based Compliance Verification                        │  │
│  │  - Check Controller-ID disclosure completeness (DPV)      │  │
│  │  - Verify privacy_choices field population (DPV)          │  │
│  │  - Validate GPC signal documentation (DPV)                │  │
│  │  - Assess context integrity (72 contexts)                 │  │
│  │  - Generate TPI-Report with DPV-structured findings       │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                   │
└───────────────────────────────────────────────────────────────────┘
```

### 5.2 Data Flow: GPC Signal with DPV Annotations

**Step 1**: Individual enables GPC in browser

- Browser sets `navigator.globalPrivacyControl = true`
- Browser sends `Sec-GPC: 1` HTTP header

**Step 2**: Controller receives signal with DPV context

```json
{
  "signal_received": {
    "dpv:hasRight": "dpv:OptOut",
    "dpv:exerciseMethod": "dpv:AutomatedDecision",
    "mechanism": "global_privacy_control",
    "timestamp": "2026-01-08T04:48:21Z"
  }
}
```

**Step 3**: Controller determines applicable context (72 contexts)

```json
{
  "context_determination": {
    "current_context": "CR-L2-LI",
    "dpv:hasVector": "co_regulation_vector",
    "dpv:hasAssuranceLevel": "tata_level_2",
    "dpv:hasLegalBasis": "dpv:LegitimateInterest"
  }
}
```

**Step 4**: Controller generates PII processing record with DPV annotations

```json
{
  "pii_processing": {
    "dpv:legalBasis": "dpv:LegitimateInterest",
    "privacy_choices": [{
      "dpv:hasRight": "dpv:Object",
      "mechanism": "global_privacy_control",
      "processing_response": "ceased_non_essential"
    }]
  }
}
```

**Step 5**: Controller appends event to Notice Event Log

```json
{
  "event": {
    "event_type": "automated_signal_received",
    "dpv:hasRight": "dpv:OptOut",
    "context_id": "CR-L2-LI",
    "timestamp": "2026-01-08T04:48:21Z"
  }
}
```

**Step 6**: Controller generates ANCR (bilateral receipt)

```json
{
  "receipt_header": {
    "schema_version": "27560-COMMON-NOTICE-2025-1.0",
    "dpv:hasContext": "CR-L2-LI"
  },
  "notice_context": {
    "dpv:hasRight": "dpv:Object",
    "processing_changes": {
      "ceased": ["dpv:PersonalisedAdvertising"],
      "continued": ["dpv:ServiceProvision", "dpv:FraudPreventionAndDetection"]
    }
  }
}
```

**Step 7**: Individual receives ANCR in consent wallet

- DPV annotations enable automated verification
- Individual can query Notice Event Log using DPV vocabulary

### 5.3 Data Flow: MyTerms Preference Matching with DPV

**Step 1**: Individual publishes MyTerms preferences with DPV structure

```json
{
  "preferences": {
    "restricted_purposes": [
      {"dpv:purpose": "dpv:Advertising", "preference": "opt_out"}
    ]
  }
}
```

**Step 2**: Browser queries controller notice.txt

```json
{
  "processing_activities": [{
    "dpv:purpose": ["dpv:ServiceProvision", "dpv:Advertising"],
    "dpv:hasRight": ["dpv:OptOut"]
  }]
}
```

**Step 3**: Browser matches using DPV vocabulary

```python
if "dpv:Advertising" in controller_purposes:
    if "dpv:OptOut" in controller_rights:
        result = "compatible_with_opt_out"
    else:
        result = "incompatible"
```

**Step 4**: Browser presents choice to individual

```json
{
  "compatibility_report": {
    "compatible": true,
    "action_required": "opt_out_advertising",
    "dpv:hasRight": "dpv:OptOut",
    "mechanism": "global_privacy_control"
  }
}
```

**Step 5**: Individual confirms, browser sends GPC signal

- Follows GPC flow (Section 5.2)
- ANCR receipt includes MyTerms negotiation outcome

---

## Section 6: DPV Extensions and Proposals

### 6.1 New Terms Required for Full Implementation

**Proposed additions to W3C DPV vocabulary**:

### Vector and Control Paradigm Terms

```
dpv:DataControlParadigm a rdfs:Class ;
  rdfs:label "Data Control Paradigm" ;
  rdfs:comment "Framework for expressing who controls identification and data processing decisions" .

dpv:hasVector a rdf:Property ;
  rdfs:domain dpv:DataProcessingContext ;
  rdfs:range dpv:DataControlParadigm .

dpv:PersonalControlVector a dpv:DataControlParadigm ;
  rdfs:label "Personal Control Vector" ;
  rdfs:comment "Individual self-identifies and manages own data (SSI/DID paradigm)" .

dpv:DataProtectionVector a dpv:DataControlParadigm ;
  rdfs:label "Data Protection Vector" ;
  rdfs:comment "Federated identification where third parties identify individuals" .

dpv:CoRegulationVector a dpv:DataControlParadigm ;
  rdfs:label "Co-Regulation Vector" ;
  rdfs:comment "Standardized notice-and-consent with regulatory enforcement" .
```

### Assurance Level Terms

```
dpv:AssuranceLevel a rdfs:Class ;
  rdfs:label "Transparency and Trust Assurance Level" ;
  rdfs:comment "Level of verification and auditability for transparency claims" .

dpv:hasAssuranceLevel a rdf:Property ;
  rdfs:domain dpv:DataProcessingContext ;
  rdfs:range dpv:AssuranceLevel .

dpv:SelfAssertion a dpv:AssuranceLevel ;
  rdfs:label "Self-Assertion (TATA Level 1)" ;
  rdfs:comment "Controller self-certifies transparency claims without external verification" .

dpv:RegistrarVerification a dpv:AssuranceLevel ;
  rdfs:label "Registrar Verification (TATA Level 2)" ;
  rdfs:comment "Third-party registrar verifies controller identity and basic transparency claims" .

dpv:CryptographicProof a dpv:AssuranceLevel ;
  rdfs:label "Cryptographic Proof (TATA Level 3)" ;
  rdfs:comment "Cryptographic signatures and blind notary timestamps provide verifiable proof" .

dpv:RealtimeAssurance a dpv:AssuranceLevel ;
  rdfs:label "Realtime Assurance (TATA Level 4)" ;
  rdfs:comment "Active state signaling with certified notarization and liveliness assurance" .
```

### GPC-Specific Terms

```
dpv:GlobalPrivacyControl a dpv:Right ;
  rdfs:subClassOf dpv:OptOut ;
  rdfs:label "Global Privacy Control" ;
  rdfs:comment "W3C GPC automated signal expressing opt-out preference for sale/sharing of personal data" ;
  dpv:hasSpecification <[https://globalprivacycontrol.org/](https://globalprivacycontrol.org/)> .

dpv:DoNotSellOrShare a dpv:Purpose ;
  rdfs:label "Do Not Sell or Share" ;
  rdfs:comment "Preference to prevent sale or sharing of personal data with third parties" .

dpv:SignalSource a rdf:Property ;
  rdfs:domain dpv:RightExercise ;
  rdfs:range xsd:string ;
  rdfs:label "Signal Source" ;
  rdfs:comment "Source of automated privacy signal (e.g., user_agent, browser_extension, privacy_wallet)" .

dpv:SignalValue a rdf:Property ;
  rdfs:domain dpv:RightExercise ;
  rdfs:range xsd:string ;
  rdfs:label "Signal Value" ;
  rdfs:comment "Actual signal value received (e.g., 'Sec-GPC: 1', 'DNT: 1')" .
```

### MyTerms-Specific Terms

```
dpv:PreferenceFile a rdfs:Class ;
  rdfs:label "Privacy Preference File" ;
  rdfs:comment "Machine-readable file expressing individual's privacy preferences (MyTerms architecture)" .

dpv:hasPreference a rdf:Property ;
  rdfs:domain dpv:DataSubject ;
  rdfs:range dpv:PreferenceFile .

dpv:PreferenceMatching a dpv:Process ;
  rdfs:label "Preference Matching" ;
  rdfs:comment "Automated comparison of individual preferences to controller processing disclosures" .

dpv:CompatibilityReport a rdfs:Class ;
  rdfs:label "Compatibility Report" ;
  rdfs:comment "Result of preference matching showing compatibility or conflicts" .
```

### Controller-ID First Terms

```
dpv:ControllerIdentificationRecord a rdfs:Class ;
  rdfs:label "Controller Identification Record" ;
  rdfs:comment "Standardized disclosure of controller identity before PII collection (Controller-ID first)" .

dpv:NoticeReceipt a rdfs:Class ;
  rdfs:label "Notice Receipt" ;
  rdfs:comment "Bilateral proof of notice delivery provided to both controller and individual (ANCR)" .

dpv:NoticeEventLog a rdfs:Class ;
  rdfs:label "Notice Event Log" ;
  rdfs:comment "Append-only audit trail of transparency events" .

dpv:hasNoticeReceipt a rdf:Property ;
  rdfs:domain dpv:ConsentRecord ;
  rdfs:range dpv:NoticeReceipt .
```

### 6.2 Extension Namespace Proposal

**Proposed extension namespace**: [`https://w3id.org/dpv-ancr#`](https://w3id.org/dpv-ancr#)

**Rationale**:

- Core DPV remains stable
- ANCR-specific terms in extension namespace
- Enables experimentation before potential core DPV integration

### 6.3 Compatibility with Existing DPV Terms

**Mapping ISO/IEC 27560 to existing DPV vocabulary**:

| ISO/IEC 27560 Field | Existing DPV Term | Additional ANCR Term Needed? |
| --- | --- | --- |
| lawful_basis | dpv:LegalBasis | No |
| processing_purposes | dpv:Purpose | No |
| pii_categories | dpv:PersonalData | No |
| third_party_disclosure | dpv:Recipient | No |
| retention_period | dpv:StorageDuration | No |
| privacy_choices | dpv:Right | Yes - dpv-ancr:PrivacyChoice |
| withdrawal_method | dpv:hasWithdrawalMethod | Extend with automated signals |
| notice_receipt | (none) | Yes - dpv-ancr:NoticeReceipt |
| event_log | (none) | Yes - dpv-ancr:NoticeEventLog |

---

## Section 7: Implementation Guidance

### 7.1 For Individuals Implementing MyTerms

**Step 1**: Create preference file using DPV vocabulary

- Use DPV terms for purposes, legal bases, recipients
- Express preferences as opt-in/opt-out per DPV rights
- Sign file cryptographically

**Step 2**: Publish preference file

- Personal website
- Consent wallet
- Personal data store

**Step 3**: Configure browser extension

- Install MyTerms extension that queries controller notice.txt
- Extension matches using DPV vocabulary
- Extension sends GPC signals when needed

**Step 4**: Receive ANCR receipts

- Store in consent wallet with DPV annotations
- Query Notice Event Logs using DPV vocabulary
- Verify controller compliance

### 7.2 For Controllers Implementing Controller-ID First + GPC

**Step 1**: Create Controller Identification Record with DPV annotations

- Document all processing activities using DPV terms
- Specify which automated signals are honored (GPC)
- Determine context (72 contexts) for each processing activity

**Step 2**: Publish notice.txt at `/.well-known/transparency`

- JSON-LD format with DPV @context
- Machine-readable for browser preference matching

**Step 3**: Implement GPC detection

- HTTP header check: `Sec-GPC: 1`
- DOM property check: `navigator.globalPrivacyControl`
- Express in DPV: `dpv:hasRight dpv:OptOut`

**Step 4**: Generate PII processing records

- Use ISO/IEC 27560 structure with DPV annotations
- Record GPC signal in privacy_choices field
- Determine applicable context from 72 options

**Step 5**: Append events to Notice Event Log

- Immutable, append-only log
- DPV-annotated events
- Queryable by individuals

**Step 6**: Generate and deliver ANCR

- Bilateral receipt with DPV terms
- Cryptographic signature (TATA Level 3+)
- Deliver via email, API, or download endpoint

### 7.3 For Browser/Platform Implementers

**Step 1**: Build DPV parsing engine

- Parse MyTerms preference files (DPV)
- Query controller notice.txt (DPV)
- Parse GPC signals (DPV rights expression)

**Step 2**: Implement preference matching algorithm

- Compare individual preferences to controller disclosures
- Use DPV vocabulary for semantic matching
- Generate compatibility report

**Step 3**: Present choice to individual

- Show compatibility report in human-readable format
- Explain conflicts and negotiation options
- Allow individual to confirm or adjust preferences

**Step 4**: Send automated signals

- GPC for opt-out preferences
- Future: consent tokens for opt-in preferences

**Step 5**: Receive and store ANCR receipts

- Store in consent wallet with DPV annotations
- Enable individual to query and verify
- Provide UI for viewing context transitions

### 7.4 For TPI-R Assessors

**Step 1**: Query controller notice.txt

- Verify availability at standard endpoint
- Parse DPV annotations
- Check completeness per 72 context requirements

**Step 2**: Test GPC signal handling

- Send `Sec-GPC: 1` header
- Verify appropriate processing response
- Check PII processing record generation

**Step 3**: Verify Notice Event Log

- Query log for sample events
- Check DPV annotations present
- Verify context transitions documented

**Step 4**: Validate ANCR receipts

- Request sample receipt
- Verify DPV-structured terms
- Check cryptographic signature (TATA Level 3+)

**Step 5**: Generate TPI-Report

- Use DPV vocabulary for findings
- Document context compliance (72 contexts)
- Assign TATA level certification

---

## Section 8: Use Cases and Examples

### 8.1 Use Case 1: GPC Signal from Browser to E-Commerce Site

**Scenario**: Individual visits e-commerce site with GPC enabled

**Context**: Co-Regulation Vector, TATA Level 2, Mixed Legal Bases

**DPV Flow**:

```json
// Step 1: Individual's browser sends GPC
{
  "@context": "[https://w3id.org/dpv#](https://w3id.org/dpv#)",
  "signal": {
    "dpv:hasRight": "dpv:GlobalPrivacyControl",
    "dpv:exerciseMethod": "dpv:AutomatedDecision",
    "dpv:signalValue": "Sec-GPC: 1",
    "timestamp": "2026-01-08T10:00:00Z"
  }
}

// Step 2: Controller determines context per activity
{
  "activities": [
    {
      "activity_id": "order_fulfillment",
      "context": "CR-L2-CTR",
      "dpv:legalBasis": "dpv:Contract",
      "gpc_response": "essential_processing_continues"
    },
    {
      "activity_id": "personalized_ads",
      "context": "CR-L2-LI",
      "dpv:legalBasis": "dpv:LegitimateInterest",
      "gpc_response": "processing_ceased"
    }
  ]
}

// Step 3: Controller generates PII processing record
{
  "pii_processing": {
    "privacy_choices": [{
      "dpv:hasRight": "dpv:GlobalPrivacyControl",
      "affected_activities": ["personalized_ads"],
      "continued_activities": ["order_fulfillment"],
      "context_per_activity": {
        "order_fulfillment": "CR-L2-CTR",
        "personalized_ads": "CR-L2-LI"
      }
    }]
  }
}

// Step 4: Controller generates ANCR
{
  "receipt_header": {
    "schema_version": "27560-COMMON-NOTICE-2025-1.0",
    "dpv:hasContext": ["CR-L2-CTR", "CR-L2-LI"]
  },
  "notice_context": {
    "dpv:hasRight": "dpv:GlobalPrivacyControl",
    "processing_changes": {
      "ceased": [{"dpv:purpose": "dpv:PersonalisedAdvertising"}],
      "continued": [{"dpv:purpose": "dpv:ServiceProvision", "rationale": "essential_for_contract"}]
    }
  }
}
```

**Result**: Individual receives ANCR showing personalized ads ceased but order fulfillment continues

### 8.2 Use Case 2: MyTerms Preference Matching for Banking Service

**Scenario**: Individual with MyTerms preferences seeks banking service

**Context**: Co-Regulation Vector, TATA Level 3, Contract Basis with Legitimate Interest for Analytics

**DPV Flow**:

```json
// Step 1: Individual's MyTerms preferences
{
  "@context": "[https://w3id.org/dpv#](https://w3id.org/dpv#)",
  "preferences": {
    "acceptable_legal_bases": [
      {"dpv:legalBasis": "dpv:Contract", "scope": "essential_only"},
      {"dpv:legalBasis": "dpv:LegalObligation", "scope": "any"}
    ],
    "restricted_purposes": [
      {"dpv:purpose": "dpv:Marketing", "preference": "opt_out"},
      {"dpv:purpose": "dpv:TargetedAdvertising", "preference": "prohibited"}
    ],
    "required_rights": [
      "dpv:Access",
      "dpv:Rectification",
      "dpv:Erasure",
      "dpv:DataPortability"
    ],
    "required_transparency": {
      "controller_id_first": true,
      "ancr_receipts": true,
      "tata_level_minimum": 3
    }
  }
}

// Step 2: Bank's notice.txt disclosure
{
  "@context": "[https://w3id.org/dpv#](https://w3id.org/dpv#)",
  "controller_identification_record": {
    "cir_id": "CIR-CA-BANK-2026",
    "tata_level": 3,
    "processing_activities": [
      {
        "activity_id": "account_services",
        "context": "CR-L3-CTR",
        "dpv:legalBasis": "dpv:Contract",
        "dpv:purpose": ["dpv:ServiceProvision", "dpv:AccountManagement"],
        "dpv:hasRight": ["dpv:Access", "dpv:Rectification", "dpv:Erasure"]
      },
      {
        "activity_id": "fraud_detection",
        "context": "CR-L3-LEG",
        "dpv:legalBasis": "dpv:LegalObligation",
        "dpv:purpose": "dpv:FraudPreventionAndDetection"
      },
      {
        "activity_id": "service_analytics",
        "context": "CR-L3-LI",
        "dpv:legalBasis": "dpv:LegitimateInterest",
        "dpv:purpose": "dpv:ServiceOptimization",
        "dpv:hasRight": ["dpv:Object", "dpv:OptOut"],
        "gpc_honored": true
      }
    ]
  }
}

// Step 3: Browser preference matching
{
  "compatibility_check": {
    "compatible": true,
    "matches": [
      {
        "check": "acceptable_legal_bases",
        "result": "PASS",
        "detail": "Bank uses Contract (essential only) and Legal Obligation"
      },
      {
        "check": "restricted_purposes",
        "result": "PASS",
        "detail": "No Marketing or Targeted Advertising present"
      },
      {
        "check": "required_rights",
        "result": "PARTIAL",
        "detail": "DataPortability not offered, but not essential for banking"
      },
      {
        "check": "required_transparency",
        "result": "PASS",
        "detail": "Controller-ID first, ANCR receipts, TATA Level 3"
      }
    ],
    "recommendation": "Proceed with service. Service analytics uses Legitimate Interest with opt-out available."
  }
}

// Step 4: Individual proceeds, receives ANCR
{
  "receipt_header": {
    "schema_version": "27560-COMMON-NOTICE-2025-1.0",
    "dpv:hasContext": ["CR-L3-CTR", "CR-L3-LEG", "CR-L3-LI"],
    "myterms_negotiation": "preferences_honored"
  },
  "notice_context": {
    "myterms_compatibility": {
      "individual_preferences_url": "[https://individual.example/myterms.json](https://individual.example/myterms.json)",
      "negotiation_outcome": "compatible",
      "opt_out_available": ["service_analytics"]
    }
  }
}
```

**Result**: Individual proceeds with banking service, knowing preferences are honored and analytics is opt-outable

### 8.3 Use Case 3: Context Transition in Notice Event Log

**Scenario**: Processing transitions from Consent to Legitimate Interest after consent withdrawal

**Context Transition**: CR-L2-CON → CR-L2-LI

**DPV Flow**:

```json
// Initial state: Consent basis
{
  "event": {
    "event_id": "evt-001",
    "event_type": "consent_given",
    "event_time": "2026-01-01T10:00:00Z",
    "context_before": null,
    "context_after": "CR-L2-CON",
    "dpv:legalBasis": "dpv:Consent",
    "dpv:purpose": ["dpv:PersonalisedAdvertising", "dpv:Marketing"]
  }
}

// Individual withdraws consent via GPC
{
  "event": {
    "event_id": "evt-002",
    "event_type": "consent_withdrawn",
    "event_time": "2026-01-05T14:30:00Z",
    "context_before": "CR-L2-CON",
    "context_after": null,
    "dpv:hasRight": "dpv:WithdrawConsent",
    "dpv:exerciseMethod": "dpv:GlobalPrivacyControl",
    "processing_impact": "all_consent_based_processing_ceased"
  }
}

// Controller transitions to legitimate interest for analytics
{
  "event": {
    "event_id": "evt-003",
    "event_type": "lawful_basis_change",
    "event_time": "2026-01-05T14:31:00Z",
    "context_before": null,
    "context_after": "CR-L2-LI",
    "dpv:legalBasis": "dpv:LegitimateInterest",
    "dpv:purpose": ["dpv:Analytics"],
    "rationale": "Legitimate interest for essential analytics only, personalized ads ceased",
    "balancing_test_url": "[https://controller.example/lia-assessment](https://controller.example/lia-assessment)",
    "dpv:hasRight": ["dpv:Object", "dpv:OptOut"],
    "gpc_continues_to_apply": true
  }
}

// Individual queries log, sees transition
{
  "log_query_response": {
    "contexts_over_time": [
      {"period": "2026-01-01 to 2026-01-05", "context": "CR-L2-CON"},
      {"period": "2026-01-05 14:30 to 14:31", "context": null, "note": "Processing ceased during transition"},
      {"period": "2026-01-05 14:31 onwards", "context": "CR-L2-LI"}
    ],
    "dpv:legalBasis_history": [
      {"basis": "dpv:Consent", "period": "2026-01-01 to 2026-01-05"},
      {"basis": "dpv:LegitimateInterest", "period": "2026-01-05 onwards"}
    ],
    "rights_available": {
      "before_transition": ["dpv:WithdrawConsent"],
      "after_transition": ["dpv:Object", "dpv:OptOut"]
    }
  }
}
```

**Result**: Individual has transparent audit trail showing context transition with DPV-structured legal basis change

---

## Section 9: Benefits and Recommendations

### 9.1 Benefits of DPV Integration

**For W3C DPV Community**:

- Real-world use case demonstrating vocabulary utility
- Bridges specifications (GPC) and standards (ISO/IEC 27560)
- Extends DPV into regulatory compliance infrastructure

**For ISO/IEC 27560 Implementers**:

- Machine-readable vocabulary for PII processing records
- Semantic interoperability across systems and jurisdictions
- Automated compliance checking capabilities

**For Individuals**:

- MyTerms preferences express rights using standardized vocabulary
- GPC signals documented with transparent legal effect
- Verifiable proof via ANCR receipts with DPV annotations

**For Controllers**:

- Clear guidance on GPC signal documentation requirements
- 72 context framework provides implementation roadmap
- TPI-R assessment using DPV enables certification

**For Regulators**:

- Standardized evidence format for enforcement actions
- Automated compliance monitoring using DPV vocabulary
- Co-regulatory infrastructure with verifiable transparency

### 9.2 Recommendations for W3C DPV Working Group

**Short-term (Q1 2026)**:

- Review proposed DPV extensions (Section 6.1)
- Consider creating `dpv-ancr` extension namespace
- Coordinate with ISO/IEC JTC 1/SC 27/WG 5 on vocabulary alignment
- Contribute feedback to Issue 414 (Convention 108+ legal model profile) based on this implementation report's 72 context framework demonstrating Article 5 legal bases in practice

**Medium-term (Q2-Q3 2026)**:

- Publish informative note on GPC + DPV integration
- Develop reference implementation with code examples
- Coordinate with Kantara ANCR WG on TPI-R + DPV integration

**Long-term (2026-2027)**:

- Consider incorporating successful ANCR extensions into core DPV
- Develop DPV-based MyTerms specification
- Expand 72 context framework to additional jurisdictions

### 9.3 Recommendations for ISO/IEC JTC 1/SC 27/WG 5

**ISO/IEC 27560 Enhancement**:

- Reference W3C DPV in informative annex
- Provide DPV @context for JSON examples
- Document vocabulary mapping in implementation guidance

**ISO/IEC 27091 (AI Privacy) Integration**:

- Use DPV vocabulary for AI system transparency disclosures
- Apply 72 context framework to AI training data provenance
- Extend Appendix J with DPV-structured examples

### 9.4 Implementation Roadmap

**Phase 1: Pilot (Q1 2026)**

- 5-10 volunteer organizations implement Controller-ID + GPC with DPV
- Document challenges and refinements
- Generate sample TPI-Reports

**Phase 2: Community Expansion (Q2-Q3 2026)**

- Open-source reference implementations (browser extension, controller library)
- Developer documentation with DPV examples
- Webinars and workshops

**Phase 3: Regulatory Coordination (Q4 2026)**

- Briefings for privacy commissioners (EDPB, OPC, CNIL)
- Demonstrate MyTerms + GPC + DPV as Article 7(3) compliance mechanism
- Propose regulatory guidance on Controller-ID first requirement

**Phase 4: Market Adoption (2027)**

- Integration with consent management platforms
- Browser vendor adoption (Firefox, Safari, Brave)
- Certification programs using TPI-R + DPV

---

## Section 10: Conclusion

### 10.1 Summary of Contributions

This report demonstrates:

**W3C DPV provides semantic foundation** for implementing GPC signals, MyTerms preferences, and 72 Digital Transparency Control Contexts

**ISO/IEC 27560 Universal Notice Receipt Profile** provides record structure and bilateral receipt infrastructure

**Integration creates co-regulatory system** where:

- Individuals express preferences using DPV vocabulary (MyTerms)
- Automated signals communicate choices (GPC with DPV)
- Controllers document processing using DPV-annotated records (ISO/IEC 27560)
- Independent assessment verifies transparency (TPI-R with DPV)
- Bilateral receipts provide verifiable proof (ANCR with DPV)

**72 context framework** provides comprehensive implementation guidance across three vectors, four assurance levels, and six legal bases

### 10.2 Next Steps

**For W3C DPV WG**: Review proposed extensions, coordinate with ISO/IEC JTC 1/SC 27/WG 5

**For ISO/IEC JTC 1/SC 27/WG 5**: Reference DPV in 27560 informative annexes

**For Kantara ANCR WG**: Integrate DPV vocabulary into ANCR specification

**For Implementers**: Begin pilot deployments using DPV-structured records

### 10.3 Document Status

**Status**: Draft for W3C DPV Working Group Review

**Date**: January 8, 2026

**Author**: Mark Lizar, ISO/IEC 27560-1 Profile Editor, Digital Transparency Lab

**Contact**: [mark@transparencylab.ca](mailto:mark@transparencylab.ca)

**Next Review**: W3C DPV WG meeting Q1 2026

---

## Appendix A: Complete 72 Context Enumeration with DPV

### Personal Control Vector (24 contexts)

**TATA Level 1 (Self-Assertion)**

- PC-L1-CON: Personal Control, Self-Assertion, Consent
- PC-L1-CTR: Personal Control, Self-Assertion, Contract
- PC-L1-LEG: Personal Control, Self-Assertion, Legal Obligation
- PC-L1-LI: Personal Control, Self-Assertion, Legitimate Interest
- PC-L1-VI: Personal Control, Self-Assertion, Vital Interest
- PC-L1-PUB: Personal Control, Self-Assertion, Public Interest

**TATA Level 2 (Registered Controller)**

- PC-L2-CON: Personal Control, Registered Controller, Consent
- PC-L2-CTR: Personal Control, Registered Controller, Contract
- PC-L2-LEG: Personal Control, Registered Controller, Legal Obligation
- PC-L2-LI: Personal Control, Registered Controller, Legitimate Interest
- PC-L2-VI: Personal Control, Registered Controller, Vital Interest
- PC-L2-PUB: Personal Control, Registered Controller, Public Interest

**TATA Level 3 (ANCR and Signaling)**

- PC-L3-CON: Personal Control, ANCR and Signaling, Consent
- PC-L3-CTR: Personal Control, ANCR and Signaling, Contract
- PC-L3-LEG: Personal Control, ANCR and Signaling, Legal Obligation
- PC-L3-LI: Personal Control, ANCR and Signaling, Legitimate Interest
- PC-L3-VI: Personal Control, ANCR and Signaling, Vital Interest
- PC-L3-PUB: Personal Control, ANCR and Signaling, Public Interest

**TATA Level 4 (Active State High Assurance)**

- PC-L4-CON: Personal Control, Active State High Assurance, Consent
- PC-L4-CTR: Personal Control, Active State High Assurance, Contract
- PC-L4-LEG: Personal Control, Active State High Assurance, Legal Obligation
- PC-L4-LI: Personal Control, Active State High Assurance, Legitimate Interest
- PC-L4-VI: Personal Control, Active State High Assurance, Vital Interest
- PC-L4-PUB: Personal Control, Active State High Assurance, Public Interest

### Data Protection Vector (24 contexts)

**TATA Level 1 (Self-Assertion)**

- DP-L1-CON: Data Protection, Self-Assertion, Consent
- DP-L1-CTR: Data Protection, Self-Assertion, Contract
- DP-L1-LEG: Data Protection, Self-Assertion, Legal Obligation
- DP-L1-LI: Data Protection, Self-Assertion, Legitimate Interest
- DP-L1-VI: Data Protection, Self-Assertion, Vital Interest
- DP-L1-PUB: Data Protection, Self-Assertion, Public Interest

**TATA Level 2 (Registered Controller)**

- DP-L2-CON: Data Protection, Registered Controller, Consent
- DP-L2-CTR: Data Protection, Registered Controller, Contract
- DP-L2-LEG: Data Protection, Registered Controller, Legal Obligation
- DP-L2-LI: Data Protection, Registered Controller, Legitimate Interest
- DP-L2-VI: Data Protection, Registered Controller, Vital Interest
- DP-L2-PUB: Data Protection, Registered Controller, Public Interest

**TATA Level 3 (ANCR and Signaling)**

- DP-L3-CON: Data Protection, ANCR and Signaling, Consent
- DP-L3-CTR: Data Protection, ANCR and Signaling, Contract
- DP-L3-LEG: Data Protection, ANCR and Signaling, Legal Obligation
- DP-L3-LI: Data Protection, ANCR and Signaling, Legitimate Interest
- DP-L3-VI: Data Protection, ANCR and Signaling, Vital Interest
- DP-L3-PUB: Data Protection, ANCR and Signaling, Public Interest

**TATA Level 4 (Active State High Assurance)**

- DP-L4-CON: Data Protection, Active State High Assurance, Consent
- DP-L4-CTR: Data Protection, Active State High Assurance, Contract
- DP-L4-LEG: Data Protection, Active State High Assurance, Legal Obligation
- DP-L4-LI: Data Protection, Active State High Assurance, Legitimate Interest
- DP-L4-VI: Data Protection, Active State High Assurance, Vital Interest
- DP-L4-PUB: Data Protection, Active State High Assurance, Public Interest

### Co-Regulation Vector (24 contexts)

**TATA Level 1 (Self-Assertion)**

- CR-L1-CON: Co-Regulation, Self-Assertion, Consent
- CR-L1-CTR: Co-Regulation, Self-Assertion, Contract
- CR-L1-LEG: Co-Regulation, Self-Assertion, Legal Obligation
- CR-L1-LI: Co-Regulation, Self-Assertion, Legitimate Interest
- CR-L1-VI: Co-Regulation, Self-Assertion, Vital Interest
- CR-L1-PUB: Co-Regulation, Self-Assertion, Public Interest

**TATA Level 2 (Registered Controller)**

- CR-L2-CON: Co-Regulation, Registered Controller, Consent
- CR-L2-CTR: Co-Regulation, Registered Controller, Contract
- CR-L2-LEG: Co-Regulation, Registered Controller, Legal Obligation
- CR-L2-LI: Co-Regulation, Registered Controller, Legitimate Interest
- CR-L2-VI: Co-Regulation, Registered Controller, Vital Interest
- CR-L2-PUB: Co-Regulation, Registered Controller, Public Interest

**TATA Level 3 (ANCR and Signaling)**

- CR-L3-CON: Co-Regulation, ANCR and Signaling, Consent
- CR-L3-CTR: Co-Regulation, ANCR and Signaling, Contract
- CR-L3-LEG: Co-Regulation, ANCR and Signaling, Legal Obligation
- CR-L3-LI: Co-Regulation, ANCR and Signaling, Legitimate Interest
- CR-L3-VI: Co-Regulation, ANCR and Signaling, Vital Interest
- CR-L3-PUB: Co-Regulation, ANCR and Signaling, Public Interest

**TATA Level 4 (Active State High Assurance)**

- CR-L4-CON: Co-Regulation, Active State High Assurance, Consent
- CR-L4-CTR: Co-Regulation, Active State High Assurance, Contract
- CR-L4-LEG: Co-Regulation, Active State High Assurance, Legal Obligation
- CR-L4-LI: Co-Regulation, Active State High Assurance, Legitimate Interest
- CR-L4-VI: Co-Regulation, Active State High Assurance, Vital Interest
- CR-L4-PUB: Co-Regulation, Active State High Assurance, Public Interest

---

## Appendix B: DPV Context JSON-LD Examples

*(Complete DPV-structured JSON-LD examples for select contexts from each vector and TATA level)*

---

## Appendix C: Reference Implementation Code

*(Python and JavaScript code examples for DPV parsing, preference matching, GPC signal handling, and ANCR generation)*

---

## Appendix D: GPRC Implementation in W3C DPV Report

### D.1 Overview: Global Privacy Rights Controls (GPRC)

**GPRC** provides standardized machine-readable interfaces for exercising eight derivative digital transparency controls that extend beyond traditional data protection rights (access, portability, correction, erasure).

**Eight Derivative Controls**:

1. Notice Before Collection
2. Autonomous Withdrawal
3. Surveillance Risk Disclosure
4. Scope Escalation Consent
5. Purpose Limitation Enforcement
6. Third-Party Disclosure Transparency
7. Automated Decision Review
8. Data Lifecycle Visibility

**GPRC in v1 Profile**: ISO/IEC 27560:2025-1 Universal Notice Receipt Profile implements GPRC through `privacy_rights_controls` field structure.

### D.2 Integration Point 1: Section 3.2 - MyTerms Preference File

**Add GPRC field showing derivative control availability**:

```json
{
  "@context": ["[https://w3id.org/dpv](https://w3id.org/dpv)", "[https://w3id.org/dpv-ancr](https://w3id.org/dpv-ancr)"],
  "preferences": {
    "gprc_controls": {
      "notice_before_collection": {
        "status": "active",
        "dpv:hasRight": "dpv:TransparencyBeforeProcessing",
        "requirement": "Controller-ID first disclosure required"
      },
      "autonomous_withdrawal": {
        "status": "active",
        "dpv:hasRight": "dpv:WithdrawConsent",
        "mechanism": ["dpv:GlobalPrivacyControl", "consent_wallet_api"],
        "requirement": "Immediate cessation without controller approval"
      },
      "surveillance_risk_disclosure": {
        "status": "active",
        "dpv:hasRight": "dpv:RiskDisclosure",
        "requirement": "Transparency about inference and profiling risks"
      },
      "scope_escalation_consent": {
        "status": "required",
        "dpv:hasRight": "dpv:SpecificConsent",
        "requirement": "New consent required for purpose changes"
      },
      "purpose_limitation_enforcement": {
        "status": "active",
        "dpv:hasRight": "dpv:PurposeLimitation",
        "enforcement": "Technical controls prevent purpose drift"
      },
      "third_party_disclosure_transparency": {
        "status": "active",
        "dpv:hasRight": "dpv:ThirdPartyDisclosure",
        "requirement": "Notice before each disclosure event"
      },
      "automated_decision_review": {
        "status": "active",
        "dpv:hasRight": "dpv:AutomatedDecisionReview",
        "mechanism": "Human review request interface"
      },
      "data_lifecycle_visibility": {
        "status": "active",
        "dpv:hasRight": "dpv:DataLifecycleTransparency",
        "visibility": "Notice Event Log access"
      }
    }
  }
}
```

**DPV Terms Recommended for GPRC**:

- `dpv:TransparencyBeforeProcessing` - Notice before collection control
- `dpv:WithdrawConsent` - Autonomous withdrawal (existing term)
- `dpv:RiskDisclosure` - Surveillance risk disclosure control
- `dpv:SpecificConsent` - Scope escalation consent requirement (existing term)
- `dpv:PurposeLimitation` - Purpose limitation enforcement (existing term)
- `dpv:ThirdPartyDisclosure` - Third-party disclosure transparency (existing term)
- `dpv:AutomatedDecisionReview` - Review of automated decisions (existing term)
- `dpv:DataLifecycleTransparency` - Data lifecycle visibility control

### D.3 Integration Point 2: Section 4.4 - 72 Contexts

**Map GPRC availability by context**:

| Context | Vector | TATA Level | Legal Basis | GPRC Controls Available |
| --- | --- | --- | --- | --- |
| PC-L1-CON | Personal Control | L1: Self-Assertion | Consent | #1 (Notice Before Collection), #2 (Autonomous Withdrawal), #4 (Scope Escalation) |
| PC-L3-CON | Personal Control | L3: ANCR | Consent | #1-8 (All controls via cryptographic ANCR) |
| DP-L2-LI | Data Protection | L2: Registered | Legitimate Interest | #1 (Notice), #3 (Risk Disclosure), #6 (Third-Party Transparency), #8 (Lifecycle Visibility) |
| DP-L4-CON | Data Protection | L4: Active State | Consent | #1-8 (All controls with real-time signaling) |
| CR-L2-CON | Co-Regulation | L2: Registered | Consent | #1, #2, #4, #5, #6 (Core consent controls) |
| CR-L3-LI | Co-Regulation | L3: ANCR | Legitimate Interest | #1, #3, #6, #7, #8 (Transparency-focused controls) |
| CR-L4-CON | Co-Regulation | L4: Active State | Consent | #1-8 (All controls with certified DTTO) |

**GPRC Control Availability Rules**:

- **Controls #1-3** (Notice, Withdrawal, Risk): Available at all TATA levels when Controller-ID first is implemented
- **Controls #4-6** (Scope, Purpose, Third-Party): Available L2+ with Notice Event Log
- **Controls #7-8** (Automated Review, Lifecycle): Available L3+ with ANCR infrastructure
- **All 8 controls**: Available L4 with active state signaling

### D.4 Integration Point 3: Section 6.1 - DPV Extensions

**Propose new GPRC terms for DPV vocabulary**:

```jsx
// GPRC Framework Terms
dpv:PrivacyRightsControl a rdfs:Class ;
  rdfs:label "Privacy Rights Control" ;
  rdfs:comment "Operational mechanism for exercising privacy rights per GPRC framework. Extends beyond traditional data protection rights to include derivative digital transparency controls." ;
  rdfs:seeAlso <[https://w3id.org/dpv#Right](https://w3id.org/dpv#Right)> .

dpv:hasRightsControl a rdf:Property ;
  rdfs:domain dpv:DataProcessingContext ;
  rdfs:range dpv:PrivacyRightsControl ;
  rdfs:label "has rights control" ;
  rdfs:comment "Links a data processing context to available privacy rights control mechanisms." .

// Eight Derivative Controls
dpv:NoticeBeforeCollection a dpv:PrivacyRightsControl ;
  rdfs:label "Notice Before Collection" ;
  rdfs:comment "Controller-ID first disclosure before any PII collection begins." ;
  dpv:hasLegalBasis "Convention 108+ Article 8.2" .

dpv:AutonomousWithdrawal a dpv:PrivacyRightsControl ;
  rdfs:label "Autonomous Withdrawal" ;
  rdfs:comment "Individual can withdraw consent without controller approval; immediate cessation." ;
  rdfs:subClassOf dpv:WithdrawConsent ;
  dpv:hasLegalBasis "GDPR Article 7(3), Convention 108+ Article 5(2)" .

dpv:SurveillanceRiskDisclosure a dpv:PrivacyRightsControl ;
  rdfs:label "Surveillance Risk Disclosure" ;
  rdfs:comment "Transparent disclosure of inference, profiling, and surveillance risks." ;
  dpv:hasLegalBasis "GDPR Article 35, Convention 108+ Article 8.4" .

dpv:ScopeEscalationConsent a dpv:PrivacyRightsControl ;
  rdfs:label "Scope Escalation Consent" ;
  rdfs:comment "New consent required when processing purpose or scope changes." ;
  rdfs:subClassOf dpv:SpecificConsent ;
  dpv:hasLegalBasis "GDPR Article 6(1)(a), Convention 108+ Article 5(2)" .

dpv:PurposeLimitationEnforcement a dpv:PrivacyRightsControl ;
  rdfs:label "Purpose Limitation Enforcement" ;
  rdfs:comment "Technical controls enforce purpose limitation; prevent purpose drift." ;
  dpv:hasLegalBasis "Convention 108+ Article 5(4)(b)" .

dpv:ThirdPartyDisclosureTransparency a dpv:PrivacyRightsControl ;
  rdfs:label "Third-Party Disclosure Transparency" ;
  rdfs:comment "Notice provided before each third-party disclosure event." ;
  dpv:hasLegalBasis "Convention 108+ Article 8.2, GDPR Article 14" .

dpv:AutomatedDecisionReview a dpv:PrivacyRightsControl ;
  rdfs:label "Automated Decision Review" ;
  rdfs:comment "Individual can request human review of automated decisions." ;
  dpv:hasLegalBasis "GDPR Article 22, Convention 108+ Article 9" .

dpv:DataLifecycleVisibility a dpv:PrivacyRightsControl ;
  rdfs:label "Data Lifecycle Visibility" ;
  rdfs:comment "Individual has visibility into complete data lifecycle via Notice Event Log." ;
  dpv:hasLegalBasis "Convention 108+ Article 8.2, Article 9" .

// GPRC Field Mapping
dpv:privacyRightsControlsField a rdf:Property ;
  rdfs:domain dpv:NoticeReceipt ;
  rdfs:range dpv:PrivacyRightsControl ;
  rdfs:label "privacy rights controls field" ;
  rdfs:comment "ISO/IEC 27560:2025-1 privacy_rights_controls field structure." .
```

### D.5 Example: Complete GPRC Implementation in Notice Receipt

```json
{
  "@context": ["[https://w3id.org/dpv](https://w3id.org/dpv)", "[https://w3id.org/dpv-ancr](https://w3id.org/dpv-ancr)"],
  "receipt_header": {
    "schema_version": "27560-COMMON-NOTICE-2025-1.0",
    "dpv:hasContext": "CR-L3-CON"
  },
  "privacy_rights_controls": {
    "dpv:hasRightsControl": [
      {
        "control_type": "dpv:NoticeBeforeCollection",
        "status": "active",
        "implementation": "Controller-ID disclosed via /.well-known/transparency before PII requested",
        "evidence": "notice_event_log_entry_001"
      },
      {
        "control_type": "dpv:AutonomousWithdrawal",
        "status": "active",
        "mechanism": ["dpv:GlobalPrivacyControl", "consent_wallet_revocation"],
        "processing_impact": "immediate_cessation",
        "evidence": "withdrawal_protocol_specification"
      },
      {
        "control_type": "dpv:SurveillanceRiskDisclosure",
        "status": "active",
        "risks_disclosed": [
          "behavioral_profiling",
          "cross_context_tracking",
          "inference_generation"
        ],
        "evidence": "risk_assessment_sha256:abc123..."
      },
      {
        "control_type": "dpv:ScopeEscalationConsent",
        "status": "required",
        "trigger_conditions": [
          "new_processing_purpose",
          "new_third_party_recipient",
          "new_pii_category_collection"
        ],
        "consent_mechanism": "ANCR_renewal_required"
      },
      {
        "control_type": "dpv:PurposeLimitationEnforcement",
        "status": "active",
        "technical_controls": [
          "purpose_binding_in_data_tags",
          "automated_purpose_compliance_checks"
        ],
        "audit_frequency": "continuous"
      },
      {
        "control_type": "dpv:ThirdPartyDisclosureTransparency",
        "status": "active",
        "notification_method": "notice_event_log_append",
        "disclosure_log_url": "[https://example.com/transparency/disclosure-log](https://example.com/transparency/disclosure-log)"
      },
      {
        "control_type": "dpv:AutomatedDecisionReview",
        "status": "active",
        "review_request_endpoint": "[https://example.com/privacy/review-request](https://example.com/privacy/review-request)",
        "response_time_sla": "72_hours"
      },
      {
        "control_type": "dpv:DataLifecycleVisibility",
        "status": "active",
        "notice_event_log_url": "[https://example.com/.well-known/transparency/event-log](https://example.com/.well-known/transparency/event-log)",
        "query_interface": "REST_API"
      }
    ]
  },
  "pii_processing": {
    "dpv:legalBasis": "dpv:Consent",
    "dpv:purpose": ["dpv:ServiceProvision"],
    "tata_level": "L3_ANCR",
    "vector": "Co-Regulation"
  }
}
```

### D.6 GPRC Benefits for DPV Implementation

**Machine-readable control expression**: GPRC provides standardised vocabulary for expressing operational privacy controls

**Beyond traditional rights**: Extends access/erasure/portability to include transparency-enabling controls

**Context-aware implementation**: 72-context framework shows which controls are available at each vector/level/basis combination

**Regulatory alignment**: Maps to Convention 108+ Article 5 (legal bases) and Article 8 (transparency requirements)

**Automated verification**: TPI-R assessment can check GPRC implementation using DPV-structured queries

### D.7 Recommendations for GPRC in W3C DPV

**Short-term**: Add eight derivative control terms to DPV vocabulary (Section D.4)

**Medium-term**: Document GPRC availability rules for 72 contexts (Section D.3)

**Long-term**: Integrate GPRC with MyTerms preference matching architecture (Section D.2)

**Coordination**: Work with ISO/IEC JTC 1/SC 27/WG 5 on `privacy_rights_controls` field standardization

---

## Appendix E: Glossary of Terms

**ANCR**: Anchored Notice and Consent Receipt - bilateral proof of notice delivery

**Controller-ID First**: Architectural principle requiring controller identification disclosure before PII collection

**DPV**: W3C Data Privacy Vocabulary - semantic vocabulary for privacy concepts

**GPC**: Global Privacy Control - W3C specification for automated opt-out signals

**GPRC**: Global Privacy Rights Controls - standardized machine-readable interfaces for exercising eight derivative digital transparency controls (Notice Before Collection, Autonomous Withdrawal, Surveillance Risk Disclosure, Scope Escalation Consent, Purpose Limitation Enforcement, Third-Party Disclosure Transparency, Automated Decision Review, Data Lifecycle Visibility). Implemented via ISO/IEC 27560:2025-1 privacy_rights_controls field

**MyTerms**: Preference matching architecture enabling individuals to publish privacy preferences

**TATA**: Transparency and Trust Assurance - four-level framework for verification and auditability

**TPI-R**: Transparency Performance Indicator - Reporting - assessment methodology for transparency compliance

**72 Contexts**: Framework of 3 vectors × 4 TATA levels × 6 legal bases = 72 data control governance configurations
