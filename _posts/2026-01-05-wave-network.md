---
layout: post
title: "Wave Network Exploration"
permalink: /wave-network-exploration/
---

What happens if you replace attention with waves? That's the idea behind Wave Network. Claude and I spent a little time together seeing how far it could go.

## What is Wave Network?

Wave Network ([paper](https://arxiv.org/pdf/2411.02674)) uses complex number wave operations (modulation and interference) instead of attention. The key insight is to treat embeddings like signals and use constructive/destructive interference to find the relationships. This should give O(n) complexity instead of O(n²), like attention exhibits.

## Interesting Results

I did not perform extensive testing and I don't follow any rigorous methodology, but hopefully I document things well enough for people to understand and even point out stuff that could be wrong!

### Text (close to BERT with 4.5x fewer params)

*Note: while reviewing the paper against my implementation, I discovered that positional encoding for the models was left out, which affected the tests where word order matters. I made the fix in the repo but have not re-run any tests. I expect this will help make it even better!*

| Dataset | Wave | BERT | Params |
|---------|------|------|--------|
| AG News | 92.0% | 94.6% | 24.6M vs 109M |
| DBpedia | 98.2% | 99.3% | 24.6M vs 109M |

### Vision (beats ViT with 4x fewer params)

This was fun to explore and probably could use a lot more testing and validation.

CNN-Wave hybrid hit 92.72% on CIFAR-10 vs ViT's 90.92%, with 1.6M vs 6.3M params respectively. The real boost came from a hybrid approach using CNN for local features and wave for global.

### Audio (keyword detection)

This one I've got a demo for that runs in browser with ONNX and WASM. It was exciting to watch this one train because "it kept getting better", which makes sense to me because audio ... wave ... yeah. That's the depth of my technical commentary.

92.6% on Speech Commands (35 keywords) with a tiny 584K param model at 2.4ms inference! Cool.

## What Didn't Work

- Natural language inference (reasoning tasks). Wave Networks struggle here, but why? Claude says stuff like "no pairwise token interaction" and "no cross-sentence attention", which makes sense to me as far as I can understand those things. 
- Pure wave models for vision tasks. Those test runs were not great (adding the CNN hybrid was needed).

## AI-assisted Development

I used Claude for most of the implementation, partially because I am not great with the maths, and another part because I'm experimenting constantly with what "AI-assisted" looks like for someone like me.

Opus 4.5 has come so far and it generally does a marvelous job when used appropriately, like any tool. Exploring ideas, running tests and comparing results, and generally explaining things that I'm clueless about, work well.

Deciding what to look at, how to apply architecture decisions, and overall guidance ... that's why I'm still here.

## What's Next

More focus on audio and vision could prove interesting, since these are off to a strong start. Maybe a more substantial dataset? For edge use cases that need to have wave-based detection, looking more at the tiny model performance could be beneficial.

## Try It Yourself

- [Live demo](https://kevinraymond.github.io/wave-network)
- [wave-network repo](https://github.com/kevinraymond/wave-network)
