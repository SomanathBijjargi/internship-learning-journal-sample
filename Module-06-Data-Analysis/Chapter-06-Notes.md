Here is the markdown code for the Vibe Analysis document based on the provided sources. As requested, the code block does not contain any citations. 

# Vibe Analysis

Vibe analysis is analyzing data as if the analysis itself doesn't exist—you focus only on business outcomes, skipping intermediate steps. This approach emerged from **vibe coding**, where you code as if code doesn't exist. With vibe analysis, you give an LLM full context, state your goal, and review only the answers. The LLM handles exploration, cleaning, modeling, visualization, and deployment.

## Core Vibe Analysis Workflow
From data to deployed insights without manual coding.
* **Automated exploration**: Using LLMs to generate and test hypotheses.
* **Data quality automation**: Detecting non-obvious issues and creating validation rules.
* **Visual storytelling**: Building publication-ready data stories with style transfer.
* **Meta-prompting techniques**: Improving prompts before execution.

## The Traditional Data Science Workflow vs. Vibe Analysis
A data scientist typically:
1. **Explores** the data (EDA, profiling, types, missing values)
2. **Cleans** it (deduping, anomalies, schemas)
3. **Models** it (features, algorithms, parameters)
4. **Explains** it (narratives, visualizations, slides)
5. **Deploys** it (APIs, dashboards, reports)
6. **Anonymizes** it (synthetic data for case studies)

Vibe analysis automates most of this cycle.

### What's Dying (Automated Tasks)
* **EDA**: Profiling, types, missing values, deduping, anomaly detection.
* **Scaffolds**: Loaders, schemas, docstrings, README files, tests.
* **AutoML**: Feature selection, model choice, parameter tuning, metrics.
* **Code writing**: LLMs can fill in 80% from specifications.
* **Explainers**: Generating narratives, visuals, presentations.
* **Forms**: Project reports, timesheets, documentation.

### What's Rising (Critical Skills)
* **Leadership**: Setting the right goal and allocation across human and AI teams.
* **Problem framing**: Precise prompts that reduce iteration cycles.
* **Eval design**: Automated validation with binary checks and LLM-as-judge.
* **Invariants**: Defining ontologies, declaring truths and constraints.
* **Verticalization**: Domain-specific datasets and workflows as competitive moats.
* **Trust & taste**: Auditing AI output, telling compelling stories.

## Automated Hypothesis Generation
Instead of manually exploring data, use LLMs to generate business hypotheses automatically. Let the LLM test everything in parallel. When done, have it craft an executive summary you can directly present.

## Vibe Analysis Workflow Steps
### 1. Provide Context
Upload your dataset to ChatGPT, Claude, or Codex. Give it business context.

### 2. Request Comprehensive Analysis
Ask for specific analyses OR ask it to discover insights. The LLM will write Pandas/NumPy code, run statistical tests, generate visualizations, and create summaries.

### 3. Let it Work Autonomously
Use **Agent Full Access** mode to eliminate approval prompts. Run multiple agents in parallel—one for analysis, one for planning, one for visualization.

### 4. Find Non-Obvious Insights
Have the LLM focus on surprising patterns, such as data errors, anomalies, or quality issues.

## Visual Data Stories
Create beautiful, interactive visualizations styled after leading publications.
* **Style Transfer Technique**: Instead of specifying every detail, reference a style (e.g., "in the style of the New York Times").
* **Generate and Publish**: The agent generates `index.html`, `style.css`, and `script.js`. You can then publish this directly to GitHub Pages.

## Creating Synthetic Data with Hypotheses
Don't generate random fake data. Instead, align synthetic data with business hypotheses. The LLM creates data where these patterns hold, making it useful for demos and case studies without exposing real data.

## Meta-Prompting
Use LLMs to optimize your prompts before running analysis. 
1. Write your initial prompt.
2. Add: "Suggest an improved prompt for this".
3. Review the improved version.
4. Run the improved prompt in a fresh session.

## Parallel Chat Strategy
Run multiple chats in parallel so you don't wait for agents to finish:
* **Chat 1**: Main analysis and coding.
* **Chat 2**: "What interesting, useful analysis can I run on this dataset?"
* **Chat 3**: "Find the most interesting, useful, non-obvious insights."

## Error Detection and Validation
* **Multi-model review**: Ask different models to find errors. Many eyes make bugs shallow.
* **Checksum validation**: Ask the LLM to provide category totals that sum to the overall total.
* **Post-validation prompts**: After analysis, ask the LLM to find errors in its own analysis.

## Context Management and Session Hygiene
* **Reset over repair**: Long conversations accumulate bad assumptions ("context rot"). Start a fresh chat with a clean prompt instead of iterating through broken context.
* **Summarize and restart**: Have the LLM summarize the prompt that will yield the exact final result, and run it in a new window.

## Meta-Analysis
Analyze your analysis process by reviewing LLM action logs to continuously improve prompts, tools, and workflows.

## Risk Mitigation Strategies
When shipping LLM insights to executives:
1. **Frame safe questions**: Focus on general business recommendations, options with tradeoffs, or directional insights.
2. **Reduce error likelihood**: Write code first then analyze, add citations for insights, request checksums.
3. **Post-validation**: Ask the LLM to find errors, use multiple models, run automated statistical checks.

## Best Practices Summary
1. Provide full context upfront.
2. Separate planning from execution.
3. Run parallel sessions.
4. Use style transfer over detailed specs.
5. Create repeatable scripts after successful analysis.
6. Validate with multiple models.
7. Start fresh sessions when context rots.
8. Practice daily on personal data.
9. Frame questions safely.
10. Extract and commit prompts as your source code.

## The New Data Scientist Role
Your role has changed:
* **Don't** manually explore, clean, model, or visualize anymore.
* **Do** frame problems clearly, design evaluations, define invariants, audit outputs, and tell compelling stories.
* **Think** like a team lead managing both human and AI intelligence.
* **Focus** on domain expertise and taste, not code.
