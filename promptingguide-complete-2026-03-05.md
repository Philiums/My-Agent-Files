# PromptingGuide.ai - Complete Extraction (Part 2)
**Date:** 2026-03-05
**Scope:** Remaining sections after initial audit

---

## NEW TECHNIQUES FETCHED

### Zero-Shot Prompting
**Core Concept:** Direct instruction without examples
- Models (GPT-4, Claude 3, GPT-3.5) instruction-tuned for this
- RLHF powers ChatGPT's zero-shot capability
- Emerges with scale - larger models better at it
- **Fallback:** When zero-shot fails → use few-shot prompting

**Key insight:** Models already understand concepts like "sentiment" without explicit training

---

## MODEL-SPECIFIC GUIDES

### GPT-4
**Capabilities:**
- Multimodal (text + image input, text output)
- Top 10% on bar exam
- Strong on MMLU, HellaSwag benchmarks
- 128K context window (300+ pages)
- **GPT-4 Turbo:** Improved instruction following, JSON mode, function calling

**Prompting Tips:**
- "Provide step-by-step reasoning" for complex visual tasks
- Few-shot + CoT effective for image tasks
- Better factuality, steerability, alignment than GPT-3.5

### LLaMA (Open Models)
**Range:** 7B to 65B parameters

**Key Results:**
- LLaMA-13B outperforms GPT-3 (175B) on many benchmarks
- 10x smaller, runs on single GPU
- LLaMA-65B competitive with Chinchilla-70B, PaLM-540B
- **Training finding:** 7B model improves even after 1T tokens (more data > larger model)

**Ecosystem:**
- Koala, Vicuna, Alpaca fine-tunes
- GPT4All, ChatDoctor domain adaptations

---

## APPLICATIONS SECTION

**Available Guides:**
1. Context Engineering Guide
2. Fine-tuning GPT-4o

**Scope:** "Advanced and interesting ways to use prompt engineering"

---

## PROMPT HUB

**Purpose:** Collection of prompts to test LLM capabilities
- Fundamental capabilities
- Complex tasks
- Open for community contributions

**Categories:**
- Classification (confirmed)
- (More categories exist but not detailed in fetch)

---

## NOTEBOOKS (Hands-On Guides)

1. **Getting Started with Prompt Engineering**
   - OpenAI + LangChain library basics
   
2. **Program-Aided Language Model (PAL)**
   - Code as reasoning
   - Python interpreter + LLM
   
3. **ChatGPT API Intro**
   - OpenAI library usage
   
4. **ChatGPT API with LangChain**
   - LangChain integration
   
5. **Adversarial Prompt Engineering**
   - Defensive measures
   - Attack vectors

---

## SITE STRUCTURE OVERVIEW

**Techniques Section:**
- Zero-shot Prompting ✅
- Few-shot Prompting ✅
- Chain-of-Thought Prompting ✅
- Tree of Thoughts ✅
- ReAct ✅
- Multimodal CoT ✅
- PAL ✅
- Directional Stimulus Prompting ✅
- (Self-consistency - 404)
- (Generated Knowledge - 404)

**Models Section:**
- ChatGPT ✅
- GPT-4 ✅
- LLaMA ✅
- (Claude - 404)

**Applications:**
- Context Engineering
- Fine-tuning guides

**Resources:**
- Prompt Hub
- Notebooks
- Research papers (arXiv links)

---

## KEY GAPS IDENTIFIED

**404 Errors:**
- self-consistency technique
- generated-knowledge (gk) technique
- claude model guide

**Possible causes:**
- Pages moved/not yet published
- URL structure changed
- Permission/content issues

---

## COST SUMMARY

**This sweep:**
- Pages successfully fetched: 8
- Failed (404): 3
- Estimated tokens: ~25k input, ~8k output
- Cost: ~$0.04

---

## INTEGRATION NOTES

Combine this file with promptingguide-audit-2026-03-05.md for complete coverage.
