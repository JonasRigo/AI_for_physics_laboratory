# AI for physics laboratory

## Workflow automation projects

**Work in pairs. Projects are assigned to you.** Develop a small, useful automation for a clearly defined physics task. Your project has two main outcomes:

1. **Upload a reusable workflow to the shared course GitHub repository**, with the instructions and examples your fellow colleagues need to run and adapt it.
2. **Give a 45-minute presentation** explaining what the workflow does, how it works, why you designed it that way, and what its results and examples demonstrate.

The aim is to contribute a useful tool to the class and teach your fellow colleagues how to understand and use it.

Projects 1–10 form the main catalogue. Projects 11–15 are optional alternatives. You are free to propose your own project, after deliberation with the instructor.

### Freedom of implementation

You may use Langflow, another visual workflow editor, Python, notebooks, or your preferred software. **All software suggestions in this document are optional.** You may replace a suggested tool without changing the scientific objective or the required deliverables. A command-line workflow or notebook can be a complete submission; a polished application interface is unnecessary.

Your workflow should contain at least one clearly motivated use of a **generative model** or **embedding model**. Explain its role and why its task cannot be performed by any other traditional algorithm.

## The five deliverables required for every project

The numbered deliverables in each project below specify what these common requirements mean for that particular task. They form the contents of your GitHub contribution and the evidence for your presentation. You may combine the written material in a README or short report (four separate reports are not required).

| DELIVERABLE                       | REQUIRED CONTENTS                                            |
| --------------------------------- | ------------------------------------------------------------ |
| **1. Precise task specification** | State the scientific question, intended user, accepted inputs, expected outputs, assumptions, excluded tasks, and success criteria. Define stopping conditions, including a maximum number of iterations or tool calls where applicable and a time or compute budget. Specify what happens when evidence is missing or a check fails. A short specification of roughly one page is sufficient. |
| **2. Executable workflow**        | Submit the flow export, scripts, or notebook, plus one complete example with its input files and expected output structure. Include installation instructions, dependency versions, model requirements, and exact instructions to run it. Another group should be able to follow the instructions without reconstructing undocumented steps. Include any custom components. |
| **3. Evaluation**                 | Compare the workflow with a simple baseline on the same inputs. Use test cases, including at least one deliberately difficult or failing case and one easy baseline case. Define the expected result or checking procedure before the final evaluation. Report outcomes for all cases, scientific errors, limitations, and approximate runtime and model usage or cost. A test case can be a question, parameter setting, data variant, or input defect. |
| **4. Be the judge**               | Explain where a physicist must supply assumptions, inspect evidence, resolve ambiguity, or approve a result. Identify what triggers that intervention and what the person sees. Include one concrete example of a correction, rejection, or justified acceptance. |

### Evaluation principles

- A useful baseline can be a direct LLM prompt, keyword retrieval, a fixed script, or a conventional numerical method. Choose one that tests the value of your added workflow.
- Use the same evaluation inputs and reference criteria for both systems. Keep model and tool access comparable where possible, and report differences in compute or available information.
- Keep at least two test cases aside while developing the workflow. Do not repeatedly tune prompts on every final test.
- For your workflow, repeat at least one representative case three times and describe any variation.
- Try to compare the behaviours of different LLMs, different context lengths, and different reasoning settings.
- A workflow that identifies insufficient information or fails with interpretable traces can be more useful than one that always produces a confident answer. An honest negative result is a valid project outcome.

### Outcome 1: a reusable contribution to the shared GitHub repository

Upload your completed workflow to the shared course GitHub repository using the submission procedure provided by the instructor. The workflow must be available for your fellow colleagues to download, run, inspect, and adapt. An exported flow alone is sufficient only if it includes everything needed to execute the documented example.

Your contribution must contain:

- **The executable workflow:** flow exports, scripts, or notebooks, including custom components and required prompts or configuration files.
- **A README:** the purpose or function, supported inputs and outputs, prerequisites, software versions, model/service requirements, installation or import steps, and exact instructions for running an example.
- **An example input and its recorded output:** explain what another colleague should expect to see, including acceptable variation in stochastic outputs.
- **The four deliverables:** task specification, evaluation results and procedures, and account of human judgment alongside the executable workflow. Explain the key design choices and how to adapt the example to a new question or dataset.

