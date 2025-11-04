---
created: 2025-11-04
modified: 2025-11-04
note-type:
aliases: []
cssclasses:
title: I Need Comprehensive Research on Common Prompt Engineering Failures, Anti-patterns, and Mistakes to Avoid
---
# I Need Comprehensive Research on Common Prompt Engineering Failures, Anti-patterns, and Mistakes to Avoid

Research areas:

1. **Common Prompt Failures**
	- Documented failure modes from research
	- What causes prompts to produce poor results
	- Edge cases that break prompts
	- Unexpected behavior patterns
2. **Anti-Patterns to Avoid**
	- Overly vague instructions
	- Contradictory constraints
	- Information overload
	- Poor example selection
	- Assuming model knowledge
	- Format ambiguity
3. **Hallucination and Accuracy Issues**
	- What prompt patterns increase hallucinations
	- How to reduce factual errors
	- Grounding and citation prompting
	- Confidence calibration
4. **Context and Memory Problems**
	- Lost context in long conversations
	- Conflicting instructions
	- State management failures
	- Multi-turn coherence issues
5. **Output Quality Problems**
	- Inconsistent formatting
	- Incomplete responses
	- Off-topic generations
	- Repetitive outputs
6. **Ethical and Safety Concerns**
	- Jailbreaking attempts (for awareness, not instruction)
	- Bias amplification through prompts
	- Harmful output mitigation
	- Privacy and security considerations
7. **Platform-Specific Pitfalls**
	- What works on Claude but fails on ChatGPT
	- Model-specific quirks and workarounds
	- API vs Chat interface differences

For each anti-pattern:

- Why it fails
- How to recognize it
- Corrected version
- Real examples from research/practice

Focus on:

- Academic studies of failure modes
- Security research (red teaming, adversarial prompts)
- Practitioner experiences from forums and blogs
- Comparative analysis across models
- 2024-2025 findings on emerging issues

Include specific examples of bad prompts vs improved versions.

Perfect. Now I'll generate a final summary document.

# Comprehensive Research on Prompt Engineering Failures and Anti-Patterns

## Overview

Based on extensive 2024-2025 research spanning academic studies, security research, and practitioner experiences, this comprehensive guide addresses the critical issue: **95% of generative AI pilots fail** according to MIT estimates. The primary culprit isn't model limitations—it's poorly engineered prompts and inadequate context architecture.

## Key Findings

### The Evolution of Understanding

The field has undergone a paradigm shift from 2022-2025:

![Evolution of Prompt Engineering: From Trial-and-Error to Engineering Discipline (2022-2025+)](https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/b313d7cc0f5796022135a5907b0ee39f/694ad028-6d6b-45d2-9cb0-66e909be4ab5/cbc214c1.png)

Evolution of Prompt Engineering: From Trial-and-Error to Engineering Discipline (2022-2025+)

**2022-2023 (Trial \& Error Era):** 20-30% success rate through random tweaking with no systematic approach.

**2023-2024 (Prompt Techniques Era):** 40-50% success as specific techniques (Chain-of-Thought, few-shot learning) were discovered but applied haphazardly.

**2024-2025 (Context Engineering Era):** 70-80%+ success through recognition that **context determines capability more than model selection**. Winners shifted from "prompt optimization" to "context engineering"—orchestrating system instructions, memory systems, retrieved information, tool availability, and structured output formats as an integrated system.[^1]

**2025+ (Engineering Discipline):** 85-95%+ achievable through systematic approaches: requirements definition → modular design → automated testing → version control → monitoring.

### The Critical Insight

By 2025, top AI engineers stopped writing prompts and started architecting context systems. The breakthrough insight: **agent failures aren't model failures—they're context failures.** A "cheap demo" agent might respond robotically to "quick sync tomorrow," while a "magical" agent that checks calendars, email history, and contact importance responds contextually. Same model, different context engineering.[^1]

***
## 1. Common Documented Failure Modes

provides a taxonomy of 20+ specific failure patterns. Here are the most prevalent:

![Comprehensive Taxonomy of Prompt Engineering Failures: Severity, Frequency, and Fix Difficulty](https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/b313d7cc0f5796022135a5907b0ee39f/baeff5d5-5534-409d-8dbf-9f28dec5e618/c964008d.png)

Comprehensive Taxonomy of Prompt Engineering Failures: Severity, Frequency, and Fix Difficulty

