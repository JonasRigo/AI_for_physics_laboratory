# AI for physics laboratory

## Workflow automation projects

**Work in pairs. Projects are assigned to you.** Develop a small, useful automation for a clearly defined physics task. Your project has two main outcomes:

1. **Upload a reusable workflow to the shared course GitHub repository**, with the instructions and examples your fellow colleagues need to run and adapt it.
2. **Give a 45-minute presentation** explaining what the workflow does, how it works, why you designed it that way, and what its results and examples demonstrate.

The aim is to contribute a useful tool to the class and teach your fellow colleagues how to understand and use it.

Projects 1–10 form the main catalogue. Projects 11–15 are optional alternatives. You are free to propose your own project, after deliberation with the instructor.

### Freedom of implementation

You may use Langflow, another visual workflow editor, Python, notebooks, or your preferred software. **All software suggestions in this document are optional.** You may replace a suggested tool without changing the scientific objective or the required deliverables. A command-line workflow or notebook can be a complete submission; a polished application interface is unnecessary.

Your workflow should contain at least one clearly motivated use of a **generative model** or **embedding model**. Explain its role and why its task can not be performed by any other traditional algorithm.

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
- Try to compare the behaviours of different LLMs, different context length and reasoning settings.
- A workflow that identifies insufficient information or fails with interpretable traces can be more useful than one that always produces a confident answer. An honest negative result is a valid project outcome.

### Outcome 1: a reusable contribution to the shared GitHub repository

Upload your completed workflow to the shared course GitHub repository using the submission procedure provided by the instructor. The workflow must be available for your fellow colleagues to download, run, inspect, and adapt. An exported flow alone is sufficient only if it includes everything needed to execute the documented example.

Your contribution must contain:

- **The executable workflow:** flow exports, scripts, or notebooks, including custom components and required prompts or configuration files.
- **A README:** the purpose or function, supported inputs and outputs, prerequisites, software versions, model/service requirements, installation or import steps, and exact instructions for running an example.
- **An example input and its recorded output:** explain what another collegue should expect to see, including acceptable variation in stochastic outputs.
- **The four deliverables:** task specification, evaluation results and procedures, and account of human judgment alongside the executable workflow. Explain the key design choices and how to adapt the example to a new question or dataset.

For your submission create a folder `workflow_name/` and structure with like this: `README.md`, `workflow/`, `example/`, `evidence/` and `evaluation.md` (equivalent organization is welcome). Written explanations may share a file.

**WARNING: Remember to never share or upload credentials or API keuys!**

**Before submitting, ask another pair to run the example using your instructions.** Record any missing steps they identify and correct the instructions. If an external service or account requirement prevents execution, document that limitation and make the recorded example available for inspection.

### Outcome 2: a 35-minute presentation

Your presentation should teach your collegues how the workflow works and why its design is appropriate for the task. Show the scientific problem, the mechanism of the automation, the decisions you made, and actual examples of its behavior.

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

**Short explanation.** RAG stands for *retrieval augmented generation*, it allows agentic AI to retrieve information from a knowledge base to answer questions from facts and not memory. Facts and also be *ingested* to dynamically update the agent knowledge.

**Assignment.** Build an assistant that answers physics questions using a collection of papers or lecture-notes. Answers must identify the passages that support their important statements.

Choose one main approach: retrieval-augmented generation (RAG), graph-assisted retrieval (GraphRAG), or ontology-augmented generation (OAG). In this assignment, OAG means using an explicit description of concepts, relations, or constraints to guide retrieval or interpretation. You do not need to implement all three approaches, but ideall you build an OAG.

If you go the graph extraction route use the reference graph extraction worfklow.

**Suggested route.** Load documents, preserve source locations, divide text into useful passages (ingestion), retrieve evidence for a question, generate an answer, and check its support. A graph variant might extract a graph (see reference workflow), follow links between a model, its assumptions, and its predictions. For maximum flexibility: make the ingestion of documents and retrieval of passges separate *tools* that you can expose to an agent.

**Optional software hints.** Langflow can connect retrieval and generation and turn HTML into structured text. ChromaDB can store and search embeddings. NetworkX can hold a small graph without a separate database; RDFLib is an option for RDF-based relations. See the tool guide below.

**Comments.** Use a user-provided collection. New literature discovery belongs to Project 2. Building a general physics ontology is unnecessary.