For your submission, create a folder `workflow_name/` and structure it like this: `README.md`, `workflow/`, `example/`, `evidence/` and `evaluation.md` (equivalent organization is welcome). Written explanations may share a file.

**WARNING: Remember to never share or upload credentials or API keys!**

**Before submitting, ask another pair to run the example using your instructions.** Record any missing steps they identify and correct the instructions. If an external service or account requirement prevents execution, document that limitation and make the recorded example available for inspection.

### Outcome 2: a 35-minute presentation

Your presentation should teach your colleagues how the workflow works and why its design is appropriate for the task. Show the scientific problem, the mechanism of the automation, the decisions you made, and actual examples of its behavior.

The following timing is a suggestion. You may distribute the 35 minutes differently, provided you cover the required content.

| TIME      | TOPIC                        | WHAT TO EXPLAIN OR DEMONSTRATE                               |
| --------- | ---------------------------- | ------------------------------------------------------------ |
| 0–5 min   | Scientific task              | Who would use this workflow? What problem does it solve? What are its inputs, outputs, assumptions, and limits? |
| 5–15 min  | How the workflow works       | Show an overview, then trace one input through the stages. Explain the model calls, tools, data passed between stages, any retained state, and the conditions for repeating or stopping. |
| 15–20 min | Why you designed it this way | Justify the main decisions: where you use an LLM, where you use ordinary code, how you retrieve or check information, and where a human intervenes. Explain at least one alternative you considered and the tradeoff that informed your choice. |
| 20–30 min | Examples and results         | Demonstrate one complete run and inspect important intermediate artifacts. Show the evaluation against your baseline and at least one failure or difficult case. Explain what the results establish and what remains uncertain. |
| 30–35 min | Reuse and limitations        | Show where the workflow lives in the shared repository, how to run the supplied example, and how to adapt it. Summarize the situations in which a physicist should inspect, modify, or decline to use its output. |

**Note**: Prepare a saved example if a live run depends on network access or takes too long.

The presentation should answer four questions clearly: **What does it do? How does it work? Why is it designed this way? What do its results show?**

**A completed project consists of the uploaded, documented workflow with all five deliverables and a 35-minute presentation. More components or more agents do not by themselves make a better project.**

------

## 1. Evidence-grounded question answering: RAG, GraphRAG, or OAG

**Short explanation.** RAG stands for *retrieval-augmented generation*. It allows agentic AI to retrieve information from a knowledge base to answer questions from facts and not memory. Facts can also be *ingested* to dynamically update the agent's knowledge.

**Assignment.** Build an assistant that answers physics questions using a collection of papers or lecture notes. Answers must identify the passages that support their important statements.

Choose one main approach: retrieval-augmented generation (RAG), graph-assisted retrieval (GraphRAG), or ontology-augmented generation (OAG). In this assignment, OAG means using an explicit description of concepts, relations, or constraints to guide retrieval or interpretation. You do not need to implement all three approaches, but ideally you will build an OAG.

If you go the graph extraction route, use the reference graph extraction workflow.

**Suggested route.** Load documents, preserve source locations, divide text into useful passages (ingestion), retrieve evidence for a question, generate an answer, and check its support. A graph variant might extract a graph (see reference workflow) and follow links between a model, its assumptions, and its predictions. For maximum flexibility: make the ingestion of documents and retrieval of passages separate *tools* that you can expose to an agent.

**Application proposal.** You can see what the process of writing your bachelor's thesis would have been like by adding the literature that you used to the RAG. Would it have been easier to find the information you need? After going back to your bachelor's thesis, does the RAG help you find new information or information you forgot?

**Optional software hints.** Langflow can connect retrieval and generation and turn HTML into structured text. ChromaDB can store and search embeddings. NetworkX can hold a small graph without a separate database; RDFLib is an option for RDF-based relations. See the tool guide below.

**Comments.** Use a user-provided collection. New literature discovery belongs to Project 2. Building a general physics ontology is unnecessary.

------

## 2. Literature research with review

**Assignment.** Build a workflow that investigates a question or a topic, searches and screens for candidate papers, and produces a literature synthesis. To improve the synthesis results, use the reference graph extraction workflow.

**Suggested route.** Specify criteria, discover candidates, remove duplicates, review one paper at a time, record inclusion decisions, extract claims and relations, then synthesize the accepted evidence.