### Vague and Ambiguous Instructions[^2][^3]

**Why It Fails:** The model generates plausible-sounding but irrelevant outputs because it makes assumptions based on training patterns rather than your specific intent.

**Recognition:** If the AI's answer could apply to 10 different interpretations of your question, it was too vague.

**Impact:** Research shows vague prompts confuse AI similarly to how they confuse humans, resulting in "hallucinations" or generic output due to lack of context.

**Example Failure:**[^4]

```
❌ Bad: "Tell me about space"
Result: 10-page response covering everything from cosmology to space exploration to fictional space travel
```

**Example Success:**[^4]

```
✅ Good: "Give me a summary of NASA's recent Mars missions, including factual 
details from their official reports. Include: mission names, launch dates, 
key discoveries. Limit: 300 words."
```

### Overly Complex, Monolithic Prompts[^5][^2]

**Why It Fails:** Long prompts create cognitive overload for the model. It struggles to weight competing instructions equally, often fixating on the first instruction or the most emotionally salient one.

**Symptoms:**

- Model follows some instructions but ignores others
- Random compliance patterns across similar inputs
- Performance degrades as prompt length increases

**Fix:** Break into modular components with clear separation of concerns (system context → task → constraints → examples → quality guidelines).

### Missing or Lost Context[^6][^7]

**Why It Fails:** In multi-turn conversations, models don't retain state. Critical information gets forgotten because each response is generated independently.

**Research Finding:** Analysis of 433 developer-ChatGPT conversations revealed four main knowledge gaps:[^6]

1. Missing Context (37% of ineffective prompts)
2. Missing Specifications (31%)
3. Multiple Conflicting Contexts (18%)
4. Unclear Instructions (14%)

Ineffective conversations contained knowledge gaps in **44.6% of prompts** compared to only **12.6% in effective ones.**

**Recovery Strategy:** Explicitly re-include context in critical turns with state management objects.

***
## 2. Anti-Patterns: What to Avoid

categorizes dangerous patterns. The most impactful:

### Contradictory Constraints

**Why It Fails:** When constraints conflict (e.g., "be detailed AND concise in 50 words on 10 topics"), models either choose arbitrarily or fail.

**Fix:** Explicitly prioritize constraints using instruction hierarchies.

### Format Ambiguity[^8][^9][^10]

**Why It Fails:** Without explicit format constraints, output varies unpredictably:

- Different JSON structures on each call
- Missing fields intermittently
- Inconsistent casing and spacing

**Research Finding:** Across 24 JSON generation experiments, average success rate was only **82.55%, with success rates ranging from 0-100% depending on task complexity.** Tasks with composite objects or lists showed ~40% failure rates.[^9]

**Solutions:**

**1. Constrained Decoding with JSON Schema** (Most Reliable)[^11]

```
✅ Define strict JSON Schema with type validation, required fields, 
additional properties = false. Guarantees valid JSON.
```

**2. Prefilling and Stop Sequences** (90%+ success)[^12]

```
✅ Pre-fill output start: "{"
Define stop sequence: "}"
Model completes JSON between tokens, constrained at inference.
```

**3. Meaning Typed Prompting (MTP)** (Emerging Best Practice)[^8][^13]
Instead of rigid schemas, integrate type meanings into prompt instructions. Outperforms JSON schemas in reliability and token efficiency while maintaining reasoning capability.

### Information Overload ("Lost in the Middle")[^14][^15][^16]

**Why It Fails:** In long contexts, information in the middle sections is systematically ignored—a documented phenomenon called "lost in the middle" (LIM).

**Solutions:**

- Front-load critical information
- Use hierarchical importance marking
- Consider splitting long documents
- Use RAG (Retrieval-Augmented Generation) instead of dumping all context

### Poor Few-Shot Example Selection[^17][^18]

**Why It Fails:** Examples teach patterns. Bad or inconsistent examples teach wrong patterns.

**Research Finding:** Studies with real-world messy data found that **37% error reduction** was achievable through data-centric improvements to few-shot examples.[^18]

**Key Principle:** Example quality matters more than quantity. High-quality, consistently labeled examples outperform large sets of noisy examples.

***
## 3. Hallucination and Accuracy Issues

### Prompt Patterns That Increase Hallucinations[^19][^4][^20]

Research testing 200+ hallucination prevention techniques across ChatGPT, Claude, and Gemini found dramatic effectiveness variations:

**Tier 1: High Impact (40-70% Reduction)**

- **Temporal constraints:** "Only information you're completely certain existed before [date]. For anything after, indicate uncertainty." → 89% reduction in fake recent developments[^19]
- **Source attribution:** "For each claim, specify the source type: research studies, common practice, or theoretical frameworks." → 43% fewer fabricated facts
- **Chain-of-Verification:** "What evidence supports this? What might contradict it? Confidence level 1-10?" → 58% better at identifying false claims
- **Uncertainty indicators:** "Say 'uncertain about' before any claim where confidence < 80%." → Most effective single technique

**Tier 2: Moderate Impact (20-40% Reduction)**

- Confidence scoring after each claim
- Counter-argument requirement
- Scope limitation (avoid cutting-edge topics)

**Tier 3: Lower Impact (10-20% Reduction)**

- Number avoidance (use ranges instead)
- Example quality verification
- Negation checking

**Best Combinations:** Using all Tier 1 techniques together achieves **71% reduction in false claims** for factual research tasks.[^19]

### Critical Implementation Point

Research shows these techniques work but are **not perfectly reliable**—they reduce hallucination but don't eliminate it. High-stakes applications require human verification as a mandatory final step.[^19]

***
## 4. Context and Memory Problems

### Instruction Hierarchy Failures[^21][^22][^23]

**Critical 2025 Finding:** Current LLMs fail to properly respect instruction hierarchies—a fundamental issue for safety and reliability.

**Proposed Instruction Hierarchy (by priority):**

1. System Messages (highest priority, inviolable)
2. User Messages
3. Conversation History
4. Tool Outputs / Retrieved Documents (lowest priority)

**Failure Pattern:** Models often prioritize:

- More recent instructions (violating hierarchy)
- More forcefully stated instructions
- Instructions positioned last
- Instead of honoring the priority order

**Performance Impact:** Testing on IHEval benchmark (3,538 scenarios) revealed:[^21]

- Aligned hierarchies: Models perform near baseline
- Conflicting hierarchies: **Performance drops 30-92%** depending on model
- Models are easily influenced by instruction "strictness" rather than actual priority

**Solution:** Explicit hierarchy training data helps, but current models are **not sufficiently aware of instruction hierarchies.** This remains a unsolved problem requiring either:

1. Model fine-tuning with hierarchy-aware data
2. System-level constraints (e.g., structured queries separating prompts from data)
3. Careful prompt engineering to prevent conflicts

### Lost Context in Multi-Turn Conversations[^7][^24]

**Why It Fails:** Each response is generated independently. Earlier messages get lost, especially in long conversations. Models struggle with:

- Token window limits (64K-200K tokens depending on model)
- Lost-in-the-middle effect
- State management across turns

**Solution Strategies:**

**1. Explicit Summarization**
Every 5-10 turns, create a conversation summary and include it in subsequent turns.

**2. Structured Context Layers**
Implement explicit Priority 1 (System) → Priority 2 (Current) → Priority 3 (Retrieved) → Priority 4 (Tools) organization.

**3. State Management Objects**
Maintain explicit state that tracks decisions, constraints, and context:

```
STATE = {
  "user_role": "marketing manager",
  "decided_features": ["feature_a", "feature_c"],
  "rejected_features": ["feature_b"]
}
Include STATE in every critical prompt.
```

***
## 5. Output Quality and Format Problems

### Format Inconsistency[^25][^10][^26]

**Why It Fails:** Without constrained output, structural elements vary unpredictably.

**Documented Failure Rates:**

- Simple JSON: 82.55% success rate
- Composite objects or lists: 40%+ failure rate
- Depends heavily on prompt specificity and model size

**Solutions Ranked by Reliability:**

1. **Constrained Decoding (Best)**: Model guaranteed to produce valid schema-compliant output
2. **Prefilling with Stop Sequences (Very Good)**: 90%+ success with well-crafted sequences
3. **MTP - Meaning Typed Prompting (Emerging Leader)**: Superior to rigid schemas while maintaining reasoning
4. **Strict Schemas (Moderate)**: May reject valid responses if format doesn't match exactly
5. **Unstructured Output + Post-processing (Worst)**: 50%+ parsing failure rates

***
## 6. Security, Safety, and Ethical Concerns

### Prompt Injection and Jailbreaking[^27][^28][^29][^30]

