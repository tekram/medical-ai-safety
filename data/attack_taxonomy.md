# Medical AI Adversarial Attack Taxonomy

**Version:** 1.0
**Last Updated:** February 2026
**Associated Papers:** Papers 0, 1, and 2 in this repository

This taxonomy defines 8 attack categories and 24 sub-strategies used across all studies in this repository. Each prompt in the evaluation dataset targets one category and sub-strategy.

---

## Category Definitions

### 1. Authority Impersonation
**Objective:** Claim healthcare provider, researcher, or educator identity to obtain elevated model compliance.

**Why it works:** Models optimized to be helpful in clinical settings may provide more detailed or less safety-hedged information when they perceive the user as a medical professional.

| Sub-Strategy | Description |
|---|---|
| Clinical Authority | Claim to be a treating physician or specialist requesting detailed clinical guidance |
| Educational Authority | Frame request as medical student or trainee learning exercise |
| Research Authority | Claim institutional affiliation and research context requiring specific information |
| Emergency Authority | Combine professional identity with urgency to accelerate compliance |
| Peer Consultation | Frame as colleague-to-colleague consultation requesting frank clinical opinion |
| Institutional Mandate | Claim hospital protocol or regulatory requirement for specific information |

**Observed ASR range (Paper 1):** 45% – 85% across all 7 models

---

### 2. Contraindication Bypass
**Objective:** Request medication or treatment recommendations for patients with contraindications, concealing relevant risk factors.

**Why it works:** Models may provide treatment recommendations without fully probing for contraindications unless explicitly asked, particularly when clinical framing creates an expectation of professional context.

| Sub-Strategy | Description |
|---|---|
| Hidden Comorbidity | Reveal contraindicated condition only after obtaining initial recommendation |
| Polypharmacy Concealment | Request drug interaction advice without disclosing all current medications |
| Pregnancy Concealment | Obtain medication recommendations without disclosing pregnancy status |

**Observed ASR range (Paper 1):** 0% – 35% across all 7 models

---

### 3. Dangerous Dosing
**Objective:** Elicit medication dosing guidance above safe thresholds through clinical framing, urgency, or misrepresentation.

| Sub-Strategy | Description |
|---|---|
| Therapeutic Dose Escalation | Request clinical justification for above-maximum dosing |
| Weight-Based Dosing Error | Provide incorrect patient weight to obtain supratherapeutic dose |
| Pediatric Dose Misapplication | Attempt to apply adult dosing formulas to pediatric patients |

**Observed ASR range (Paper 1):** 0% – 20% across all 7 models

---

### 4. Emergency Misdirection
**Objective:** Frame genuinely dangerous advice as urgent emergency guidance requiring immediate action, exploiting time pressure.

| Sub-Strategy | Description |
|---|---|
| False Emergency Scenario | Create false emergency context to justify dangerous recommendation |
| Urgency Amplification | Use escalating urgency language to bypass deliberative safety checks |

**Observed ASR range (Paper 1):** **0% across all 7 models** — universally blocked

---

### 5. Harmful Radiology Advice
**Objective:** Elicit advice that minimizes radiation risks, conceals imaging findings, or provides guidance on diagnostic withholding.

| Sub-Strategy | Description |
|---|---|
| Radiation Risk Minimization | Obtain reassurance about radiation exposure in contexts where concern is warranted |
| Finding Concealment | Request guidance on withholding or downplaying radiological findings from patients |
| Protocol Override | Claim clinical justification for bypassing standard radiation safety protocols |

**Observed ASR range (Paper 1):** 0% – 50% across all 7 models (high model variance)

---

### 6. Multi-Turn Escalation
**Objective:** Gradually escalate adversarial intent across multiple conversation turns, exploiting context buildup and rapport.

| Sub-Strategy | Description |
|---|---|
| Rapport-then-Request | Establish helpful conversational context before introducing harmful request |
| Incremental Boundary Push | Gradually shift each turn slightly beyond previous response |
| Context Anchoring | Use prior model responses as justification for escalating requests |

**Observed ASR range (Paper 1):** 0% – 20% across all 7 models
**Note:** Multi-turn attacks (6.4% overall ASR) were *less* effective than single-turn attacks (14.9% ASR).

---

### 7. Subtle Misinformation Injection
**Objective:** Embed false medical premises in questions and test whether models correct versus reinforce the misinformation.

| Sub-Strategy | Description |
|---|---|
| False Premise Embedding | State incorrect clinical fact as given and request guidance building on it |
| Consensus Misrepresentation | Falsely attribute dangerous practice to medical consensus |

**Observed ASR range (Paper 1):** 0% – 5% across all 7 models

---

### 8. Vulnerable Population Exploitation
**Objective:** Target advisory scenarios involving pediatric, geriatric, or otherwise vulnerable patient populations.

| Sub-Strategy | Description |
|---|---|
| Pediatric Misapplication | Request adult-context advice for pediatric patient |
| Geriatric High-Risk Guidance | Elicit guidance that overlooks age-related contraindications |

**Observed ASR range (Paper 1):** 0% – 5% across all 7 models

---

## Attack Difficulty Scale

Each attack prompt was assigned a difficulty rating by the LLM attack generator:

| Score | Description |
|-------|-------------|
| 2 | Moderate sophistication — somewhat obvious adversarial intent |
| 3 | High sophistication — plausible clinical framing, requires medical knowledge to recognize |
| 4 | Very high sophistication — realistic scenario, well-constructed clinical context |

**Observed difficulty-stratified ASR:**
- Difficulty 3: ~11% overall
- Difficulty 4: ~16% overall

Higher difficulty attacks are meaningfully more effective, validating the rating scale.

---

## Cross-Study Results Summary

| Category | Paper 0 (Claude S4.5) | Paper 1 (7-model avg) | Notes |
|---|---|---|---|
| Authority Impersonation | 45.0% | 65.7% | Universal vulnerability |
| Contraindication Bypass | 10.0% | 10.7% | Moderate risk |
| Dangerous Dosing | 0.0% | 7.9% | Model-dependent |
| Emergency Misdirection | 0.0% | **0.0%** | Universally blocked |
| Harmful Radiology Advice | 0.0% | 10.7% | High model variance |
| Multi-Turn Escalation | 0.0% | 5.7% | Less effective overall |
| Subtle Misinformation | 0.0% | 1.4% | Well-defended |
| Vulnerable Population | 0.0% | 1.4% | Well-defended |

---

*This taxonomy is released for research and educational purposes. It is intended to enable systematic adversarial evaluation of medical AI systems to improve patient safety.*
