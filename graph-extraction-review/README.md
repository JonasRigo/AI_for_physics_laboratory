# Paper explanation with a reviewed concept graph

This project implements the course assignment **“Ontolgoy based graph enabled paper review.”** It extracts a structured argument graph from a physics paper, then uses that graph to write a short review that separates:

- claims supported by the paper's definitions and derivations
- plausible claims that depend on assumptions, cited results, or heuristic steps
- claims that remain conjectural or unsupported

The workflow is intended for a researcher who want a fast map of a paper's logic before reading the technical details.

## Inputs and outputs

Accepted inputs are:

1. an accessible paper URL, such as `https://arxiv.org/html/1812.08657v5`; or
2. a short Markdown document containing a self-contained derivation or argument, such as [`example/short_note.md`](example/short_note.md).

The graph-extraction stage produces structured JSON containing components, proof obligations, relations, source chunks, equation tags, and imported references. The review stage produces a short Markdown review with these sections:

1. What the paper does
2. What is supported
3. What is plausible but conditional
4. What is not established
5. Overall assessment

## Workflow files

- [`workflow/Graph Extraction Tool.json`](workflow/Graph%20Extraction%20Tool.json): extracts a reviewed argument graph.
- [`workflow/Graph Extraction and Review.json`](workflow/Graph%20Extraction.json): extracts the graph and adds a paper-review stage.
- [`workflow/Table Extraction.json`](workflow/Table%20Extraction.json): related table-oriented extraction flow.
- [`SKILL.md`](SKILL.md): instructions for producing an evidence-calibrated review from graph output.

The flow uses custom `graph_extraction` components and a language model for structured extraction and review. Do not commit API keys or other credentials.

## Running an example

Open the flow in Langflow, configure the language-model provider, and provide either the arXiv URL or the contents of `example/short_note.md`. The supplied paper example is:

```text
https://arxiv.org/html/1812.08657v5
```

For a local Markdown test, paste the contents of `example/short_note.md` into the text input used by the extraction flow. Save the graph JSON and final review in `evidence/`.

If using the command-line runner, the exact command depends on the installed `lfx` and extension versions. A typical invocation is:

```bash
lfx run "workflow/Graph Extraction.json" \
  "Review the paper based on the extracted graph."
```

Configure the provider before running. The exported example currently records an OpenRouter model selection, so an `OPENROUTER_API_KEY` or an equivalent provider configuration is required. If the runner reports a missing `trustcall` dependency, install the version required by the selected `lfx` environment and rerun; do not bypass the dependency by adding credentials to the flow file.

## Task specification

**Scientific question.** What does the selected paper claim, how do its main claims depend on one another, and which claims are actually supported by the supplied argument?

**Intended user.** A researcher screening a paper before a detailed reading.

**Assumptions.** The source is accessible and sufficiently self-contained; source chunks and equation labels are retained, the model output is inspected against the original paper.

**Excluded tasks.** The workflow does not independently prove the paper, verify every citation, reproduce numerical results, or perform exhaustive literature review.

**Stopping conditions.** Run one extraction and one review for an input. Stop if the source cannot be parsed, the graph is incomplete, or required model access is unavailable. Report the failure rather than inventing missing evidence.

**Success criteria.** The output identifies the paper's central argument, preserves important dependencies, cites source locations where available, and explicitly labels conditional, conjectural, and unresolved claims.

## Human judgment

The researcher must inspect the source locations attached to headline claims, check whether cited theorems actually apply, and decide whether a plausible argument is adequate for the intended use. Intervention is required when the graph reports open obligations, missing source chunks, ambiguous terminology, or a conclusion that depends on an assumption not accepted by the reader. A researcher may accept the review as a reading aid, but should reject it as a proof certificate.

## Submission contents

- `workflow/`: executable flow exports;
- `example/`: URL choice and short Markdown test input;
- `evaluation.md`: brief evaluation plan and observations;
- `SKILL.md`: reusable review instructions.

