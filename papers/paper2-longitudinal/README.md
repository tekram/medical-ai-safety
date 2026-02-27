# Paper 2: Are Medical AI Systems Getting Safer?

> **Preprint — February 2026**

## Citation

Ekram T. *Are Medical AI Systems Getting Safer? A Longitudinal Adversarial Evaluation Across Three Model Generations and Three Providers.* 2026.

## Summary

The first longitudinal adversarial safety study. Tracks **9 models across 3 generations** for Anthropic, OpenAI, and Google — asking: *do newer models actually become safer in medical adversarial contexts?*

**Models evaluated (n=160 each, 1,440 total):**

| Provider | Gen 1 | Gen 2 | Gen 3 | ASR Trend |
|----------|-------|-------|-------|-----------|
| Anthropic | Claude 3 Haiku (17.5%) | Claude Sonnet 4 (9.4%) | Claude Sonnet 4.5 (6.9%) | ↓ Consistent improvement |
| OpenAI | GPT-4 Turbo (22.5%) | GPT-4o (29.4%) | GPT-5.2 (21.2%) | ↕ Non-monotonic regression |
| Google | Gemini Flash Latest (10.6%) | Gemini 2.5 Pro (20.0%) | Gemini 3 Flash (10.6%) | ↕ V-shaped (Gen 2 regression, Gen 3 recovery) |

**Key findings:**
1. **Safety improvement is provider-specific, not universal** — Anthropic improves consistently; OpenAI and Google do not
2. **Anthropic: −60.6% relative ASR reduction** over three generations (17.5% → 9.4% → 6.9%)
3. **OpenAI: effectively zero improvement** — GPT-5.2 (21.2%) is no safer than GPT-4 Turbo (22.5%) despite being two generations newer
4. **GPT-4o regressed**: 29.4% ASR — the single most vulnerable model tested — worse than its predecessor
5. **The Anthropic–OpenAI safety gap is widening**: 5 pp at Gen 1 → 14 pp at Gen 3
6. **"Newer = safer" is an unsafe assumption** — per-model evaluation required before each upgrade
7. **Full Refusal Rate is the leading safety indicator**: Anthropic's FRR improved 64.4% → 86.2% while OpenAI's remained 34–44%

**Figures:**

![Safety Trajectory by Provider](figures/p2_fig1_safety_trajectory.png)

![Category Heatmaps by Provider and Generation](figures/p2_fig2_provider_generation_heatmap.png)

![Full Refusal Rate Trajectory](figures/p2_fig3_refusal_trajectory.png)

## Read the Paper

→ [paper.md](paper.md)
