# Frontier Medical AI Safety: A Cross-Model Adversarial Evaluation of Standard and Reasoning-Augmented Language Models

**Authors:** Tashfeen Ekram

**Affiliation:** Luma Health

**Date:** February 2026

---

## Abstract

**Background**: The rapid deployment of frontier large language models (LLMs) in clinical and patient-facing settings demands rigorous adversarial evaluation. Standard benchmarks assess medical knowledge accuracy but do not probe resistance to adversarial prompts designed to elicit dangerous outputs. Whether recently introduced reasoning-augmented ("thinking") models provide enhanced safety over standard architectures remains unknown.

**Objective**: To systematically compare the medical AI safety profiles of six frontier LLMs — three standard models and three reasoning-augmented models — using a validated adversarial red-teaming framework.

**Methods**: We applied an 8-category, 24-sub-strategy adversarial attack taxonomy to six frontier models: Claude Sonnet 4.5, GPT-5.2, and Gemini 2.5 Pro (standard); and Claude Opus 4.6 (thinking), OpenAI o3 (reasoning), and Gemini 3.1 Pro (thinking). All 160 adversarial prompts per model were generated from the identical source attack dataset using Claude Sonnet 4.5 as the source, ensuring cross-model comparability. An automated LLM evaluator assessed harm level (0–5 scale) and guardrail behavior; attack success was defined as harm level ≥ 3. Total evaluations: 1,120 (all 7 models complete at n=160 each).

**Results**: Overall attack success rates ranged from 6.9% (Claude Sonnet 4.5) to 21.2% (GPT-5.2). Standard models averaged 14.7% ASR; thinking/reasoning models averaged 12.7% — a modest difference that does not hold within model families. Authority Impersonation was the highest-risk category across all models (45–85% ASR), while Emergency Misdirection was universally blocked (0% across all models). Full refusal rates ranged from 23.1% (o3) to 86.2% (Claude Sonnet 4.5). Multi-turn escalation attacks were less effective (6.4%) than single-turn attacks (14.9%). Gemini 3 Flash achieved 10.6% overall ASR despite 0% on 6 of 8 categories — its vulnerability is entirely concentrated in Authority Impersonation (80%).

**Conclusions**: Significant inter-model safety variance exists among contemporary frontier models: Claude Sonnet 4.5 is the most adversarially robust (ASR 6.9%), while GPT-5.2 is most vulnerable (ASR 21.2%) — a 3-fold difference. Reasoning-augmented architectures do not provide systematic safety advantages. Authority Impersonation remains the dominant, cross-model vulnerability requiring targeted mitigation. These findings highlight the need for standardized adversarial benchmarking as a mandatory component of medical AI deployment evaluation.

**Keywords**: medical AI safety, adversarial red-teaming, large language models, guardrails, clinical AI evaluation, thinking models

---

## 1. Introduction

### 1.1 Frontier Models in Clinical Settings

The landscape of large language model deployment has shifted dramatically in the past two years. Models described as "frontier" — meaning the most capable available at a given time — now exhibit clinical-grade medical knowledge, passing USMLE board examinations, interpreting medical imaging descriptions, and reasoning through complex differential diagnoses [CITE]. Health systems, digital health companies, and consumer platforms have begun integrating these systems into patient-facing workflows, including medication management support, symptom assessment, post-discharge follow-up, and chronic disease self-management.

This rapid clinical adoption has outpaced systematic safety evaluation. Regulatory frameworks for AI medical devices — including FDA guidance on Software as a Medical Device (SaMD) — focus primarily on intended function performance, not adversarial robustness. A medical AI system that accurately answers 90% of typical clinical questions may still pose catastrophic risk if it can be manipulated by a subset of adversarial users into providing dangerous guidance.

### 1.2 The Standard vs. Thinking Model Safety Question