------

## 2. Literature research with review

**Assignment.** Build a workflow that investigates a question or a topic, searches and screens for candidate papers, and produces a literature synthesis. To improve the synthesis results use the reference reference graph extraction worfklow.

**Suggested route.** Specify criteria, discover candidates, remove duplicates, review one paper at a time, record inclusion decisions, extract claims and relations, then synthesize the accepted evidence.

**Optional software hints.** OpenAlex can supply scholarly metadata and search results. Beautiful Soup or Langflow's URL/Parse components can parse accessible article HTML, but it does not retrieve paywalled full texts or understand equations automatically. Langflow can organize discovery and one-paper review stages. NetworkX and a CSV file are sufficient for a first graph and screening record.

**Comments.** A general-purpose research agent and exhaustive coverage are outside the minimum task. Define a policy for missing full text rather than treating an abstract as a full-paper review.

**Comments.** Let reviewer feedback propose a revised search query within the fixed search budget. Use arxiv papers for easily accessible search and note: Arxiv HTML papers are easier to read for a machine than PDFs (PDFs are hard to read!) and arxiv search does not rank results liek OpenAlex.

------

## 3. A physics tutor and examiner

**Assignment.** Build a tutor for a user suplied topic, such as the harmonic oscillator, dimensional analysis, or numerical integration. The tutor should explain ideas, respond to answers, and choose a suitable follow-up exercise. An examiner stage should assess answers using explicit criteria.

Prepare **three learning objectives and six reference-checked questions**, including a conceptual question and a calculation. Use simulated collegue responses for evaluation; a study with real learners is not required.

**Suggested route.** Establish the learner's starting point, offer an explanation or hint, collect an answer, assess it, and choose a next step. Bound the number of exchanges and finish with a summary of demonstrated understanding and unresolved difficulties.

**Optional software hints.** Langflow can separate tutor and examiner stages. Plain JSON can hold questions and marking criteria. SymPy can check suitable algebraic expressions under stated assumptions. A notebook or a simple chat interface is enough.

**Comments:** Teach one topic. The tutor and examiner may use the same model, but their agreement cannot replace reference checking.

**Optional extension.** Adapt question difficulty using a transparent record of demonstrated skills.

------

## 4. Reproduce one result from a paper

**Assignment.** Use a generative model within a workflow that translates a paper's method into executable code and attempts to reproduce **one figure, table entry, or quantitative result**. Choose a result that fits your available hardware and time budget.

**Suggested route.** Identify the target, extract equations and numerical choices, inspect omissions, generate or adapt code, run it, compare against the target, and perform at most three documented correction rounds.

**Optional software hints.** Langflow can organize extraction, generation, execution, and review. SciPy can supply numerical solvers, Matplotlib can produce plots, and pytest can run scientific checks. nbclient can execute notebooks automatically.

**Comments.** Reproducing the whole paper or matching a figure visually without a quantitative comparison is outside the assignment. A documented unsuccessful reproduction is valid if it identifies the remaining obstacle. Choose a paper from the arxiv, since they give you access to HTML versions (easier to read for LLMs than PDFs) and access to the Latex source code.

**Optional extension.** Test a prediction at a parameter value not shown in the paper.

------

## 5. Experimental data analysis and presentation

**Assignment.** Build a workflow that turns measurements and a description of the experiment into a justified analysis, uncertainty estimates, and clear plots. Use one experiment, such as a damped oscillation, an RC circuit, or a calibration curve.

Use an openly available dataset of your choice, or generate realistic synthetic measurements with known parameters. The generative model may interpret the measurement description, propose an analysis plan, or explain results; numerical calculations must be inspectable..

**Optional software hints.** Langflow can organize the analysis stages and call a Python component. pandas (python library that works natively with Langflow) can read and organize tables, SciPy can fit models, Matplotlib can plot results, and Pint can help handle units. See the tool guide for documentation.

