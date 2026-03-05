# SYSTEM_DESIGN

Growth Execution Analysis is an AI-assisted deal intelligence system that turns fragmented deal evidence into a consistent, executive-ready strategic interpretation.

This document explains the architecture in plain English: what the system does, how data moves through it, why it is designed this way, and how you would extend it in a real production environment.

------------------------------------------------------------
1. What Problem This System Solves
------------------------------------------------------------

Leadership teams often review major wins and losses without enough context to answer the strategic question: “So what?”

Most organizations can list large deals and their outcomes, but the supporting evidence is scattered across:

- conversation transcripts
- email threads
- customer profile and account data
- subjective seller reflections

Without a consistent analysis layer, QBR-style reviews become anecdotal. The biggest deal in a quarter can be shown with little explanation of why it matters, whether it supports strategy, or whether it can be repeated.

Growth Execution Analysis standardizes this process. It creates a single structured deal dataset, then runs a consistent strategic analysis framework to produce outputs that leadership can act on.

------------------------------------------------------------
2. What the System Produces
------------------------------------------------------------

For each deal, the system produces:

A. Unified Deal Dataset
A normalized, structured record that consolidates all deal evidence into a consistent schema.

B. Strategic Deal Report
A long-form interpretation that evaluates the deal across multiple strategic dimensions (profile, buying signals, alignment, competitive dynamics, signal strength).

C. Executive “So What?” Summary
A short 2–3 sentence CEO-ready narrative that can be inserted into leadership reporting.

D. Quarterly Strategic Rollup
An aggregation layer that looks across multiple analyzed deals to identify repeatable patterns, strategic strengths, and recurring weaknesses.

------------------------------------------------------------
3. High-Level Architecture Overview
------------------------------------------------------------

The architecture follows a standard pattern used in production data/AI systems:

1. Collect evidence from multiple sources
2. Normalize and assemble into a canonical dataset
3. Run an analysis engine against a consistent framework
4. Produce outputs tailored to different audiences
5. Aggregate results across time periods for trend-level conclusions

In simplified form:

Evidence Sources
  - transcripts
  - emails
  - customer profile data
  - seller questionnaire
        ↓
Ingestion + Normalization
        ↓
Unified Deal Dataset
        ↓
LLM Strategic Analysis (Prompt + Output Contract)
        ↓
Deal Report + Executive Summary
        ↓
Quarterly Rollup (Pattern Extraction)

------------------------------------------------------------
4. Core Design Principles
------------------------------------------------------------

4.1 Evidence First, Interpretation Second
The system is designed to treat transcripts, emails, and account data as primary evidence, not just context. Seller reflections are included as an explicit input rather than the entire story.

4.2 Canonical Deal Dataset
Before analysis happens, the system builds a structured dataset. This makes the system:
- auditable (you can inspect what the model saw)
- repeatable (same inputs generate comparable outputs)
- extendable (you can add new evidence sources without rewriting analysis logic)

4.3 Prompt as a Versioned Asset
Prompts are stored as files, not embedded in code. This supports:
- versioning
- iteration
- evaluation against multiple prompt revisions
- reproducibility

4.4 Output Contract
The analysis output follows a strict contract. This prevents the system from generating unstructured narratives that are hard to compare across deals.

4.5 Connectors Are Swappable
External systems are represented as “connectors.” In production, these would integrate with real platforms. In this repo, they are stubs that model real interfaces.

This reduces coupling. The pipeline logic does not depend on any one vendor or data source.

------------------------------------------------------------
5. Data Flow by Stage
------------------------------------------------------------

Stage 1: Ingest Evidence

Inputs for a deal are collected from four sources:

1. Conversation Evidence
One or more transcripts associated with the deal.

2. Communication Evidence
Email threads between seller and customer during the deal.

3. Customer Profile Data
Firmographic and account metadata (size, region, product footprint, contract value).

4. Seller Questionnaire
A structured reflection completed after the deal outcome is known.

In production, each source would be pulled from a system of record:
- conversation intelligence platform
- email platform
- CRM or data warehouse
- internal survey tool

In this repo, inputs are stored under sample_data as synthetic examples.

Stage 2: Normalize Inputs