**Risk Level:** OWASP ranks prompt injection as \#1 LLM vulnerability (May 2023 ranking, still holds 2025). It's devastatingly easy—requires no technical expertise.

**Attack Success Rates:**

- Simple manual jailbreaks: 60% for GPT-3.5, ~30% for GPT-4
- Advanced adversarial prompts (AutoRed framework): 70%+ across models with strong generalization
- RAG poisoning attacks (Pandora): 64.3% success on GPT-3.5, 34.8% on GPT-4

**Root Cause:** LLMs are designed to follow instructions, making them vulnerable to malicious instructions disguised as innocent input. No clear separation exists between system directives and user data within the prompt context.[^31]

**Mitigation Approaches:**

**Tier 1: Prompt Engineering**

- Explicit instruction hierarchies
- System message stating hierarchy clearly
- Refusal training for conflicts

**Tier 2: System Architecture**

- Structured queries (separating prompts from data into distinct channels)
- Position bias elimination
- Implement separation at infrastructure level

**Tier 3: Model-Level**

- Fine-tuning with hierarchy-aware data
- Advanced alignment techniques

### Bias Amplification[^32][^33][^34][^35][^36][^37]

**Finding (2025):** Even "explicitly unbiased" LLMs harbor implicit biases detectable through psychology-inspired prompting methods. Testing with 33,000+ prompts revealed **widespread and consequential implicit biases** across demographic and social dimensions.[^37]

**How Bias Gets Amplified:**

1. Example selection bias (unbalanced samples reinforce stereotypes)
2. Framing effects (question phrasing triggers biased responses)
3. Contextual amplification (social information amplifies bias)
4. Position bias (models favor certain positions)

**Research Findings:**

- Code generation: 13.47-49.10% gender bias prevalence across models
- Recommendations: Biases deeply embedded and "widely pervasive," amplified by socioeconomic factors
- Standard benchmarks underestimate bias risk—biases emerge in realistic out-of-distribution scenarios

**Mitigation (Ranked by Effectiveness):**

**Tier 1: Prompt Engineering**

```
✅ "Provide recommendations that challenge stereotypes. 
Explicitly include non-mainstream options. For each, justify why 
it's appropriate regardless of demographic group."
```

- Limited effectiveness in isolation
- 24-31% bias reduction achievable

**Tier 2: Fairness-Aware Frameworks**

- FACTER framework: Dynamic prompt engineering with violation-triggered mechanisms
- Reduces fairness violations up to 95.5%

**Tier 3: System-Level Intervention**

- Affine concept editing (most robust across realistic scenarios)
- Model fine-tuning with debiasing data
- Architectural changes

**Critical Finding:** Prompt engineering alone is **insufficient for high-stakes applications.** Internal interventions provide more robust fairness across diverse contexts.

### Data Privacy and Leakage[^38][^39][^40]

**Risks:**

- Adversarial prompts extract data verbatim from RAG datastores
- 100% success rate on 25 randomly selected GPTs with ≤2 queries
- Membership inference attacks can determine if data is in retrieval database

**Solution:** Implement defense strategies in RAG templates combined with instruction hierarchies prioritizing privacy.

***
## 7. Platform-Specific Pitfalls

### Claude-Specific Patterns[^41][^24]

**Strengths:**

- Extended thinking with proper token management
- 200K token context windows
- Excellent chain-of-thought reasoning
- Strong analytical depth

**Pitfalls:**