**Comments.** One measurement type and one principal analysis are sufficient. Automatic selection among every possible fit model is unnecessary. Possible data set is [LIGO: GW150914 discovery figure](https://dcc.ligo.org/LIGO-G1600204-v6/public?utm_source=google.com): Load the two measured signals, reproduce their comparison, estimate the relative time delay, compare with the supplied theoretical waveforms, and produce a report describing residuals and analysis limitations.

**Optional extension.** Compare two physically motivated models or add a documented treatment of correlated calibration uncertainty.

------

## 6. Analytic derivation with verification

**Assignment.** Build a workflow that develops a derivation, checks it, and returns either a supported result or an explicit unresolved step. Select one family of problems, such as linear differential equations, commutator identities, or a thermodynamic relation under specified assumptions.

**Suggested route.** Record the claim and assumptions, propose a method, attempt the derivation, check individual steps or seek a counterexample, then revise the argument. Use at most three revision rounds. If the claim changes, preserve the original and explain the change.

**Optional software hints.** Langflow can implement the bounded proposal/check/revision cycle. SymPy can perform symbolic manipulation. Numerical substitution can help find counterexamples. For a familiar finite-dimensional operator problem, explicit matrix calculations may provide useful checks.

**Comments.** An arbitrary theorem prover is unnecessary. Use a problem family you can independently assess.

**Optional extension.** Add a dedicated counterexample search before accepting a claim.

------

## 7. Orchestration of a numerical experiment

**Assignment.** Build a workflow that runs a small numerical study, evaluates its results, and allocates a limited budget of further runs. Choose either convergence control or hyperparameter (e.g. integration step size, time step size, etc.) optimization as the main objective. Finish with a separate production run or data accumulation stage using the selected settings.

Example tasks include selecting an integration step for a target accuracy, choosing sampling settings for an observable, or tuning a small numerical approximation.

**Optional software hints.** Langflow can organize run selection and review. SciPy can provide a small simulator. Optuna is an option for optimization, while a fixed grid may be a better starting point. Store configurations and results in JSON or CSV.

**Comments.** Start on a laptop with a proposed budget of no more than 20 short tuning runs per study, adjusted with the instructor. Cluster integration is optional. Do not stop solely because an LLM says the result looks converged.

**Application proposal.** [How reliably can a small neural network distinguish the two phases, and how does its inferred transition depend on training data, network size, and random seed?](https://arxiv.org/abs/1605.01735?utm_source=google.com)

| STAGE                    | WHAT THE WORKFLOW DOES                                       |
| ------------------------ | ------------------------------------------------------------ |
| Generate data            | Run Monte Carlo simulations at specified temperatures; save configurations and simulation settings (16x16 sites for H = \sum_{ij} s^z_i s^z_j) |
| Establish a baseline     | Classify configurations using their absolute magnetisation \lvert m\rvert. |
| Tune the network         | Compare a set of hidden-layer sizes and regularisation strengths. |
| Select a model           | Use validation performance, with a preference for a smaller model when performance is comparable. |
| Accumulate final results | Retrain the selected configuration with several seeds and evaluate on independent simulations. |
| Report                   | Plot predictions versus temperature, uncertainty across runs, baseline comparisons, and computational cost. |

------

## 8. Explanation of code or a paper with a reviewed concept graph

**Assignment.** Build a workflow that explains either one small code module or one self-contained section of a physics paper. The explanation should connect statements to source locations and show how the important concepts depend on each other.

Choose a target such as approximately 100–300 lines of scientific code or 2–4 pages containing a derivation. Adjust these bounds for mathematical density.

**Optional software hints.** Langflow can separate extraction, explanation, and review. NetworkX can store the concept graph. Beautiful Soup can extract article HTML. For code, language parsers or Python's standard `ast` module can help identify structure.

**Comments.** Explain one selected target. A graph, as produced by the reference graph extraction worfklow, or the [graphify](https://github.com/Graphify-Labs/graphify) tool is very helpful for the user and the agent to better understand code or math.

**Optional extension.** Offer a second explanation for a reader with different prior knowledge while retaining the same source links.

------

## 9. A scientific text editor with terminology and consistency checks

**Assignment.** Build an editor for short physics texts that proposes traceable changes to language and identifies possible scientific inconsistencies. Use a 500–1,000-word example and a small supplied collection of definitions or reference passages.

Create a compact terminology specification: approximately ten terms or symbols, their definitions, relevant relations, and at least five explicit consistency rules. This can be a structured table or a small ontology. A glossary alone should not be described as a complete ontology.

**Optional software hints.** Langflow can separate language review from evidence and terminology checks. LanguageTool is an optional grammar-checking component. RDFLib can represent an ontology if useful; a reviewed JSON table is sufficient for the minimum scope. Standard text-difference tools can produce the change list.

**Comments.** The editor checks against supplied evidence and explicit rules. It does not certify the truth of an arbitrary manuscript. A suggestion list is sufficient; no word-processor plugin is required.

**Optional extension.** Add a check that accepted edits preserve equations, numerical values, and defined notation.

------

## 10. A daily research briefing

**Assignment.** Build a workflow that combines a calendar and project task list into a concise daily briefing. The briefing should distinguish fixed events, explicit deadlines, preparation requirements, conflicts, and suggested priorities.

Use a **synthetic calendar covering one week and 10–15 tasks**. Treat briefing generation as an on-demand operation with a supplied date and time; a deployed scheduler is optional. You can also search the web for information, like news, weather and fun new physics articles. These web summaries can be added to the daily breifing.

**Optional software hints.** Langflow can join the inputs and generate the briefing. Python's `icalendar` package can read `.ics` files. JSON or CSV can hold tasks, and standard date/time libraries can handle time zones. Live account connections are unnecessary for the assignment.

**Comments.** A briefing is sufficient. Automatic rescheduling, email sending, and integration with multiple private services are outside the minimum task.

**Optional extension.** Generate a weekly planning summary using the same evidence record.

------

# Optional alternative projects

These projects use exactly the same five deliverables and evaluation requirements. They can replace any project in the main catalogue.

## 11. Equation-to-code verification

**Assignment.** Build a workflow that translates one family of physics equations into executable code and verifies the implementation against independent checks. A suitable example is a driven or damped oscillator with specified initial conditions.

**Optional software hints.** SymPy can manipulate equations and reference expressions, SciPy can solve suitable numerical problems, and pytest can execute checks. Langflow can organize translation and review. A notebook can present both equations and computed results.

**Comments.** One equation family is sufficient. Unlike Project 4, no paper reproduction or method extraction from a publication is required.

**Optional extension.** Compare two numerical methods under the same accuracy target.

------

## 12. Diagnosis of failed or suspicious simulations

**Assignment.** Build an assistant that uses a small simulator's configuration, logs, and outputs to propose possible causes of failure and select a few diagnostic checks. Include failures that produce plausible-looking output, not only exceptions.

Use a familiar simulator and at least four controlled defects, together with a successful control run.

**Optional software hints.** Langflow can coordinate diagnosis and diagnostic tools. Python's logging module can create consistent logs, pandas can summarize outputs, and pytest can check known symptoms. A simple local simulator is sufficient.

**Comments.** Diagnosis and a justified repair proposal are sufficient. Arbitrary modification of a research codebase is unnecessary.

**Optional extension.** Apply one approved repair and rerun the scientific checks.

------

## 13. Scientific dataset curation

**Assignment.** Build a workflow that combines measurement or simulation files into a documented dataset. It should reconcile documented units and column names, detect duplicates and missing metadata, and separate ambiguous records for review.

Start with approximately **ten small files** from one type of experiment or simulation. Use the generative model for a specific semantic task, such as interpreting free-text headers or proposing column mappings, and validate the proposed mapping before applying it.

**Optional software hints.** pandas can read, combine, and validate tables. Pint can handle compatible units. Langflow can invoke an LLM for ambiguous headers and route uncertain proposals to review. CSV plus JSON metadata is sufficient.

**Comments.** Curate one dataset family. Do not invent missing measurements or silently infer unrecorded units.

**Optional extension.** Add a summary of how the curated dataset differs from its previous version.

------

## 14. Adaptive design of a simulated experiment

**Assignment.** Build a workflow that chooses successive measurements in a simulated physics experiment to reduce parameter uncertainty or distinguish between two candidate models. Use a one-dimensional control variable and an explicit measurement budget.

Suitable examples include selecting frequencies around a resonance or times at which to measure an exponential decay. The generative model can interpret the experiment request or explain proposed choices. The numerical selection criterion must remain inspectable.

**Optional software hints.** SciPy can supply a simulator and parameter fits. NumPy can generate controlled noise. Langflow can organize the measurement loop. A Gaussian-process model from scikit-learn is an optional extension when useful; it is unnecessary for a simple parameter-fitting experiment.

**Comments.** Use a simulated experiment. Hardware control and a fully general autonomous laboratory are unnecessary.

**Optional extension.** Examine how the selection strategy behaves under a misspecified noise model.

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