**Application proposal.** As a benchmark of your code, you can perform the literature search you conducted for your bachelor's thesis using the AI workflow. Does it find the same literature? Does it find different literature? If so, are the results interesting?

**Optional software hints.** OpenAlex can supply scholarly metadata and search results. Beautiful Soup or Langflow's URL/Parse components can parse accessible article HTML, but it does not retrieve paywalled full texts or understand equations automatically. Langflow can organize discovery and one-paper review stages. NetworkX and a CSV file are sufficient for a first graph and screening record.

**Comments.**

* A general-purpose research agent and exhaustive coverage are outside the minimum task. Define a policy for missing full text rather than treating an abstract as a full-paper review.

* Let reviewer feedback propose a revised search query within the fixed search budget. Use arXiv papers for easily accessible search and note: arXiv HTML papers are easier to read for a machine than PDFs (PDFs are hard to read!), and arXiv search does not rank results like OpenAlex.

------

## 3. A physics tutor and examiner

**Assignment.** Build a tutor for a user-supplied topic, such as the harmonic oscillator, dimensional analysis, or numerical integration. The tutor should explain ideas, respond to answers, and choose a suitable follow-up exercise. An examiner stage should assess answers using explicit criteria.

Prepare **three learning objectives and six reference-checked questions**, including a conceptual question and a calculation. Use simulated colleague responses for evaluation; a study with real learners is not required.

**Suggested route.** Establish the learner's starting point, offer an explanation or hint, collect an answer, assess it, and choose a next step. Bound the number of exchanges and finish with a summary of demonstrated understanding and unresolved difficulties.

**Optional software hints.** Langflow can separate tutor and examiner stages. Plain JSON can hold questions and marking criteria. SymPy can check suitable algebraic expressions under stated assumptions. A notebook or a simple chat interface is enough.

**Comments:** Teach one topic. The tutor and examiner may use the same model, but their agreement cannot replace reference checking.

**Optional extension.** Adapt question difficulty using a transparent record of demonstrated skills.

------

## 4. Reproduce one result from a paper

**Assignment.** Use a generative model within a workflow that translates a paper's method into executable code and attempts to reproduce **one figure, table entry, or quantitative result**. Choose a result that fits your available hardware and time budget.

**Suggested route.** Identify the target, extract equations and numerical choices, inspect omissions, generate or adapt code, run it, compare against the target, and perform at most three documented correction rounds.

**Application proposal.** You can reproduce the algorithm presented in this paper: https://arxiv.org/abs/2108.09575. As a check, you can compare the code produced by the workflow with the official code provided publicly https://github.com/JonasRigo/AD-Numerical-Renormalization-Group.

**Optional software hints.** Langflow can organize extraction, generation, execution, and review. SciPy can supply numerical solvers, Matplotlib can produce plots, and pytest can run scientific checks. nbclient can execute notebooks automatically.

**Comments.** Reproducing the whole paper or matching a figure visually without a quantitative comparison is outside the assignment. A documented unsuccessful reproduction is valid if it identifies the remaining obstacle. Choose a paper from arXiv, since it gives you access to HTML versions (easier to read for LLMs than PDFs) and access to the LaTeX source code.

**Optional extension.** Test a prediction at a parameter value not shown in the paper.

------

## 5. Experimental data analysis and presentation

**Assignment.** Build a workflow that turns a dataset and a description of the experiment into a justified analysis, uncertainty estimates, and clear plots. Focus on one scientific question, such as estimating a physical parameter or assessing whether a model describes the observations. The workflow should use the supplied context to propose and justify an analysis, carry it out with inspectable numerical code, and assess the result.

Use one of the datasets below, another openly available dataset, or realistic synthetic measurements with known parameters. Supply a short experimental description, column definitions, units, known preprocessing or selection steps, and available uncertainty information. The generative model may interpret this context, propose a physical model and analysis plan, or explain results. Record the chosen model, assumptions, and analysis settings. If essential information is missing, the workflow should request it or state how this limits the analysis.