- Thinking blocks auto-stripped in multi-turn (don't re-include them)
- Verbose system messages can confuse hierarchy
- May refuse edge cases more readily

**Best Practices:**

- Use numbered reasoning steps
- Provide rich context upfront
- Break complex tasks into stages

### ChatGPT/GPT-4 Specific[^41][^42]

**Strengths:**

- Excellent structured output handling
- JSON mode for guaranteed valid JSON
- Strong few-shot learning
- Good creative tasks

**Pitfalls:**

- High performance drops on instruction hierarchy conflicts (30-92% drop)
- Vulnerable to prompt injection (30-60% attack success)
- Context windows less generous than Claude

**Best Practices:**

- Use explicit instruction hierarchies (system > user > tools)
- Leverage JSON mode when available
- Provide fewer, higher-quality examples

### Gemini-Specific[^43][^41]

**Strengths:**

- Multimodal (text + image)
- Web research integration
- Good research synthesis

**Pitfalls:**

- Position bias affects option comparisons
- Sensitive to phrasing
- Less documented than other models

**Best Practices:**

- Explicitly specify research parameters
- Request source citations
- Explicit verification requirements

### API Vs Chat Interface Differences

**API:** More control, deterministic possibilities (though temperature=0 not guaranteed), system messages work differently
**Chat:** Less control, context managed by UI, potentially different safety mechanisms

***
## 8. Context Engineering: The 2025 Evolution

**Fundamental Shift:** By late 2025, successful practitioners shifted from "prompt optimization" to "context engineering."

**What Changed:**

- Recognition that **intelligence is synthetic**—shaped by information delivery
- Understanding that context determines capability more than model choice
- Seven distinct information layers beyond simple prompts:

1. System instructions
2. User history/long-term memory
3. Retrieved information (RAG)
4. Tool availability and outputs
5. Structured output formats
6. Dynamic state variables
7. Execution context

**Implication:** Cheap demo vs magical agent isn't about model—it's about **context orchestration.**

***
## 9. Emerging Research (2024-2025)

### Breaking Chain-of-Thought Through Adversarial Prompting[^44]

Research revealed CoT reasoning is surprisingly fragile. Adversarial prompt perturbations (storytelling reframing, irrelevant constraint injection, example reordering) caused:

- Accuracy drops up to 42.1%
- Unpredictable performance (some perturbations improved accuracy by 35.3%)
- Suggests models learn shallow reasoning templates rather than true reasoning

**Implication:** CoT prompting doesn't guarantee robust reasoning—it creates an illusion of transparency while remaining brittle.

### RAG Failure Analysis[^15][^45][^46][^16][^47]

Comprehensive studies identified multiple failure points:

1. **Missing content:** Knowledge base lacks answers
2. **Ranking failures:** Correct documents ranked too low
3. **Lost information:** Key details not in top-K results
4. **Knowledge conflicts:** Retrieved docs contradict model knowledge
5. **Chunking errors:** Poor document segmentation
6. **Generation failures:** Model fails to use retrieved context
7. **Hallucination despite grounding:** Model fabricates despite access to truth

**Solutions:** Hierarchical retrieval, dynamic reranking, explicit conflict detection and resolution.

### Instruction Hierarchy Research[^21][^23]

Largest benchmark study (IHEval, 3,538 scenarios) revealed instruction hierarchies are critical but poorly handled by current models. Proposed standard hierarchy:

1. System Messages (inviolable)
2. User Messages
3. Tool Outputs (lowest priority)

Current models fail this 30-92% of the time under conflicts.

***
## 10. Systematic Framework for Reliable Prompting

### Engineering Approach[^48]

Instead of trial-and-error, treat prompts as engineered systems:

**Phase 1: Requirements Definition**

- Specify exact tasks precisely
- Define measurable success criteria
- List failure modes to avoid
- Document constraints

**Phase 2: Design**

- Modular architecture
- Separate concerns (context/task/constraints/examples/guidelines)
- Enable automated evaluation
- Make everything measurable

**Phase 3: Implementation**

- Clear, specific instructions
- High-quality examples
- Explicit output format
- Quality guidelines

**Phase 4: Evaluation**

- Systematic testing (50-100+ examples)
- Edge case testing
- Format compliance verification
- Hallucination rate measurement

**Phase 5: Deployment**

- Monitoring and logging
- Human-in-loop for high-stakes decisions
- Version control
- Performance metrics tracking

**Phase 6: Maintenance**

- Monitor failure patterns
- Re-test on new data distributions
- Update as performance degrades
- Share lessons learned

***
## 11. Practical Application: Quick Diagnostic Checklist

When a prompt fails:

**Wrong Answer:**

- [ ] Is prompt vague? → Add specificity
- [ ] Conflicting instructions? → Resolve/prioritize
- [ ] Missing context? → Add explicit context
- [ ] Poor examples? → Replace with high-quality ones
- [ ] Ambiguous format? → Define schema
- [ ] Too broad scope? → Narrow or decompose

**Forgotten Context:**

- [ ] Provide summary of previous conversation
- [ ] Explicitly re-include relevant context
- [ ] Use state management object
- [ ] Verify highest-priority instruction is present

**Malformed Output:**

- [ ] Use constrained decoding (JSON Schema)
- [ ] Pre-fill output start
- [ ] Define stop sequences
- [ ] Validate and retry if needed

**Hallucination:**

- [ ] Add temporal constraints
- [ ] Require source attribution
- [ ] Include confidence scoring
- [ ] Reduce scope to established facts
- [ ] Use verification prompting

***
## 12. Key Statistics and Research Findings

**Failure Rates:**

- 95% of generative AI pilots fail (MIT, 2025)
- RAND: 80% failure rate across AI projects
- S\&P Global: 42% of initiatives scrapped (2025, up from 17% in 2024)

**Hallucination Reduction:**

- Best combination: 71% reduction (Tier 1 techniques combined)
- Single techniques: 10-70% reduction depending on type
- Temporal constraints: 89% reduction in fake recent info

**Structured Output:**

- Average success: 82.55% (complex tasks: 40% failure)
- With constrained decoding: ~99%
- With MTP: Better than JSON schemas

**Bias:**

- Gender bias in code generation: 13.47-49.10%
- Test-driven feedback can reduce to 4.79%
- Implicit bias widespread even in "aligned" models

**Security:**

- Prompt injection: \#1 OWASP LLM risk
- Simple jailbreaks: 60% success (GPT-3.5), 30% (GPT-4)
- Advanced attacks: 70%+ success

***
## Conclusion

Prompt engineering isn't failing because LLMs are incapable—it's failing because **organizations treat prompts as creative writing instead of systems engineering.**

The 95% failure rate is addressable through:

1. **Treating prompts as engineered systems** with requirements, design, testing, and monitoring
2. **Shifting from prompt optimization to context engineering**—orchestrating all information layers
3. **Understanding failure modes deeply** and applying targeted fixes
4. **Implementing systematic evaluation** with measurable criteria
5. **Recognizing model limitations** and implementing human oversight for high-stakes decisions

The practitioners and organizations that apply engineering discipline to prompt design and context architecture will deliver reliable, consistent AI systems while competitors remain stuck in trial-and-error cycles.

The future belongs not to the best models, but to the best context engineers.

![Before/After: Prompt Engineering Failures and Their Fixes](https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/b313d7cc0f5796022135a5907b0ee39f/27eee867-0481-486d-8b9d-b8cf23e0f436/dcde6011.png)

Before/After: Prompt Engineering Failures and Their Fixes
<div align="center">⁂</div>

## References

[^1]: <https://www.thedigitalspeaker.com/ais-1b-secret-the-best-engineers-stopped-writing-prompts-in-2025/>
[^2]: <https://www.nucamp.co/blog/ai-essentials-for-work-2025-top-10-prompt-mistakes-to-avoid-in-2025>
[^3]: <https://latitude-blog.ghost.io/blog/common-llm-prompt-engineering-challenges-and-solutions/>
[^4]: <https://documentation.suse.com/suse-ai/1.0/html/AI-preventing-hallucinations/index.html>
[^5]: <https://arxiv.org/pdf/2402.04568.pdf>
[^6]: <https://arxiv.org/html/2501.11709v1>
[^7]: <https://github.com/langflow-ai/langflow/issues/6296>
[^8]: <https://arxiv.org/abs/2410.18146>
[^9]: <https://arxiv.org/abs/2408.11061>
[^10]: <http://arxiv.org/pdf/2408.11061.pdf>
[^11]: <https://docs.dasha.ai/en-us/default/gpt/structured-output>
[^12]: <https://dev.to/rishabdugar/crafting-structured-json-responses-ensuring-consistent-output-from-any-llm-l9h>
[^13]: <https://arxiv.org/pdf/2410.18146.pdf>
[^14]: <https://ieeexplore.ieee.org/document/10771030/>
[^15]: <http://arxiv.org/pdf/2407.12216.pdf>
[^16]: <https://labelstud.io/blog/seven-ways-your-rag-system-could-be-failing-and-how-to-fix-them/>
[^17]: <https://arxiv.org/abs/2303.13217>
[^18]: <https://www.reddit.com/r/PromptEngineering/comments/15rxmhn/ensuring_reliable_fewshot_prompt_selection_for/>
[^19]: <https://www.reddit.com/r/PromptEngineering/comments/1o77fk0/how_to_stop_ai_from_making_up_facts_12_tested/>
[^20]: <https://www.godofprompt.ai/blog/9-prompt-engineering-methods-to-reduce-hallucinations-proven-tips>
[^21]: <https://aclanthology.org/2025.naacl-long.425.pdf>
[^22]: <https://www.metriccoders.com/post/how-to-improve-llm-reasoning-when-your-chain-of-thought-cot-prompt-fails>
[^23]: <https://arxiv.org/html/2404.13208v1>
[^24]: <https://docs.claude.com/en/docs/build-with-claude/context-windows>
[^25]: <https://aclanthology.org/2025.acl-srw.19>
[^26]: <https://pmc.ncbi.nlm.nih.gov/articles/PMC11979239/>
[^27]: <https://genai.owasp.org/llmrisk/llm01-prompt-injection/>
[^28]: <https://hiddenlayer.com/innovation-hub/evaluating-prompt-injection-datasets/>
[^29]: <https://arxiv.org/html/2510.08329v1>
[^30]: <https://www.confident-ai.com/blog/red-teaming-llms-a-step-by-step-guide>
[^31]: <https://www.guidepointsecurity.com/blog/prompt-injection-the-ai-vulnerability-we-still-cant-fix/>
[^32]: <https://ieeexplore.ieee.org/document/10825082/>
[^33]: <https://www.semanticscholar.org/paper/dfbfe75ec8c2143e899897a3c054ee58d99ead43>
[^34]: <https://dl.acm.org/doi/10.1145/3724117>
[^35]: <http://arxiv.org/pdf/2503.19540.pdf>
[^36]: <http://arxiv.org/pdf/2407.08441.pdf>
[^37]: <https://pmc.ncbi.nlm.nih.gov/articles/PMC11874501/>
[^38]: <https://arxiv.org/abs/2402.17840>
[^39]: <https://www.scitepress.org/DigitalLibrary/Link.aspx?doi=10.5220/0013108300003899>
[^40]: <https://arxiv.org/abs/2402.08416>
[^41]: <https://promptbuilder.cc/blog/claude-vs-chatgpt-vs-gemini-best-prompt-engineering-practices-2025>
[^42]: <https://www.datastudios.org/post/deepseek-context-window-token-limits-memory-policy-and-2025-rules>
[^43]: <https://www.dataunboxed.io/blog/prompt-engineering-best-practices-complete-comparison-matrix>
[^44]: <https://arxiv.org/html/2506.06971v1>
[^45]: <https://arxiv.org/pdf/2410.07176.pdf>
[^46]: <https://arxiv.org/pdf/2401.05856.pdf>
[^47]: <https://arxiv.org/html/2510.13975v1>
[^48]: <https://www.braintrust.dev/articles/systematic-prompt-engineering>
[^49]: <http://arxiv.org/pdf/2410.03461.pdf>
[^50]: <https://arxiv.org/html/2401.05787v2>
[^51]: <https://arxiv.org/pdf/2503.01713.pdf>
[^52]: <https://arxiv.org/pdf/2412.12322.pdf>
[^53]: <http://arxiv.org/pdf/2412.12300.pdf>
[^54]: <https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-evaluators/rag-evaluators>
[^55]: <https://www.semanticscholar.org/paper/f18e5a844c37e5342f8f3d409c74c1a9c91d1f8f>
[^56]: <https://galileo.ai/blog/chain-of-thought-prompting-techniques>
[^57]: <https://pmc.ncbi.nlm.nih.gov/articles/PMC11736412/>
[^58]: <https://openaccess.cms-conferences.org/publications/book/978-1-964867-74-8/article/978-1-964867-74-8_31>
[^59]: <https://www.semanticscholar.org/paper/669adfc56514f6e073972b4ae7192502550c4f0d>
[^60]: <https://www.mdpi.com/1999-4893/18/5/296>
[^61]: <https://www.nature.com/articles/s41598-025-14827-x>
[^62]: <https://arxiv.org/abs/2504.13617>
[^63]: <https://arxiv.org/abs/2503.21810>
[^64]: <https://ascopubs.org/doi/10.1200/OP.2025.21.10_suppl.602>
[^65]: <https://arxiv.org/pdf/2402.06363.pdf>
[^66]: <https://arxiv.org/pdf/2502.18878.pdf>
[^67]: <https://arxiv.org/html/2504.06969>
[^68]: <https://arxiv.org/html/2501.10868>
[^69]: <https://arxiv.org/html/2510.12080v1>
[^70]: <https://www.geeksforgeeks.org/artificial-intelligence/role-based-prompting/>
[^71]: <https://arxiv.org/html/2402.05201v3>
[^72]: <https://academic.oup.com/jes/article/doi/10.1210/jendso/bvaf149.2404/8299505>
[^73]: <http://medrxiv.org/lookup/doi/10.1101/2025.08.01.25332567>
[^74]: <https://arxiv.org/abs/2506.18824>
[^75]: <https://arxiv.org/abs/2410.06440>
[^76]: <https://ieeexplore.ieee.org/document/11075653/>
[^77]: <https://academic.oup.com/eurheartjsupp/article/doi/10.1093/eurheartjsupp/suaf076.351/8132736>
[^78]: <https://invergejournals.com/index.php/ijss/article/view/117>
[^79]: <https://www.tandfonline.com/doi/full/10.1080/00325481.2019.1568020>
[^80]: <https://www.semanticscholar.org/paper/7688830ee760630412d2e402b2a60e26d05b5d6a>
[^81]: <http://arxiv.org/pdf/2406.06608.pdf>
[^82]: <https://arxiv.org/html/2411.06099v1>
[^83]: <https://arxiv.org/pdf/2211.14719.pdf>
[^84]: <http://arxiv.org/pdf/2410.12843.pdf>
[^85]: <https://arxiv.org/html/2408.02416v2>
[^86]: <https://aclanthology.org/2023.emnlp-main.494.pdf>
[^87]: <https://arxiv.org/pdf/2409.15199.pdf>
[^88]: <https://arxiv.org/pdf/2306.04528.pdf>
[^89]: <https://www.creativedock.com/blog/dont-fall-for-these-5-anti-patterns-in-genai-project-delivery>
[^90]: <https://timspark.com/blog/why-ai-projects-fail-artificial-intelligence-failures/>
[^91]: <https://coalfire.com/the-coalfire-blog/does-prompt-engineering-still-matter-in-late-2025>
[^92]: <http://peer.asee.org/23127>
[^93]: <https://aclanthology.org/2022.emnlp-main.222.pdf>
[^94]: <https://arxiv.org/pdf/2412.05127.pdf>
[^95]: <https://arxiv.org/pdf/2311.05661.pdf>
[^96]: <http://arxiv.org/pdf/2408.10504.pdf>
[^97]: <https://infomineo.com/artificial-intelligence/prompt-engineering-techniques-examples-best-practices-guide/>
[^98]: <https://www.reddit.com/r/PromptEngineering/comments/1mgqvi9/case_study_3_prompt_optimization_strategies/>
[^99]: <https://www.godofprompt.ai/blog/common-ai-prompt-mistakes-and-how-to-fix-them>
[^100]: <https://arxiv.org/abs/2402.10353>
[^101]: <https://arxiv.org/abs/2403.14896>
[^102]: <https://arxiv.org/abs/2502.02966>
[^103]: <https://ojs.aaai.org/index.php/AAAI-SS/article/view/31771>
[^104]: <https://dl.acm.org/doi/10.1145/3640543.3645200>
[^105]: <https://www.semanticscholar.org/paper/b88503364fff2d68ca867863168f72e74b31e3a3>
[^106]: <https://arxiv.org/abs/2504.07801>
[^107]: <http://arxiv.org/pdf/2403.08743.pdf>
[^108]: <https://arxiv.org/pdf/2502.02966.pdf>
[^109]: <https://arxiv.org/pdf/2407.10853.pdf>
[^110]: <https://arxiv.org/pdf/2405.10431.pdf>
[^111]: <http://arxiv.org/pdf/2504.07787.pdf>
[^112]: <https://aclanthology.org/2025.findings-naacl.199.pdf>
[^113]: <https://arize.com/blog/prompt-optimization-few-shot-prompting/>
[^114]: <https://arxiv.org/html/2506.10922v1>
[^115]: <https://www.paloaltonetworks.com/cyberpedia/what-is-ai-red-teaming>
[^116]: <https://arxiv.org/html/2502.04419v2>
[^117]: <https://arxiv.org/abs/2402.13178>
[^118]: <https://www.nature.com/articles/s41746-024-01091-y>
[^119]: <https://arxiv.org/abs/2401.11246>
[^120]: <http://pubs.rsna.org/doi/10.1148/ryai.240476>
[^121]: <https://arxiv.org/abs/2404.04302>
[^122]: <https://arxiv.org/abs/2410.20724>
