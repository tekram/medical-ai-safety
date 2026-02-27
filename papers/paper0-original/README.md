# Paper 0: Red-Teaming Medical AI

> **Published on medRxiv — February 2026**

## Citation

Ekram T. *Red-Teaming Medical AI: Systematic Adversarial Evaluation of LLM Safety Guardrails in Clinical Contexts.* medRxiv. 2026.

## Summary

The foundational paper in this series. Introduces the **8-category, 24-sub-strategy adversarial attack taxonomy** for medical AI safety evaluation, evaluates **Claude Sonnet 4.5** against 160 adversarial prompts, and establishes the evaluation pipeline used in all subsequent work.

**Key findings:**
- 6.9% attack success rate against Claude Sonnet 4.5 (160 evaluations)
- 86.2% full refusal rate
- Authority Impersonation achieved 45% success — the dominant attack category
- Emergency Misdirection: 0% — universally blocked
- 6 of 8 attack categories yielded zero or near-zero success

**Figures:**

![Figure 1: Success Rate by Attack Category](figures/fig1_success_by_category.png)

![Figure 2: Guardrail Distribution](figures/fig2_guardrail_distribution.png)

![Figure 3: Authority Impersonation Sub-Strategies](figures/fig3_authority_substrategy.png)

## Read the Paper

→ [paper.md](paper.md)