The output should include the inferred quantities and their uncertainties, plots of the data and analysis results, and a short interpretation of agreement, discrepancies, and limitations. Use error bars or uncertainty bands where meaningful, with clearly stated uncertainty definitions. The workflow should justify axis scales and choose coordinates appropriate to the measured quantities: polar plots are useful only when the physical variables warrant them. For a fit, include residuals or another suitable diagnostic rather than judging agreement from the main plot alone.

**Application proposals.** The following are two suitable examples. Give the workflow a scientific question and enough experimental context to make informed choices. Keep your expected analysis and checking criteria in the evaluation record so you can assess whether the workflow identifies the relevant steps without a step-by-step recipe.

**a) Cosmic microwave background spectrum.** Use the [COBE/FIRAS CMB monopole spectrum from NASA LAMBDA](https://lambda.gsfc.nasa.gov/product/cobe/firas_monopole_get.html), a small plain-text dataset containing spectral intensities, residuals, and quoted uncertainties. The [dataset description](https://lambda.gsfc.nasa.gov/product/cobe/firas_monopole_spect.html) explains the experiment and how the published spectrum was constructed. Ask whether the measured spectrum is consistent with thermal radiation and what temperature it implies. Build a workflow that uses the experimental context to select and justify an appropriate physical model, estimate its parameters and uncertainties, and assess agreement through residuals. Check that it interprets the spectral coordinate and intensity units correctly and distinguishes measured intensity from the published residual column. This is a reanalysis of a processed scientific data product; any assumption of independent spectral errors must be stated, and fit uncertainties must be distinguished from the experiment's full calibration uncertainty.

**b) Z-boson resonance from muon pairs.** Use the `Zmumu` CSV sample in the [CMS educational datasets derived from Run2011A](https://opendata.cern.ch/record/cms-545). It contains muon energies and momenta for events selected around the Z-boson resonance; the linked page describes the columns and event selection. The [CMS education guide](https://opendata.cern.ch/docs/cms-guide-for-education) provides additional context. Ask what resonance mass the muon-pair events support and how robust that estimate is to analysis choices. Build a workflow that identifies and reconstructs the relevant observable, selects a justified peak model, and reports the inferred position with statistical uncertainty. Evaluate whether it recognises binning and fit interval as choices worth investigating and tests their effect on the result. Check the reconstructed invariant masses against the supplied mass column, and assess the fit with appropriate counting statistics and diagnostics. Keep the resonance position as the main target. Note that interpreting its width requires accounting for detector effects. State the limitations of the educational event selection.

**Optional software hints.** Langflow can organize the analysis stages and call a Python component. pandas (a Python library that works natively with Langflow) can read and organize tables, SciPy can fit models, Matplotlib can plot results, and Pint can help handle units. See the tool guide for documentation.

**Comments.** One dataset and one principal analysis are sufficient. Model choice should follow the physics and measurement context; an unrestricted search over fitting functions is unnecessary. Compare with a simple reference analysis on the same data and check at least one controlled input defect, such as a missing unit or uncertainty column. A physicist must review the proposed assumptions and whether the reported uncertainties support the interpretation.

**Optional extension.** Compare two physically motivated models or add a documented treatment of correlated calibration uncertainty.

------

## 6. Analytic derivation with verification

**Assignment.** Build a workflow that develops a derivation, checks it, and returns either a supported result or an explicit unresolved step. Select one family of problems, such as linear differential equations, commutator identities, or a thermodynamic relation under specified assumptions.

**Suggested route.** Record the claim and assumptions, propose a method, attempt the derivation, check individual steps or seek a counterexample, then revise the argument. Use at most three revision rounds. If the claim changes, preserve the original and explain the change.

**Application examples.** Choose one of the following calculations. Start from a supplied Hamiltonian and a clearly specified approximation; reconstruct and verify the steps leading to a closed-form result.

**a) Anderson's poor man's scaling for the Kondo model.** Use Sections 2–3 of Andriy Nevidomskyy's [The Kondo Model and Poor Man's Scaling (2015)](https://www.cond-mat.de/events/correl15/manuscripts/nevidomskyy.pdf). Restrict the calculation to a spin-$\tfrac{1}{2}$ impurity, a single conduction channel, isotropic exchange, and a constant density of states. Derive the second-order correction from eliminating a narrow energy shell, obtain the one-loop scaling equation, and solve it for the running coupling. With the reference's convention $H_{\mathrm{int}} = 2J\,\mathbf{s}\cdot\mathbf{S}$, $g = \rho J$, and $\ell = \ln(D_0/D)$, the targets are

$$
\frac{dg}{d\ell} = 2g^2
$$

and the leading exponential Kondo scale

$$
k_B T_K \sim D_0 \exp\left(-\frac{1}{2g_0}\right).
$$

State the density-of-states and coupling conventions explicitly. Verify the spin algebra with Pauli matrices and compare the analytical flow with numerical integration for antiferromagnetic and ferromagnetic initial couplings. Explain where weak-coupling perturbation theory breaks down; the pole of the approximate flow does not establish a physical divergence.

**b) The weak-coupling s-wave BCS superconducting gap.** Use Bardeen, Cooper, and Schrieffer's [Theory of Superconductivity, Physical Review 108, 1175 (1957)](https://doi.org/10.1103/PhysRev.108.1175) as the reference. Starting from the reduced pairing Hamiltonian, use the BCS mean-field approximation to derive the self-consistency equation for an isotropic s-wave gap. Assume a constant density of states $N(0)$ per spin and an attractive interaction of magnitude $V$ within the energy window $|\xi| < \hbar\omega_D$. With $\lambda = N(0)V \ll 1$, derive the zero-temperature result