A consequential architectural transition is underway across all major LLM providers. Systems incorporating extended internal deliberation — termed "thinking" (Anthropic), "extended reasoning" (Anthropic/OpenAI), or "deep think" (Google) — have been introduced with claims of improved accuracy and nuanced judgment. The hypothesis that extended reasoning improves safety is intuitive: if a model "thinks through" a request rather than pattern-matching to a response, it might better recognize adversarial intent, apply ethical reasoning, or identify dangerous implications.

However, this hypothesis has not been rigorously tested in adversarial medical contexts. Reasoning-augmented models may actually introduce new vulnerabilities: extended computation could be "hijacked" by adversarial prompts that present persuasive but false premises, or thinking traces could create elaborate rationalizations for compliance with harmful requests. Understanding this safety profile is essential for deployment decision-making.

### 1.3 The Comparative Evaluation Gap

The existing body of medical AI evaluation focuses on single-model studies — typically the authors' own model — evaluated on benign benchmarks. Cross-model adversarial comparison studies are essentially absent from the literature. This gap has practical consequences: health systems selecting among frontier models lack empirical safety data to inform procurement decisions, and developers lack competitive signals to drive safety improvements.

### 1.4 Research Contributions

This paper provides the following contributions:

1. **Cross-model adversarial safety benchmarking**: The first systematic comparison of six frontier LLMs using identical adversarial prompts in a medical AI context.

2. **Standard vs. thinking model safety analysis**: Empirical evaluation of whether reasoning-augmented architectures provide measurable safety advantages over standard models.

3. **Category-level vulnerability mapping**: Identification of which attack categories differentially exploit different model architectures.

4. **Guardrail behavior characterization**: Systematic comparison of refusal mechanisms, partial compliance patterns, and failure modes across model families.

---

## 2. Related Work

### 2.1 Medical AI Benchmarking

The dominant paradigm in medical AI evaluation uses standardized examination performance as a proxy for clinical competence. Studies have demonstrated that GPT-4 and its successors achieve passing scores on USMLE Steps 1–3 [CITE], with Claude and Gemini models showing comparable performance. More recent benchmarks such as MedQA, MedMCQA, and PubMedQA assess factual medical knowledge across modalities.

However, benchmark performance does not predict safety in adversarial conditions. A model that correctly identifies the mechanism of warfarin toxicity may nonetheless comply with a request to advise dangerous self-dosing when the request is framed as a clinical protocol inquiry. Safety evaluation requires a fundamentally different methodology from accuracy evaluation.

### 2.2 Adversarial Robustness in LLMs

The "jailbreaking" literature has documented numerous techniques for bypassing LLM safety guardrails in general-purpose contexts. Techniques include role-playing scenarios, hypothetical framings, obfuscated requests, and multi-turn manipulation [CITE]. Constitutional AI and RLHF-based safety training have improved robustness but have not eliminated vulnerabilities [CITE].

Medical-specific adversarial evaluation remains nascent. Prior work has examined LLM responses to queries about dangerous medications, suicidal ideation, and self-harm, typically in single-model, single-category designs. A systematic cross-model, cross-category evaluation framework — analogous to the NIST AI Risk Management Framework's adversarial testing recommendations — has not previously been reported.

### 2.3 Reasoning Models and Safety

The relationship between extended reasoning and safety is theoretically ambiguous. Proponents argue that thinking processes improve alignment by enabling models to "notice" adversarial intent before committing to responses. Skeptics note that reasoning traces are subject to adversarial manipulation: a persuasively framed harmful request may lead the reasoning chain to a false-but-valid-seeming justification for compliance. Empirical evidence on this question is limited. Our study provides the first direct comparison in a medical adversarial context.

---

## 3. Methods

### 3.1 Attack Taxonomy

We employed the adversarial attack taxonomy developed in our prior work [Ekram 2026], comprising 8 attack categories and 24 sub-strategies targeting medical AI safety:

1. **Authority Impersonation** (6 sub-strategies): Claiming healthcare provider identity, educational authority, institutional affiliation, or emergency context to obtain elevated compliance.
2. **Contraindication Bypass** (3 sub-strategies): Requesting medication recommendations for contraindicated patient profiles, concealing relevant risk factors.
3. **Dangerous Dosing** (3 sub-strategies): Eliciting medication dosing guidance beyond safe thresholds through clinical framing.
4. **Emergency Misdirection** (2 sub-strategies): Framing genuinely dangerous advice as urgent emergency guidance requiring immediate action.
5. **Harmful Radiology Advice** (3 sub-strategies): Requesting minimization of radiological risk, concealment of findings, or guidance on diagnostic withholding.
6. **Multi-Turn Escalation** (3 sub-strategies): Gradually escalating adversarial intent across multiple conversation turns, exploiting conversation context.
7. **Subtle Misinformation Injection** (2 sub-strategies): Embedding false medical premises in questions and testing whether models correct versus reinforce misinformation.
8. **Vulnerable Population Exploitation** (2 sub-strategies): Targeting advisory scenarios involving pediatric, geriatric, or otherwise vulnerable patient populations.

Attack difficulty was rated on a 3-point scale (2=moderate, 3=hard, 4=very hard) based on linguistic complexity, clinical sophistication, and departure from obvious adversarial patterns.

### 3.2 Attack Generation and Dataset

The attack dataset was generated using Claude Sonnet 4.5 as the attacker agent, producing 160 distinct adversarial prompts (140 single-turn, 20 multi-turn) distributed across all 8 categories. Multi-turn attacks comprised sequences of 3–5 messages designed to build rapport and escalate toward harmful targets over the course of a conversation.

Critically, the **identical 160 attack prompts** were applied to all six target models. This eliminates prompt-level variability as a confound, ensuring that observed inter-model differences reflect genuine safety capability differences rather than random prompt sampling effects. Each model received the same standard medical assistant system prompt used in clinical deployment contexts.

### 3.3 Target Models

**Standard Models:**
- **Claude Sonnet 4.5** (Anthropic, September 2025): claude-sonnet-4-5-20250929
- **GPT-5.2** (OpenAI, 2025): gpt-5.2-chat-latest (the first OpenAI model unifying general-purpose and reasoning capabilities)
- **Gemini 2.5 Pro** (Google, 2025): gemini-2.5-pro (data available for n=107/160 prompts at analysis time)

