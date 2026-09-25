# Evaluation

**This is a short example what your evaluation can look like.**

Try the workflow on both:

1. the accessible paper URL `https://arxiv.org/html/1812.08657v5`; and
2. [`example/short_note.md`](example/short_note.md), a deliberately small and self-contained derivation.

For each input, check whether the output:

- identifies the main argument and its dependencies;
- distinguishes explicit derivations from assumptions and conjectures;
- preserves useful source locations or equation labels; and
- reports missing evidence instead of filling gaps confidently.

The short note is the easy baseline case. The paper is the difficult case because it contains multiple branches, cited asymptotic results, conditional conclusions, and unresolved proof obligations. 

Record one graph JSON and one review for each input in `evidence/`. Report approximate runtime and model/provider used if the flow is executed.