$$
\Delta(0) \simeq 2\hbar\omega_D \exp\left(-\frac{1}{\lambda}\right).
$$

Verify the integral and weak-coupling limit symbolically and compare the approximation with a numerical solution of the gap equation. As an optional extension, derive

$$
k_B T_c \simeq \frac{2e^\gamma}{\pi}\hbar\omega_D \exp\left(-\frac{1}{\lambda}\right)
$$

and the ratio

$$
\frac{2\Delta(0)}{k_B T_c} \simeq 3.53,
$$

where $\gamma$ is the Euler–Mascheroni constant. This example uses a self-consistent mean-field approximation; explain why the exponentially small gap is not captured by a finite power series in $\lambda$.

**Optional software hints.** Langflow can implement the bounded proposal/check/revision cycle. SymPy can perform symbolic manipulation. Numerical substitution can help find counterexamples. For a familiar finite-dimensional operator problem, explicit matrix calculations may provide useful checks.

**Comments.** An arbitrary theorem prover is unnecessary. Use a problem family you can independently assess.

**Optional extension.** Add a dedicated counterexample search before accepting a claim.

------

## 7. Orchestration of a numerical experiment

**Assignment.** Build a workflow that runs a small numerical study, evaluates its results, and allocates a limited budget of further runs. Choose either convergence control or hyperparameter (e.g. integration step size, time step size, etc.) optimization as the main objective. Finish with a separate production run or data accumulation stage using the selected settings.

Example tasks include selecting an integration step for a target accuracy, choosing sampling settings for an observable, or tuning a small numerical approximation.

**Optional software hints.** Langflow can organize run selection and review. SciPy can provide a small simulator. Optuna is an option for optimization, while a fixed grid may be a better starting point. Store configurations and results in JSON or CSV.

**Comments.** Start on a laptop with a proposed budget of no more than 20 short tuning runs per study, adjusted with the instructor. Cluster integration is optional. Do not stop solely because an LLM says the result looks converged.

