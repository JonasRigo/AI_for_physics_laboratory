---
name: paper-review-from-graph
description: Write short, evidence-calibrated reviews of papers from graph-extraction output.
---

# Paper Review from Graph

Use this skill when a paper has been passed through the graph-extraction MCP tool and the user wants a concise review of its content, evidential support, and unresolved claims.

## Inputs

The primary input is the graph-extraction result. It may contain components, obligations, relations, source chunks, equation tags, imported documents, and confidence labels such as `asserted`, `cited`, `conditional`, `conjectural`, `partially_discharged`, or `open`. If only a paper URL is supplied, obtain graph extraction first with the graph-extraction MCP tool.

Do not treat the graph as an independent peer review. It is an organized representation of the paper's claims and proof status. Distinguish what the paper states from what has been independently verified.

## Review workflow

1. Identify the paper's question, method, and main claimed results from the headline and necessary components.
2. Reconstruct the dependency chain from the relations. Explain the chain in plain language, preserving important mathematical notation.
3. Classify claims using the graph's evidence, not rhetorical strength:
   - **Supported in the paper:** definitions, derivations, estimates, or arguments that are actually developed and internally connected.
   - **Credible/plausible:** claims consistent with the framework or cited results, but dependent on assumptions, unverified applicability, heuristic asymptotics, or incomplete steps.
   - **Not established:** claims marked open, conjectural, conditional without discharged premises, or conclusions whose key bridge is missing.
4. Pay special attention to open obligations that sit upstream of headline conclusions. An unresolved foundational or convergence obligation weakens every downstream claim that depends on it.
5. Separate “the paper gives an argument for X” from “X is proved.” Do not upgrade cited results, numerical evidence, examples, or formal asymptotics into proofs.
6. Mention notable limitations without attempting to correct the paper unless the user asks for a technical audit.

## Output

Write a short review, normally 400–800 words unless the user requests another length. Use this structure:

### What the paper does

State the problem, framework, and principal results. Explain the central dependency chain in one coherent paragraph.

### What is supported

Discuss the parts that are definitions, internally derived identities, combinatorial setups, explicit model calculations, or validly stated consequences of clearly identified assumptions. Use cautious wording when support is only internal to the manuscript.

### What is plausible but conditional

Discuss claims that are mathematically motivated and compatible with known results but rely on cited theorems, applicability conditions, asymptotic replacements, universality hypotheses, or extrapolation from examples.

### What is not established

Identify the most consequential open obligations and explain which headline claims they affect. Include conjectural results and unsupported maximality or universality claims here.

### Overall assessment

Give a balanced verdict: distinguish a valuable framework or strong heuristic program from a complete proof. State whether the paper's main contribution is theorem-level, conditional, conjectural, or a mixture.

Use inline equation tags or section names from the graph when they help the reader locate a claim. Do not reproduce the entire graph, list every obligation, or invent missing proofs.

## Calibration rules

- “Supported” means supported by the supplied paper/extraction, not externally validated.
- “Cited” means the paper relies on another result; summarize the dependency and flag that applicability may remain unchecked.
- “Conditional” and “conjectural” must remain visibly conditional in the prose.
- If the extraction says an obligation is `open`, call it unresolved even if the resulting formula is plausible.
- Avoid accusing the authors of error when the evidence only shows incompleteness.
- If the graph output is truncated or ambiguous, say so and limit the review to claims with clear support.