Raw artifacts come in different formats. Normalization converts them into consistent structures:

- transcripts become structured objects with IDs, timestamps, participants, and text
- emails become threads with participants, timestamps, and content blocks
- firmographics map into a fixed schema
- seller questionnaires map into a fixed schema

Normalization also applies optional redaction and cleaning rules to prepare data for downstream analysis.

Stage 3: Assemble the Unified Deal Dataset

The normalized artifacts are combined into one deal-level dataset that includes:

- deal metadata and customer profile
- concatenated or excerpted transcript content
- email highlights and timeline context
- seller reflections

This stage is the “single source of truth” for the analysis. The dataset is saved as an output artifact, which makes the system auditable.

Stage 4: Strategic Analysis

The unified dataset is passed to an analysis engine guided by:

- a strategy knowledge base document (generic in this repo)
- a win or loss prompt framework
- an output contract defining required sections and constraints

The analysis focuses on interpretation, not summary. It aims to distinguish signal from anecdote and to make explicit what the deal proves, suggests, and does not prove.

Stage 5: Generate Outputs

From the model output, the system produces:

- a long-form report (plain text)
- a short CEO-ready executive summary (2–3 sentences)
- metadata fields for aggregation (e.g., themes, risks, alignment markers)

Stage 6: Quarterly Rollup

When multiple deals are analyzed, the system aggregates outputs to identify patterns such as:

- repeated buyer pain themes
- recurring competitive dynamics
- alignment vs misalignment with strategy
- conditions that appear to correlate with wins or losses

In a production system, this stage would typically feed dashboards, leadership memos, or internal knowledge bases.

------------------------------------------------------------
6. Why This Architecture Works Well for Growth Engineering
------------------------------------------------------------

This system is designed like a growth engineering platform:

- It standardizes analysis so insights are comparable across deals.
- It builds leverage: a single analysis engine can process many deals.
- It embeds strategy directly into operational review, reducing anecdotal interpretation.
- It creates reusable infrastructure, not a one-off report.

This is the difference between:
- “analyzing deals”
and
- “building a system that continuously produces strategic deal intelligence.”

------------------------------------------------------------
7. Extending This System in Production
------------------------------------------------------------

7.1 Replace Stubs with Real Connectors
Connector stubs can be replaced with production connectors that:
- authenticate via OAuth or service accounts
- fetch data from APIs or data warehouses
- apply governance controls

7.2 Add Observability
Production deployments should add:
- structured logs per pipeline stage
- retries and backoff for external calls
- run metadata (timestamp, version, prompt hash)
- error handling with clear failure modes

7.3 Add Evaluation and Quality Controls
To maintain trust, the analysis engine should be evaluated using:
- golden test cases (curated deals with expected outputs)
- output validation against schema
- confidence tagging and evidence sufficiency checks
- prompt regression tests

7.4 Add Human Review Workflows
Executive narratives can be optionally routed through review:
- seller review before publication
- leadership review for high-impact deals
- feedback captured and used to improve prompts and strategy KB

7.5 Add Knowledge Base Management
The strategy knowledge base should be:
- versioned
- updated quarterly
- aligned to current initiatives

The system can record which KB version was used for each analysis.

------------------------------------------------------------
8. Key Files to Understand
------------------------------------------------------------

If you are reviewing this repository, these are the most important parts:

- docs/architecture.md
High-level system flow and purpose.

- docs/data-model.md
The canonical deal dataset structure.

- prompts/
Win and loss prompt frameworks plus output contract.

- sample_data/
Synthetic deals demonstrating how the system works end-to-end.

- src/
Pipeline logic for ingesting, assembling, analyzing, and producing outputs.

------------------------------------------------------------
9. Summary
------------------------------------------------------------

Growth Execution Analysis is a strategic intelligence pipeline that converts scattered deal evidence into consistent executive interpretation.

The architecture intentionally separates:
- data ingestion from analysis logic
- connectors from pipeline stages
- prompts from code
- outputs from inputs

This makes the system:
- repeatable
- auditable
- extendable
- suitable for production hardening

The public version in this repository uses synthetic data and stub integrations to demonstrate the end-to-end design without exposing sensitive information.