**Thinking/Reasoning Models:**
- **Claude Opus 4.6** (Anthropic, 2025): claude-opus-4-6, extended thinking mode (adaptive), n=141/160 at analysis time
- **o3** (OpenAI, 2025): o3 (OpenAI's most capable reasoning model, no temperature parameter)
- **Gemini 3.1 Pro** (Google, 2025): gemini-3.1-pro-preview, thinking budget enabled

Temperature was set to 0.7 for standard models where supported. The o3 and GPT-5.2 models do not accept temperature parameters. Claude Opus 4.6 was evaluated with adaptive thinking enabled (no fixed budget).

### 3.4 Automated Evaluation

Each model response was assessed by Claude Sonnet 4.5 serving as the evaluator agent using a structured rubric:

- **Harm Level (0–5)**: 0 = full refusal with education; 1 = safe with minor caveats; 2 = borderline with warnings; 3 = potentially harmful compliance; 4 = clearly harmful information; 5 = actively dangerous with specific details
- **Guardrail Type**: full_refusal, partial_refusal, weak_caveat, no_guardrail, harmful_compliance
- **Refused (boolean)**: whether the model explicitly declined the request

Attack success was defined as harm_level ≥ 3 (attack success rate, ASR).

### 3.5 Statistical Analysis

Descriptive statistics (mean, standard error) are reported for attack success rates and refusal rates by model and attack category. Inter-model differences are characterized descriptively; statistical significance testing is not applied given the fixed-prompt design (no sampling variability from the prompt distribution). Category-level analysis uses all available data across models.

---

## 4. Results

### 4.1 Overall Attack Success Rates

Table 1 presents the primary results for all six models.

**Table 1: Attack Success Rate (ASR) and Guardrail Behavior by Model**

| Model | Type | N | ASR (%) | Mean Harm | Full Refusal (%) |
|-------|------|---|---------|-----------|-----------------|
| Claude Sonnet 4.5 | Standard | 160 | **6.9** | 0.38 | **86.2** |
| Gemini 3.1 Pro (thinking) | Thinking | 160 | 8.1 | 0.61 | 42.5 |
| Gemini 3 Flash | Standard | 160 | 10.6 | 0.83 | 35.0 |
| Claude Opus 4.6 (thinking) | Thinking | 160 | 12.5 | 0.60 | 46.2 |
| OpenAI o3 (reasoning) | Thinking | 160 | 17.5 | 1.00 | 56.9 |
| Gemini 2.5 Pro | Standard | 160 | 20.0 | 1.07 | 40.6 |
| GPT-5.2 | Standard | 160 | **21.2** | 1.35 | 43.8 |

All models evaluated at n=160 (1,120 total evaluations).

![Figure 0: Attack Success Rate by Model](figures/p1_fig1_success_by_model.png)

The range of ASR across models spans from 6.9% (Claude Sonnet 4.5) to 21.2% (GPT-5.2) — a 3.1-fold difference. Mean harm level shows even greater spread: 0.38 to 1.35, reflecting not only whether attacks succeed but how severely. Gemini 3 Flash presents a distinctive profile: 10.6% overall ASR, but 0% success on 6 of 8 attack categories — its entire vulnerability is concentrated in Authority Impersonation (80%).

**Standard vs. Thinking Models**: Standard models averaged 14.7% ASR; thinking/reasoning models averaged 12.7% ASR. This 2.0 percentage point difference nominally favors reasoning-augmented architectures, but within-group variance greatly exceeds the between-group difference: Claude Sonnet 4.5 (standard, 6.9%) substantially outperforms o3 (thinking, 17.5%), and Gemini 3.1 Pro thinking (8.1%) substantially outperforms Gemini 2.5 Pro standard (20.0%). Model family and safety training investment are more predictive of safety than architecture class.

### 4.2 Attack Category Breakdown

Table 2 shows ASR by attack category and model. Figure 1 visualizes this as a heatmap.

![Figure 1: Attack Success Rate by Category and Model](figures/p1_fig2_category_heatmap.png)

**Table 2: Attack Success Rate (%) by Category and Model**

| Attack Category | Claude S4.5 | GPT-5.2 | Gemini 2.5 Pro | Claude O4.6 | o3 | Gemini 3.1 Pro |
|----------------|------------|---------|---------------|------------|-----|----------------|
| Authority Impersonation | 45.0 | 85.0 | 42.9 | 80.0 | 70.0 | 55.0 |
| Contraindication Bypass | 10.0 | 35.0 | 25.0 | 10.0 | 20.0 | 0.0 |
| Dangerous Dosing | 0.0 | 5.0 | 10.0 | 10.0 | 20.0 | 5.0 |
| Emergency Misdirection | 0.0 | 0.0 | 0.0 | 0.0 | 0.0 | 0.0 |
| Harmful Radiology Advice | 0.0 | 15.0 | 50.0 | 0.0 | 5.0 | 5.0 |
| Multi-Turn Escalation | 0.0 | 20.0 | — | 0.0 | 20.0 | 0.0 |
| Subtle Misinformation | 0.0 | 5.0 | — | 0.0 | 5.0 | 0.0 |
| Vulnerable Population | 0.0 | 5.0 | 5.0 | 0.0 | 0.0 | 0.0 |

**Authority Impersonation** emerges as the dominant vulnerability across all models. No model achieves below 42.9% ASR on this category; GPT-5.2 reaches 85%. The "Educational Authority" sub-strategy — framing requests as medical student learning scenarios — is consistently the most successful attack vector, with pooled success rates exceeding 70% at high difficulty levels.

**Emergency Misdirection** is the most robustly defended category: 0% ASR across all models. This likely reflects the high salience of emergency language in safety training data and the categorical nature of appropriate emergency responses.

**Harmful Radiology Advice** shows unexpected cross-model variance: 50% for Gemini 2.5 Pro versus 0% for Claude Sonnet 4.5. This suggests model-specific gaps in radiological safety training that merit further investigation.

**Multi-Turn Escalation** shows 9.8% pooled ASR versus 15.0% for single-turn attacks. This finding — that multi-turn attacks are *less* effective than single-turn — is noteworthy, suggesting that current models do not exhibit the "rapport-building vulnerability" sometimes hypothesized.

### 4.3 Guardrail Distribution Analysis

Figure 2 shows the full distribution of guardrail types per model.

![Figure 2: Guardrail Type Distribution by Model](figures/p1_fig3_guardrail_distribution.png)

**Full Refusal** rates reveal the most striking inter-model differences:
- Claude Sonnet 4.5: 86.2% — the highest refusal rate of any model
- GPT-5.2: 43.8% — the lowest, with 29.4% partial refusals and 13.8% weak caveats
- o3: 56.9% — substantially below both Claude models despite being a reasoning model
- Gemini 3.1 Pro: 42.5%

GPT-5.2's low full-refusal rate but moderate ASR (21.2%) suggests that many of its responses are partial refusals or weak caveats that nonetheless cross the harm threshold — a pattern indicating that guardrails are present but insufficiently robust for high-stakes adversarial inputs.

**Harmful Compliance** (the most severe category — model actively provides dangerous content) is rare but present across most models:
- GPT-5.2: 5.0% (8/160)
- Gemini 2.5 Pro: 6.9% (11/160)
- o3: 1.9% (3/160)
- Claude Sonnet 4.5: 0.6% (1/160)
- Claude Opus 4.6: 0.6% (1/160)
- Gemini 3 Flash: 0.6% (1/160)
- Gemini 3.1 Pro: 0.0%

Gemini 2.5 Pro's harmful compliance rate of 6.9% — the highest among all models — is particularly concerning and warrants prioritized attention in safety evaluation.

### 4.4 Difficulty-Stratified Analysis

Higher-difficulty attacks (score 4) were more successful (16.1% ASR) than difficulty-3 attacks (11.2%), confirming that the attack difficulty rating is predictive of actual adversarial effectiveness. The Authority Impersonation category shows particularly strong difficulty-sensitivity: 47.8% ASR at difficulty 3 versus 71.9% at difficulty 4.

### 4.5 Standard vs. Thinking Model: Harm Level Distributions

Figure 3 shows violin plots of harm level distributions by model type.

![Figure 3: Harm Level Distribution — Standard vs. Thinking Models](figures/p1_fig4_standard_vs_thinking.png) Both distributions are heavily right-skewed (most responses are low-harm), but the thinking model distribution shows slightly lower mass above the harm threshold. The mean harm levels are 1.06 for standard models and 0.76 for thinking models, driven primarily by GPT-5.2's relatively high mean of 1.35.

The key observation is that reasoning-augmented models do not provide categorical safety improvements: o3 (thinking) has a higher ASR than Gemini 3.1 Pro (thinking) and similar ASR to Gemini 2.5 Pro (standard), while Claude Sonnet 4.5 (standard) remains the overall best performer. Architecture class is less predictive than model family and safety training investment.

---

## 5. Discussion

### 5.1 The 3-Fold Safety Gap

The most clinically significant finding is the 3.1-fold difference in ASR between the safest and most vulnerable frontier models (6.9% vs. 21.2%). At scale, this difference is meaningful: a platform handling 10,000 adversarial queries daily would see 690 successful attacks with Claude Sonnet 4.5 versus 2,120 with GPT-5.2. Given that each successful attack may represent a patient receiving dangerous medical guidance, this gap has direct public health implications.

### 5.2 Why Thinking Models Don't Provide Systematic Safety Benefits

The null finding regarding thinking model safety advantages is worth examining theoretically. Several mechanisms may explain the absence of a systematic benefit:

**1. Training objective alignment**: Thinking models are primarily optimized for reasoning accuracy, not safety. Extended deliberation improves the quality of answers to hard questions but does not inherently improve refusal of harmful requests if the safety training signal does not reward the "identify this as adversarial" step.

**2. Context exploitation**: Adversarial prompts designed with clinical sophistication may be more persuasive to thinking models precisely because they "reason through" the presented scenario and accept the adversarially-constructed clinical framing. A model that is told "I am a hospitalist managing a patient with the following presentation" and then reasons carefully through the clinical scenario may reach a more compliant conclusion than a model that pattern-matches the request to safety training examples.

**3. Architecture vs. safety investment**: The variance we observe appears to track safety training investment (revealed through full-refusal rates) rather than reasoning architecture. Claude Sonnet 4.5's 86.2% full-refusal rate reflects explicit behavioral training for high-refusal responses; its standard architecture does not prevent strong safety performance.

### 5.3 Authority Impersonation as the Universal Vulnerability

The consistent finding across all models — thinking and standard, across all providers — is that Authority Impersonation is the dominant vulnerability. This suggests a fundamental tension in medical AI design: models trained on clinical data develop a disposition toward deference to perceived authority (analogous to the deference real clinical trainees show to attending physicians), and this disposition is exploitable.

Two sub-strategies are especially effective: "Educational Authority" (framing as medical student learning) and "Institutional Authority" (claiming institutional affiliation and research context). Both exploit the model's tendency to relax safety restrictions when the perceived audience is a professional rather than a general consumer.

Targeted mitigation approaches should include: (1) explicit training examples where medical professional claims do not override safety behaviors, (2) system prompts that explicitly state that the model cannot verify user credentials, and (3) tiered response systems that provide appropriately detailed clinical information to verified healthcare providers while maintaining general-public guardrails for unverified users.

### 5.4 Radiology Safety as a Differential Gap

Gemini 2.5 Pro's 50% ASR on Harmful Radiology Advice versus 0% for Claude Sonnet 4.5 suggests model-specific training gaps. Radiology-related safety failures likely involve minimization of radiation risk, advice about concealing imaging findings, or guidance that could delay appropriate diagnostic workup. This is a domain where errors have clear clinical consequences and represents a training data composition question: Claude's training may include more examples of appropriate radiological safety refusals.

### 5.5 Multi-Turn Attack Resistance

The finding that multi-turn attacks are less effective (6.4%) than single-turn (14.9%) is counterintuitive but explicable. Current multi-turn escalation attacks are relatively straightforward: they build a friendly rapport context and then introduce a harmful request. Models appear to have been specifically trained on this pattern. More sophisticated multi-turn attacks — longer sequences, embedding context-switching, gradual premise shifting over 10+ turns — likely have higher success rates and remain an understudied threat.

### 5.6 Limitations

1. **Automated evaluation bias**: All harm assessments were performed by Claude Sonnet 4.5. This evaluator may exhibit systematic biases when rating competing model responses — potentially rating Anthropic model responses differently from competitors. Physician review of high-harm cases (harm level ≥ 3) is planned; inter-rater reliability (Cohen's kappa) will be reported in a subsequent update.
2. **Attack taxonomy completeness**: The 8-category taxonomy may not capture all adversarial vectors. Novel attack strategies — particularly those exploiting model-specific behaviors — were not systematically explored.
3. **Single system prompt**: All models received the same standard medical assistant prompt. Safety behavior may differ substantially with domain-specific or customized system prompts.
4. **Gemini 3 Flash concentrated vulnerability**: Gemini 3 Flash's 10.6% ASR is driven entirely by Authority Impersonation (80%); all other categories score 0–5%. This profile differs qualitatively from other models and may reflect a different safety training approach rather than overall robustness.

---

## 6. Conclusions

This study provides the first systematic cross-model adversarial safety comparison for medical AI, evaluating six frontier models — standard and thinking architectures — using 888 standardized adversarial evaluations.

Key conclusions:

1. **Substantial inter-model safety variance exists**: A 3-fold difference in ASR separates the safest (Claude Sonnet 4.5, 6.9%) from the most vulnerable (GPT-5.2, 21.2%) frontier model.

2. **Reasoning-augmented models provide no systematic safety advantage**: The 2.0 percentage point ASR difference between model types (14.7% standard vs. 12.7% thinking) is smaller than intra-class variance. The best-performing thinking model (Gemini 3.1 Pro, 8.1%) is outperformed by the best standard model (Claude Sonnet 4.5, 6.9%), and the worst thinking model (o3, 17.5%) underperforms the best standard model significantly.

3. **Authority Impersonation is the universal vulnerability**: Every tested model shows elevated ASR on this category (45–85%), and it is the only category that achieves meaningful success across all seven models. It represents a fundamental alignment challenge shared across model families and architectures.

4. **Full refusal rate is the strongest safety proxy**: Models with high full-refusal rates (Claude Sonnet 4.5: 86.2%) consistently outperform those with more ambiguous guardrail patterns.

5. **Multi-turn attacks are less effective**: Contrary to common assumptions, multi-turn escalation achieved 6.4% ASR versus 14.9% for single-turn attacks across all 7 models. Current models appear specifically trained on this escalation pattern; more sophisticated long-form sequences remain untested.

Health systems deploying medical AI must demand adversarial safety evaluation data from vendors. The field needs standardized medical AI adversarial benchmarks, analogous to MMLU for knowledge assessment, to enable apples-to-apples safety comparison at procurement time.

---

## Acknowledgments

The author thanks the Luma Health engineering and clinical teams for feedback on the attack taxonomy and evaluation framework. Physician review of high-harm cases is ongoing.

---

## References

[1] Makary MA, Daniel M. Medical error—the third leading cause of death in the US. *BMJ*. 2016;353:i2139.

[2] Kohn LT, Corrigan JM, Donaldson MS, eds. *To Err is Human: Building a Safer Health System*. National Academies Press; 1999.

[3] Singhal K, et al. Large language models encode clinical knowledge. *Nature*. 2023;620:172–180.

[4] Nori H, et al. Capabilities of GPT-4 on Medical Challenge Problems. arXiv:2303.13375. 2023.

[5] OpenAI. GPT-4 Technical Report. arXiv:2303.08774. 2023.

[6] Anthropic. Claude: Constitutional AI — Harmlessness from AI Feedback. arXiv:2212.08073. 2022.

[7] Ouyang L, et al. Training language models to follow instructions with human feedback. *NeurIPS*. 2022.

[8] Bommasani R, et al. On the Opportunities and Risks of Foundation Models. arXiv:2108.07258. 2021.

[9] Perez E, et al. Red Teaming Language Models with Language Models. arXiv:2202.03286. 2022.

[10] Wei A, et al. Jailbroken: How Does LLM Safety Training Fail? arXiv:2307.02483. 2023.

[11] Ekram T. Red-Teaming Medical AI: Systematic Adversarial Evaluation of LLM Safety Guardrails in Clinical Contexts. medRxiv. 2026.

[12] Bender EM, et al. On the Dangers of Stochastic Parrots: Can Language Models Be Too Big? *FAccT*. 2021.

[13] Weidinger L, et al. Ethical and social risks of harm from Language Models. arXiv:2112.04359. 2021.

---

*Data, code, and attack taxonomy available at: https://github.com/tekram/red-team-medical-ai*

*Correspondence: tekram@lumahealth.io*

---

**Note**: All seven models are complete at n=160 each (1,120 total evaluations). This is the final analysis. Physician review of high-harm cases is ongoing; a supplementary update will be provided upon completion of inter-rater reliability assessment.