**Application proposal.** Use [qs-snapshots-fidelity](https://github.com/JonasRigo/qs-snapshots-fidelity) to locate a phase transition in the one-dimensional spinless t–V model from occupation snapshots. Keep the supplied conditional autoregressive RNN architecture fixed; students implement and orchestrate training, validation, and analysis rather than design a network. Investigate how the inferred transition depends on the training-data budget, training settings, and random seed. Start with small systems suitable for a laptop.

| STAGE                    | WHAT THE WORKFLOW DOES                                       |
| ------------------------ | ------------------------------------------------------------ |
| Prepare snapshots       | Use supplied datasets or `generate_samples.py` for exact-diagonalization snapshots at a small system size, initially L = 10, across interaction strengths. Record seeds and split data into training, validation, and held-out test sets at each interaction strength. |
| Establish a baseline    | Compute the charge-density-wave structure factor S(π) directly from held-out snapshots and inspect its variation with interaction strength. |
| Train the supplied RNN  | Implement teacher-forced negative-log-likelihood (NLL) training using `rnn_model.py`; keep the architecture fixed. |
| Allocate tuning runs    | Within the 20-run budget, vary learning rate, training duration, or snapshot count. Use validation NLL and runtime to choose further runs and apply a predefined stopping rule. |
| Select training settings | Select settings using validation results; reserve the test set for final evaluation. |
| Accumulate final results | With settings fixed, train independent seeds and sample the RNN. Estimate the Bhattacharyya-based fidelity susceptibility and S(π), increasing sample counts until the chosen precision or compute limit is reached. |
| Report                  | Plot susceptibility peaks, S(π), its derivative, and NLL curves. Compare with the snapshot baseline and report variation across seeds, finite-size limitations, runtime, and computational cost. |

------

## 8. Explanation of code or a paper with a reviewed concept graph

**Assignment.** Build a workflow that explains either one small code module or one self-contained section of a physics paper. The explanation should connect statements to source locations and show how the important concepts depend on each other.

Choose a target such as approximately 100–300 lines of scientific code or 2–4 pages containing a derivation. Adjust these bounds for mathematical density.

**Application proposal.** Think back to the literature research you conducted for your bachelor's thesis. Choose a paper that you came across and had trouble understanding at the time. Use your workflow to get it explained to you. Does your understanding improve? What has helped you get a better understanding of the concepts?

**Optional software hints.** Langflow can separate extraction, explanation, and review. NetworkX can store the concept graph. Beautiful Soup can extract article HTML. For code, language parsers or Python's standard `ast` module can help identify structure.

**Comments.** Explain one selected target. A graph, as produced by the reference graph extraction workflow, or the [graphify](https://github.com/Graphify-Labs/graphify) tool is very helpful for the user and the agent to better understand code or math.

**Optional extension.** Offer a second explanation for a reader with different prior knowledge while retaining the same source links.

------

## 9. A scientific text editor with terminology and consistency checks

**Assignment.** Build an editor for short physics texts that proposes traceable changes to language and identifies possible scientific inconsistencies. Use a 500–1,000-word example and a small supplied collection of definitions or reference passages.

Create a compact terminology specification: approximately ten terms or symbols, their definitions, relevant relations, and at least five explicit consistency rules. This can be a structured table or a small ontology. A glossary alone should not be described as a complete ontology.

**Application proposal.** Test your editor by opening your bachelor's thesis and deleting a paragraph or even a whole page from the text. Try to write the missing text again using the features of your editor. Does this make your work faster? How accurate is the produced text? Does the text differ from your original text?

**Optional software hints.** Langflow can separate language review from evidence and terminology checks. LanguageTool is an optional grammar-checking component. RDFLib can represent an ontology if useful; a reviewed JSON table is sufficient for the minimum scope. Standard text-difference tools can produce the change list.

**Comments.** The editor checks against supplied evidence and explicit rules. It does not certify the truth of an arbitrary manuscript. A suggestion list is sufficient; no word-processor plugin is required.

**Optional extension.** Add a check that accepted edits preserve equations, numerical values, and defined notation.

------

# Optional software guide

The following are suggestions for particular operations, not a required software stack. Start with the fewest tools that make your example work. Check access requirements and the documentation for your installed version. A native Langflow component may not exist for every library; ordinary Python code, a custom component, or an external tool interface are all possible choices.

| OPERATION                       | POSSIBLE TOOL                                                | ROLE IN A PROJECT                                            |
| ------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Run LLMs locally                | [Ollama](https://ollama.com/)                                | Run open models on your own computer. Model size and performance depend on the available memory and hardware. ([ollama.com](https://www.ollama.com/?utm_source=chatgpt.com)) |
| Coding harness                  | [OpenCode](https://opencode.ai/)                             | Use a coding agent to help implement, explain, and debug your workflow. Supports different model providers. Review generated code and verify its results. ([The open source AI coding agent](https://opencode.ai/en/?utm_source=chatgpt.com)) |
| Access hosted LLMs              | [OpenRouter](https://openrouter.ai/)                         | Access models from multiple providers through a common API. Useful for comparing models without changing your workflow substantially. Paid and some free models are available. ([Documentation](https://openrouter.ai/docs/cookbook/coding-agents/opencode-integration?utm_source=chatgpt.com)) |
| Build agentic workflows in code | [LangGraph](https://docs.langchain.com/oss/python/langgraph/overview) | Define workflows with shared state, conditional branches, tool calls, and review loops. Suitable when you want detailed control over execution in code. ([LangChain Reference](https://reference.langchain.com/python/langgraph/overview?utm_source=chatgpt.com)) |
| Graph-based bookkeeping         | [Semantica-Agi](https://docs.getsemantica.ai/)               | Semantica-Agi is a powerful tool to manage and create graphs, manage big knowledge bases and extract useful connections and insights from the provided data using graph reasoning. |
| General-purpose AI assistant    | [Hermes Agent](https://hermes-agent.nousresearch.com/)       | Use an open-source assistant with tools, persistent memory, and reusable skills. A possible starting point for a research assistant or daily-briefing project. ([Hermes Agent](https://hermes-agent.nousresearch.com/docs/?utm_source=chatgpt.com)) |
| Visual workflow construction    | [Langflow](https://docs.langflow.org/)                       | Connect model calls and processing stages. [Custom Python components](https://docs.langflow.org/components-custom-components) can implement calculations or integrations. |
| Reusable tool interface         | [Langflow MCP server](https://docs.langflow.org/mcp-server)  | Optionally expose a completed bounded workflow as a tool for another application. This is an extension, not a submission requirement. |
| HTML extraction                 | [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) | Parse HTML and select sections or metadata. Fetching a page is a separate step; inspect mathematical markup rather than assuming plain-text extraction preserves equations. |
| Scholarly discovery             | [OpenAlex API](https://help.openalex.org/api/)               | Find scholarly records and metadata. Check current access requirements and preserve search snapshots. Metadata availability does not imply accessible full text. |
| Embedding retrieval             | [Chroma](https://docs.trychroma.com/docs/overview/introduction) | Store embeddings and retrieve related passages. An index is optional when a small collection can be searched directly. |
| Small graphs                    | [NetworkX](https://networkx.org/documentation/stable/)       | Represent and inspect entities and relations in Python.      |
| RDF relations                   | [RDFLib](https://rdflib.readthedocs.io/en/stable/)           | Represent and query RDF statements. Define the meaning of your relations explicitly. |
| Tabular data                    | [pandas](https://pandas.pydata.org/docs/getting_started/overview.html) | Read tables, organize records, merge files, and perform explicit checks. |
| Numerical calculation           | [SciPy](https://docs.scipy.org/doc/scipy/tutorial/index.html) | Use numerical integration, optimization, fitting, and statistical routines appropriate to the problem. |
| Arrays and simulated noise      | [NumPy](https://numpy.org/doc/stable/)                       | Work with numerical arrays and controlled random sampling.   |
| Plotting                        | [Matplotlib](https://matplotlib.org/stable/users/getting_started/index.html) | Create figures with explicit units, labels, and uncertainty representations. |
| Symbolic calculation            | [SymPy](https://docs.sympy.org/latest/index.html)            | Manipulate symbolic expressions and check suitable identities under explicit assumptions. |
| Physical units                  | [Pint](https://pint.readthedocs.io/en/stable/)               | Represent quantities with units and perform compatible conversions. |
| Repeatable checks               | [pytest](https://docs.pytest.org/en/stable/)                 | Run numerical and structural checks with recorded expected outcomes. |
| Notebook execution              | [nbclient](https://nbclient.readthedocs.io/en/latest/)       | Execute a notebook programmatically and preserve outputs.    |
| Optimization studies            | [Optuna](https://optuna.readthedocs.io/en/stable/tutorial/10_key_features/003_efficient_optimization_algorithms.html) | Propose optimization trials and manage their evaluation. Use only when optimization matches the scientific objective. |
| Grammar suggestions             | [LanguageTool](https://languagetool.org/proofreading-api)    | Supply language-level checks. It does not establish scientific correctness. |
| Calendar files                  | [icalendar](https://icalendar.readthedocs.io/en/latest/)     | Read and write iCalendar data; read-only input is sufficient for Project 10. |
| Statistical surrogate models    | [scikit-learn Gaussian processes](https://scikit-learn.org/stable/modules/gaussian_process.html) | Optional regression and predictive uncertainty models for adaptive experiment design. |

## Final Remark

**The central question is what the automation contributes to a scientific task, and how a physicist can tell whether its result is usable.**
