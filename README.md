# Growth Execution Analysis

**Growth Execution Analysis** is an AI-assisted system designed to analyze large enterprise deals and extract strategic insights for leadership teams.

The system aggregates multiple sources of deal evidence and produces a structured **“So What?” interpretation** that explains what the outcome of the deal means from a strategic growth perspective.

The goal is to transform raw operational deal data into **executive-level intelligence** that can inform strategy, messaging, and go-to-market execution.

This repository contains a **sanitized demonstration version** of the system using synthetic data.

> No real company information, customers, or internal systems are represented.

---

# System Purpose

Organizations often review major wins and losses without deeper context. Leadership may see which deals closed or were lost but lack a structured understanding of **why those outcomes occurred** or **what they imply about the company’s growth strategy**.

Growth Execution Analysis solves this problem by combining evidence from multiple sources and generating a structured strategic interpretation of the deal.

The system answers questions such as:

- What does this deal actually mean?
- Is this outcome aligned with our growth strategy?
- Is this a repeatable pattern or a one-off event?
- Does this indicate market momentum or structural weakness?
- What should leadership conclude from this outcome?

---

# Input Data Sources

Each deal is constructed from **four primary inputs**.

## 1. Conversation Evidence

Conversation transcripts related to the opportunity.  

These may include:

- Sales calls  
- Discovery conversations  
- Product demos  
- Executive meetings

---

## 2. Communication Evidence

Email conversations or written exchanges between the customer and the seller during the deal lifecycle.

These communications provide insight into:

- buyer priorities
- evaluation dynamics
- competitive positioning
- urgency and risk signals

---

## 3. Customer Profile Data

Firmographic and account information such as:

- Customer size
- Industry segment
- Geography
- Existing product footprint
- Contract timing
- Estimated contract value

This information helps contextualize the strategic importance of the deal.

---

## 4. Seller Review Questionnaire

A structured questionnaire completed by the seller after the deal closes or is lost.

The questionnaire captures qualitative reflections including:

- How the deal progressed
- Unexpected events
- Customer needs alignment
- Lessons learned
- Strategic alignment with company initiatives

---

# System Output

The system produces **three levels of analysis**.

## Deal Dataset

A normalized structured dataset combining all evidence related to the deal.

---

## Strategic Deal Report

A long-form analysis that interprets the deal across multiple strategic dimensions.

---

## Executive “So What?” Summary

A concise **two-to-three sentence interpretation** designed for leadership reporting.

---

## Quarterly Strategic Rollup

When multiple deals are analyzed, the system aggregates findings and produces a **quarterly narrative** describing emerging patterns across deals.

---

# High Level Architecture

```
Data Sources
   • Conversation transcripts
   • Customer emails
   • Firmographic account data
   • Seller questionnaire

        ↓

Data Assembly Pipeline

        ↓

Unified Deal Dataset

        ↓

LLM Strategic Analysis

        ↓

Outputs
   • Strategic Deal Report
   • Executive Summary
   • Quarterly Strategic Rollup
```

---

# Repository Structure

```
growth-execution-analysis/

docs/
  Architecture and methodology documentation

src/
  Pipeline implementation

prompts/
  Strategic analysis prompts used by the system

sample_data/
  Synthetic example deals used to demonstrate the pipeline

outputs/
  Generated analysis outputs

scripts/
  Example commands for running the demo pipeline
```

---

# Running the Demo

### Install dependencies

```bash
pip install -r requirements.txt
```

### Run the demo pipeline

```bash
python src/main.py
```

The pipeline will read the **synthetic sample deals** in the `sample_data` directory and generate analysis outputs in the `outputs` directory.

---

# Security and Sanitization

This repository contains **only synthetic data**.

Customer names, emails, transcripts, and firmographic data are fictional and provided solely to demonstrate the architecture of the system.

Real integrations with external systems are represented by **stub connectors**.

---

# License

MIT License
