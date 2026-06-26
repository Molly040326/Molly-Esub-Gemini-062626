# TFDA-Copilot: Next-Generation Generative AI Regulatory Review & Defense Architecture Specification

**Document Version:** 3.1.0-PROD  
**Target Runtime:** Node.js 20+ / Cloud Run Sandboxed Containers / React 18 Concurrent SPA  
**Primary Regulatory Frameworks:** Taiwan Medical Devices Act (醫療器材管理法), TFDA Regulations Governing Issuance of Medical Device Licenses, EU CE MDR (2017/745), US FDA 21 CFR Part 807/809/820, Japan PMDA Act on Securing Quality, Efficacy and Safety of Products.  
**Default AI Engine:** Google GenAI SDK (`@google/genai`) targeting `gemini-3.1-flash-lite`  

---

## Table of Contents

1. [Executive Summary & System Vision](#1-executive-summary--system-vision)
2. [Core Architectural Blueprint & State Engine](#2-core-architectural-blueprint--state-engine)
3. [Subsystem Detailed Specifications (Existing Core Modules)](#3-subsystem-detailed-specifications-existing-core-modules)
   - 3.1 [MDEA Resolution Engine (Clause 02 Address Alignment Matrix)](#31-mdea-resolution-engine)
   - 3.2 [Regulatory Document Version Comparison Vault](#32-regulatory-document-version-comparison-vault)
   - 3.3 [AI Risk Assessment Scoring Engine](#33-ai-risk-assessment-scoring-engine)
   - 3.4 [ISO 14971 Risk Management Matrix (FMEA / RPN Calculator)](#34-iso-14971-risk-management-matrix)
   - 3.5 [Cross-Lingual IFU Harmonization Engine](#35-cross-lingual-ifu-harmonization-engine)
   - 3.6 [QMS Audit Trail Visualization & Immutable Document Control](#36-qms-audit-trail-visualization)
   - 3.7 [Automated Deficiency Drafting Engine](#37-automated-deficiency-drafting-engine)
4. [Specification of 3 Additional Wow AI Regulatory Features](#4-specification-of-3-additional-wow-ai-regulatory-features)
   - 4.1 [Wow Feature A: Real-Time Post-Market Surveillance (PMS) Adverse Event Graph Synthesizer](#41-wow-feature-a-real-time-pms-adverse-event-graph)
   - 4.2 [Wow Feature B: Automated Predicate Substantial Equivalence (SE) Triangulation Matrix](#42-wow-feature-b-automated-predicate-se-triangulation)
   - 4.3 [Wow Feature C: AI Cyber-Defense & AST Confidentiality Redaction Engine v3.0](#43-wow-feature-c-ai-cyber-defense--ast-redaction)
5. [LLM Integration & Prompt Engineering Architecture](#5-llm-integration--prompt-engineering-architecture)
6. [Defect Prevention, Concurrency & Container Resilience Engineering](#6-defect-prevention-concurrency--container-resilience-engineering)
7. [Verification & Quality Assurance Matrix](#7-verification--quality-assurance-matrix)
8. [20 Comprehensive Engineering & Regulatory Follow-Up Questions](#8-20-comprehensive-engineering--regulatory-follow-up-questions)

---

## 1. Executive Summary & System Vision

The **TFDA-Copilot** is an enterprise-grade, mission-critical regulatory software platform engineered specifically to bridge the analytical gap between medical device manufacturers (sponsors) and regulatory review authorities, specifically the Taiwan Food and Drug Administration (TFDA). Navigating the lifecycle of Software as a Medical Device (SaMD), In Vitro Diagnostic (IVD) reagents, and high-risk Class III active implants requires meticulous cross-examination of massive Summary Technical Documentation (STED) dossiers. Traditional review processes suffer from human fatigue, cross-lingual semantic misalignment, physical address formatting discrepancies across international health authorities, and unquantified clinical bias risks.

TFDA-Copilot eliminates these systemic vulnerabilities by introducing an agentic, offline-first client browser sandbox augmented with server-side AI intelligence. Operating on a **strict single-view architectural ceiling**, the system consolidates ten specialized review workflows into an intuitive, high-contrast, zero-latency interface. By enforcing zero client-side exposure of API secrets and binding exclusively to sandboxed container infrastructure on container ingress port 3000, TFDA-Copilot establishes a new benchmark for regulatory compliance automation, forensic auditability, and clinical patient safety.

```
+---------------------------------------------------------------------------------------------------+
|                                      TFDA-COPILOT SYSTEM TOPOLOGY                                 |
+---------------------------------------------------------------------------------------------------+
|  [Client Browser SPA - React 18 / Tailwind CSS / Lucide Icons / Virtualized DOM]                  |
|     |                                                                                             |
|     +---> Tab 01: Interactive Annotation Workspace (Sticker & Redaction Sandbox)                 |
|     +---> Tab 02: Document Version Diff Comparison Vault (Surgical Line-by-Line AST Diffs)        |
|     +---> Tab 03: STED Submission Vault & Clause 02 MDEA Address Alignment Matrix                 |
|     +---> Tab 04: AI Risk Scoring Dashboard (Multi-Factor Patient Safety & Regulatory Weights)    |
|     +---> Tab 05: ISO 14971 FMEA Risk Management Matrix (RPN 3D Calculation Engine)               |
|     +---> Tab 06: Cross-Lingual IFU Harmonization Engine (PMDA / CE MDR to TFDA Alignment)        |
|     +---> Tab 07: QMS Forensic Audit Trail Visualizer (Role-Based Mutation Lineage)               |
|     +---> Tab 08: SaMD Clinical Defense & Rebuttal Simulator (CLSI EP07-A3 Monte Carlo)         |
|     +---> Tab 09: Official TFDA Deficiency Drafting Engine (Statutory Article 25 Synthesis)       |
|     +---> Tab 10: Wow AI Advanced Sandbox (PMS Graph, SE Triangulation, AST Encryption)           |
+---------------------------------------------------------------------------------------------------+
                                                  |
                                   [Secure Nginx Reverse Proxy]
                                        (Port 3000 Ingress)
                                                  |
+---------------------------------------------------------------------------------------------------+
|  [Backend Server Runtime - Node.js Express / TSX Dev / Esbuild Bundled dist/server.cjs]           |
|     |                                                                                             |
|     +---> /api/gemini/default  ---> Google GenAI SDK (@google/genai)                              |
|     |                               Model: gemini-3.1-flash-lite (Default High-Throughput Triage) |
|     +---> /api/gemini/pro      ---> Model: gemini-2.5-pro (Deep Clinical Reasoning & SE Matrix)   |
|     +---> /api/gemini/stream   ---> Server-Sent Events (SSE) Asynchronous Streaming Channel       |
+---------------------------------------------------------------------------------------------------+
```

---

## 2. Core Architectural Blueprint & State Engine

The application architecture adheres to a rigorous **unidirectional data flow** governed by TypeScript strict type checking. To prevent memory exhaustion during long review sessions involving multi-megabyte regulatory dossiers, state management is partitioned into immutable domain stores.

### 2.1 State Store Structure

The primary application state is encapsulated within top-level React hooks in `App.tsx`, passed down via memoized props (`React.memo`) to prevent cascading re-renders.

```typescript
// Core Application State Topology
interface AppRootState {
  activeTab: ActiveTabId;
  llmConfig: {
    defaultModel: 'gemini-3.1-flash-lite';
    fallbackModel: 'gemini-2.5-pro';
    temperature: number; // Default: 0.1 for deterministic regulatory compliance
    systemInstructionOverride: string;
  };
  dossierRepository: {
    masterSTED: RegulatoryDocVersion[];
    activeAnnotations: Annotation[];
    auditDiscrepancies: DifferencesTableItem[];
    mdeaMatrix: AddressResolution[];
    riskScores: RiskScoringItem[];
    fmeaMatrix: RiskMatrixItem[];
    lingualDiffs: CrossLingualIfuDiff[];
    qmsTrail: QmsAuditTrailLog[];
  };
}
```

### 2.2 DOM Virtualization & Memory Limits

Regulatory technical files (such as clinical study reports or line data listings) routinely exceed 10,000 lines. Standard DOM rendering of such structures causes immediate browser frame drops and garbage collection pauses. TFDA-Copilot mandates the use of **windowed slice rendering** for all tables and document viewers:
1. **Viewport Bounding:** View components calculate container heights via `ResizeObserver` and render only the visible $\pm 25$ buffer items.
2. **String Interning:** Large text blocks are retained in raw primitive buffers rather than split into millions of React nodes.
3. **Canvas Decoupling:** Graphical dashboards (such as the FMEA risk matrix and QMS timeline) utilize HTML5 Canvas or SVG layers debounced to render at a maximum of 60fps during container window resizing.

---

## 3. Subsystem Detailed Specifications (Existing Core Modules)

### 3.1 MDEA Resolution Engine (Clause 02 Address Alignment Matrix)

According to *TFDA Medical Device Registration and Issuance Regulations Article 11*, the manufacturing site address listed on the Application Form, the Certificate of Free Sale (CFS) issued by the country of origin (e.g., US FDA), and the Quality System Documentation (QSD) approval letter must demonstrate absolute physical equivalence.

#### Algorithmic Trap Mechanisms
The MDEA engine implements a 3-stage lexical normalization pipeline:
* **Stage 1 (Case & Punctuation Stripping):** Converts all inputs to uppercase ASCII and eliminates trailing commas, periods, and redundant whitespace.
* **Stage 2 (Administrative Abbreviation Reduction):** Applies official postal mapping dictionaries (`SUITE` $\rightarrow$ `STE`, `BUILDING` $\rightarrow$ `BLDG`, `DRIVE` $\rightarrow$ `DR`, `AVENUE` $\rightarrow$ `AVE`, `FLOOR` $\rightarrow$ `FL`). If discrepancies are strictly confined to Stage 2 reductions, the system assigns a compliance status of `Warn` (🟡 Administrative Tolerance).
* **Stage 3 (Physical Spatial Boundary Traps):** Compares physical spatial unit identifiers (`ROOM`, `UNIT`, `BLOCK`, `FLOOR`). If Document A specifies `Suite A, Bldg 2` and Document B specifies `Room A, 2nd Floor`, the engine triggers a **Hard Blocking Failure** (`Fail` 🔴). The system automatically attaches a formal review note mandating the submission of an official, 3D architectural floor plan certified by the original manufacturer to prove physical contiguity.

---

### 3.2 Regulatory Document Version Comparison Vault

Sponsors frequently submit amended STED dossiers during deficiency remediation. Reviewers must immediately identify what text was altered, inserted, or redacted without reading 500 pages manually.

#### Technical Implementation
* **Ingestion:** Files uploaded via drag-and-drop are parsed into indexed line arrays (`RegulatoryDocVersion`).
* **Diff Engine:** Employs a dynamic programming Longest Common Subsequence (LCS) algorithm to generate line-by-line surgical delta blocks (`DocVersionChange`).
* **Significance Triage:** An automated heuristic tags each delta:
  * `Critical`: Modifications to clinical performance endpoints, contraindications, storage temperatures, or sterilization methods.
  * `Major`: Alterations to manufacturing steps, packaging specifications, or software revision numbers.
  * `Minor`: Typographical corrections, formatting adjustments, or renumbering of reference appendices.

---

### 3.3 AI Risk Assessment Scoring Engine

To quantify the overall risk profile of a medical device submission, the platform integrates a weighted multi-factor scoring algorithm (`RiskScoringItem`).

#### Mathematical Scoring Formula
For each identified compliance issue $i$ (derived from audit discrepancies, SaMD contamination alerts, or QMS mismatches), the system evaluates three orthogonal vectors on a scale of $1$ to $10$:
* $P_i$: **Patient Safety Impact** (1 = Negligible transient discomfort, 10 = Death or irreversible organ impairment).
* $R_i$: **Regulatory Statutory Impact** (1 = Formatting guideline suggestion, 10 = Direct violation of Medical Devices Act compulsory clauses).
* $S_i$: **Clinical Severity of Non-Compliance** (1 = Minor administrative delay, 10 = Systematic falsification of clinical trial verification data).

The composite weighted risk score $W_i \in [0, 100]$ is computed as:

$$W_i = \min\left(100, \left(0.40 \cdot P_i + 0.35 \cdot R_i + 0.25 \cdot S_i\right) \times 10\right)$$

#### Tier Stratification
* **Extreme Risk ($W_i \ge 80$):** Immediate rejection recommendation. Requires convening an emergency TFDA advisory panel.
* **High Risk ($60 \le W_i < 80$):** Mandatory formal deficiency letter issuance with explicit 30-day correction deadlines.
* **Moderate Risk ($30 \le W_i < 60$):** Sponsor clarification required during post-market surveillance commitment.
* **Low Risk ($W_i < 30$):** Approved with standard post-market quality reporting notes.

---

### 3.4 ISO 14971 Risk Management Matrix (FMEA / RPN Calculator)

Medical device software and hardware safety must comply with *ISO 14971:2019 Application of Risk Management to Medical Devices*. The FMEA tab (`RiskMatrixItem`) provides a live calculation engine.

#### Risk Priority Number (RPN) Mechanics
Users define three parameters ($1$ to $5$ integer scale):
1. **Severity ($S$):** Catastrophic (5), Critical (4), Serious (3), Minor (2), Negligible (1).
2. **Probability of Occurrence ($P$):** Frequent (5), Probable (4), Occasional (3), Remote (2), Improbable (1).
3. **Detectability ($D$):** Undetectable prior to patient harm (5), Low detectability (4), Moderate (3), High (2), Immediate automatic failsafe trap (1).

$$\text{RPN} = S \times P \times D \quad (\text{Range: } 1 \text{ to } 125)$$

#### ALARP Decision Thresholds
* **Unacceptable Zone ($\text{RPN} \ge 60$ or $S=5 \land P \ge 3$):** Design modification compulsory. Risk mitigation controls must be implemented in software architecture (e.g., adding CRC32 memory corruption checkups).
* **ALARP Zone ($25 \le \text{RPN} < 60$):** As Low As Reasonably Practicable. Requires formal cost-benefit safety analysis documented in STED Annex.
* **Acceptable Zone ($\text{RPN} < 25$):** Broadly acceptable safety margin.

---

### 3.5 Cross-Lingual IFU Harmonization Engine

Imported medical devices originate from global jurisdictions. Sponsors frequently translate Japanese (PMDA) or German (CE MDR) Instructions for Use (IFU) into Traditional Chinese and English for Taiwan registration. Subtle commercial translations frequently loosen safety warnings.

#### Cross-Lingual Verification Pipeline
1. **Extraction:** Ingests foreign text (`foreignOriginalText`) and official approved English text (`approvedEnglishIfu`).
2. **Neural Semantic Parsing:** The backend proxies requests to `gemini-3.1-flash-lite` with specialized medical regulatory system prompts to produce an independent literal English translation (`aiTranslatedEnglish`).
3. **Discrepancy Detection:** Compares literal translation against submitted draft IFU across four critical dimensions:
   * **Contraindication Omissions:** Deletion of specific high-risk patient populations (e.g., pregnant women, pediatric cohorts under 12).
   * **Storage Temperature Loosening:** Altering original $2^\circ\text{C}-8^\circ\text{C}$ refrigerated mandates to $2^\circ\text{C}-10^\circ\text{C}$ room-temp storage.
   * **Cutoff Alterations:** Shifting IVD clinical assay cutoff thresholds (e.g., changing CRP inflammation thresholds from $5.0\text{mg/L}$ to $10.0\text{mg/L}$).
   * **Concordant:** Exact semantic alignment.

---

### 3.6 QMS Audit Trail Visualization & Immutable Document Control

To satisfy *ISO 13485:2016* and *TFDA Quality Management System (QSD) standards*, document lifecycle mutations must maintain forensic traceability (`QmsAuditTrailLog`).

#### Forensic Mutation Model
Every modification to regulatory files or review verdicts generates an immutable transaction log record tagged with:
* `timestamp`: ISO 8601 UTC timestamp with millisecond precision.
* `userRole`: Enforced role-based access hierarchy (`TFDA Assessor`, `QA Lead`, `Regulatory Manager`, `System Automated`).
* `actionType`: State transition action (`CREATE`, `REVISE`, `APPROVE`, `REVOKE`, `OVERRIDE`).
* `versionRef`: Cryptographic SHA-256 hash pointer linking the modification to the exact document tree snapshot.

---

### 3.7 Automated Deficiency Drafting Engine

When review panels conclude their examination, identified `Fail` and `Warn` items must be synthesized into formal government administrative correspondence.

#### Synthesis Rules
* **Statutory Framework:** Automatically wraps findings within official bureaucratic phrasing referencing *Medical Devices Act Article 25 (Dismissal of Non-Compliant Applications)* and *Article 32 (Prohibition of Unauthorized IFU Alterations)*.
* **Sticker Aggregation:** Interactively pulls real-time reviewer annotations (`Annotation[]`) from Tab 01 and injects them into Section Two, Paragraph 4 of the official government draft notice.
* **Export Modality:** Generates pure UTF-8 plain text sheets formatted strictly for direct copy-pasting into the Ministry of Health and Welfare (MOHW) electronic document dispatch network.

---

## 4. Specification of 3 Additional Wow AI Regulatory Features

To further elevate the analytical superiority of the TFDA-Copilot platform, this technical specification formally designs three brand-new, cutting-edge AI regulatory subsystems.

### 4.1 Wow Feature A: Real-Time Post-Market Surveillance (PMS) Adverse Event Graph

#### Regulatory Justification
Under *Medical Devices Act Article 48* and *Article 49*, sponsors must monitor post-market adverse events and report severe incidents within 15 days. Reviewers evaluating a pre-market De Novo or license renewal application must verify whether similar devices globally (e.g., US FDA MAUDE database or EU EUDAMED) exhibit unaddressed predicate failure clusters.

#### Algorithmic Architecture
1. **Data Ingestion Stream:** Simulates asynchronous REST ingestion of international adverse event registries, extracting Device Problem Codes (DPC) and Patient Problem Codes (PPC).
2. **Graph Construction:** Constructs an in-memory directed bipartite graph $G = (V, E)$ where nodes $V$ represent either specific hardware/software components (e.g., *Battery Power Controller*, *Optical Sensor*, *Firmware v1.2*) or clinical adverse outcomes (e.g., *Thermal Burn*, *False Negative Diagnosis*, *Cardiac Arrhythmia*). Edges $E$ represent incident frequency weights.
3. **AI Root-Cause Clustering:** The system applies Louvain community detection algorithms to identify hidden structural dependencies. When a user inspects an active submission, the AI highlights high-degree centrality failure nodes and generates a **Pharmacovigilance Alert Report**, warning TFDA assessors if the candidate device shares circuit topology with a historically recalled predicate.

---

### 4.2 Wow Feature B: Automated Predicate Substantial Equivalence (SE) Triangulation

#### Regulatory Justification
For Class II medical device registration via the 510(k) or TFDA Substantial Equivalence pathway, sponsors must complete **TFDA Form 3A (Comparison Table of Substantial Equivalence)**. Establishing SE requires demonstrating that the candidate device possesses identical Intended Use, Technological Characteristics, and Biological Biocompatibility to a legally marketed predicate device.

#### Technical Architecture
* **High-Dimensional Embeddings:** Technical sections of the candidate STED dossier and predicate technical summaries are converted into 768-dimensional dense vector embeddings.
* **Vector Cosine Similarity Matrix:** The engine computes cosine similarity scores across six standardized regulatory pillars:
  1. Primary Indications for Use & Target Patient Population.
  2. Operating Principles & Physical Energy Source (e.g., Piezoelectric vs. Electromagnetic).
  3. Software Architecture & Cybersecurity Assurance Level (SAL).
  4. Sterilization Modality (EtO vs. Gamma Irradiation) & Shelf-Life Validation.
  5. Patient Contact Material Biocompatibility (ISO 10993).
  6. Clinical Performance & Diagnostic Accuracy Sensitivity/Specificity.
* **Automated Rebuttal Synthesis:** If vector similarity on Pillar 2 (Technological Characteristics) falls below $\tau = 0.85$, the AI automatically drafts a formal **SE Scientific Justification Statement**, outlining why the technological divergence does not raise different questions of safety and efficacy.

---

### 4.3 Wow Feature C: AI Cyber-Defense & AST Redaction Engine v3.0

#### Regulatory Justification
Submitting complete technical STED dossiers to public government portals exposes manufacturers to catastrophic intellectual property theft (e.g., theft of proprietary monoclonal antibody chemical reagent formulas) and violates the *Personal Information Protection Act (個人資料保護法)* regarding clinical patient trial identifiers.

#### Algorithmic Implementation
The v3.0 Redaction Engine transitions from fragile Regular Expression string matching to **Abstract Syntax Tree (AST) & Named Entity Recognition (NER)** tokenization:
1. **Dossier Tokenization:** The text is parsed into a structured document tree identifying structural headers, tabular matrices, and prose blocks.
2. **Bi-Directional Contextual Masking:** An embedded Transformer model scans tokens for sensitive entity classes:
   * `CLINICAL_PATIENT_UUID`: Regex patterns matching `ID-\d{4}` or patient alphanumeric hospital codes.
   * `PROPRIETARY_BIOFORMULA`: Chemical compounds, primer sequences, antibody clone designations (e.g., `Mouse Monoclonal Anti-human CRP Clone #M881-A`).
   * `CONFIDENTIAL_FACILITY`: GPS coordinates, cleanroom room designations, and internal server IP addresses.
3. **Cryptographic Salt Replacement:** Identified tokens are replaced with standardized, scannable regulatory placeholder tags (e.g., `██████ [PROPRIETARY_ANTIBODY_MATRIX_01]`). The original plaintext mapping is encrypted using AES-256-GCM and stored exclusively within volatile client memory (`sessionStorage`), guaranteeing that exported public inspection PDFs contain zero leakable proprietary secrets.

---

## 5. LLM Integration & Prompt Engineering Architecture

The platform's cognitive capabilities are powered exclusively by server-side proxy routes interfacing with Google's GenAI SDK (`@google/genai`).

### 5.1 Model Selection Strategy
* **Default Tier (`gemini-3.1-flash-lite`):** Mandated for all real-time interactive tasks including cross-lingual IFU translation, diff classification, and audit log summarization. Engineered for extremely high token throughput and minimal container latency.
* **Pro Tier (`gemini-2.5-pro`):** Activated selectively for deep analytical tasks requiring multi-step clinical reasoning, such as evaluating SaMD bilirubin optical interference Monte Carlo simulations or generating complex Predicate SE Triangulation tables.

### 5.2 Server-Side Secure Proxy Pattern
To adhere strictly to platform security mandates, API keys are stored exclusively in backend environment variables (`process.env.GEMINI_API_KEY`). Client components issue standard asynchronous HTTP POST requests to backend endpoints:

```typescript
// Backend Route Handler Blueprint (Express Server runtime)
import { GoogleGenAI } from '@google/genai';

// Lazy initialization pattern to prevent container crash during build phase
let aiClient: GoogleGenAI | null = null;
function getAiClient(): GoogleGenAI {
  if (!aiClient) {
    const key = process.env.GEMINI_API_KEY;
    if (!key) throw new Error('FATAL: GEMINI_API_KEY environment variable missing.');
    aiClient = new GoogleGenAI({ apiKey: key });
  }
  return aiClient;
}

// System Regulatory Master Blueprint Prompt
const REGULATORY_MASTER_PROMPT = `
You are the TFDA Senior Regulatory AI Triage Assessor.
Your mandate is strictly governed by the Taiwan Medical Devices Act.
When analyzing technical dossiers:
1. Maintain absolute deterministic objectivity.
2. Flag any unauthorized relaxation of storage temperatures or clinical cutoffs as Critical Statutory Violations.
3. Always output responses conforming strictly to requested JSON Schema structures.
`;
```

---

## 6. Defect Prevention, Concurrency & Container Resilience Engineering

Deploying full-stack web applications within sandboxed Cloud Run container environments introduces rigorous infrastructure constraints. TFDA-Copilot incorporates comprehensive resilience patterns.

### 6.1 Network Ingress & Port Binding Enforcement
The container runtime operates behind an enterprise Nginx reverse-proxy ingress layer that strictly enforces external communication exclusively over **Port 3000**.
* **Hardcoded Binding:** The backend Express server (`server.ts`) strictly binds to `0.0.0.0:3000`. No dynamic `PORT` environment variable resolution is permitted.
* **Vite Middleware Integration:** During development (`NODE_ENV !== "production"`), Vite is attached as Express middleware (`middlewareMode: true`), ensuring API routes (`/api/*`) take absolute routing precedence before SPA static fallback routing (`app.get('*all', ...)`).

### 6.2 Container OOM & Concurrency Throttling
Regulatory document parsing creates massive temporary memory allocations.
1. **Memory Leak Prevention:** Client-side file uploads utilizing `URL.createObjectURL()` must explicitly invoke `URL.revokeObjectURL()` within component cleanup hooks (`useEffect`) to prevent browser heap memory leaks.
2. **Express Payload Throttling:** Backend body parsers strictly cap JSON and raw buffer payloads at `50mb` to prevent container Out-Of-Memory (OOM) cold restarts.
3. **Asynchronous Backoff:** Client API calls implement exponential backoff retry logic with jitter:

$$\text{Delay}_{\text{retry}} = \min\left(10000, 1000 \cdot 2^{\text{attempt}} + \text{random}(0, 500)\right) \text{ ms}$$

### 6.3 ESM vs. CommonJS Module Resolution
To ensure reliable production deployments without runtime TypeScript transpilation failures, the build pipeline compiles backend server logic into a self-contained CommonJS bundle:
* **Command:** `esbuild server.ts --bundle --platform=node --format=cjs --packages=external --sourcemap --outfile=dist/server.cjs`
* **Outcome:** Resolves all relative module imports at compile time, eliminating Node.js ES Module relative path resolution crashes (`ERR_MODULE_NOT_FOUND`) during container cold boots.

---

## 7. Verification & Quality Assurance Matrix

The codebase undergoes automated verification to enforce craftsmanship and functional integrity.

| Verification Tool | Execution Phase | Target Criteria | Pass Threshold |
| :--- | :--- | :--- | :--- |
| **`lint_applet`** | Incremental Turn Edits | ESLint / TypeScript strict type checking | Zero Fatal Errors / Zero Missing Named Imports |
| **`compile_applet`** | Deployment Preparation | Full Vite Client Build & Esbuild Server Transpilation | Clean Static Asset Generation in `/dist/` |
| **Color Contrast Audit** | Visual Styling Design | WCAG 2.1 AA Compliance across UI palettes | Minimum 4.5:1 ratio for normal body text |
| **Touch Target Audit** | Responsive Layout | Mobile viewport interactive accessibility | Minimum 44px height for touch controls |

---

## 8. 20 Comprehensive Engineering & Regulatory Follow-Up Questions

To thoroughly evaluate candidate engineering competencies and regulatory science mastery, the following 20 advanced interview questions are formally appended to this specification:

### Category A: Regulatory Science & Cross-Dossier Alignment
1. **[MDEA Spatial Jurisprudence]** When comparing manufacturing addresses across US FDA CFS certificates and TFDA QSD approval letters, how does the MDEA Resolution Engine legally differentiate between an administrative abbreviation reduction (`Suite` vs. `STE`) and a physical spatial unit mismatch (`Suite A` vs. `Room A, 2nd Floor`) under TFDA Registration Guidelines Article 11?
2. **[Cross-Lingual IFU Integrity]** In the Cross-Lingual Harmonization module, if an imported Japanese PMDA IVD reagent IFU specifies storage at $2^\circ\text{C}-8^\circ\text{C}$ but the submitted Taiwan Chinese draft IFU expands this to $2^\circ\text{C}-10^\circ\text{C}$, what specific statutory clauses of the Taiwan Medical Devices Act are violated, and how does the AI quantify the clinical shelf-life degradation risk?
3. **[Predicate SE Triangulation]** When utilizing Wow Feature B to construct a Substantial Equivalence Form 3A comparison table, how should the vector cosine similarity engine weigh technological divergences in software operating algorithms versus divergences in patient contact biocompatibility materials?
4. **[ISO 14971 ALARP Boundaries]** In the 3D Risk Management Matrix, if a software hazard is calculated to have Severity = 5 (Catastrophic) and Detectability = 5 (Undetectable), why does the ALARP principle prohibit approving the device even if Probability = 1 (Improbable), and what architectural software interlocks must be mandated?

### Category B: SaMD Clinical Verification & Monte Carlo Simulations
5. **[CLSI EP07-A3 Optical Interference]** In the SaMD Defense Sandbox, when simulating CRP optical immunoassay measurements in serum matrices with bilirubin concentrations exceeding $20\text{mg/L}$, explain the mathematical basis of the generated $-18.4\%$ negative clinical bias and its catastrophic potential for false-negative sepsis diagnoses.
6. **[Monte Carlo Sample Size Sizing]** How does adjusting the synthetic patient cohort size ($n=50$ vs. $n=500$) impact the statistical confidence intervals of the AI-simulated assay bias, and why must regulatory reviewers demand real clinical validation data alongside synthetic simulations?
7. **[AI Model Grounding & Hallucination]** When proxying regulatory review queries to `gemini-3.1-flash-lite`, what specific prompt engineering interlocks and grounding schemas are implemented to prevent the LLM from hallucinating non-existent TFDA statutory guidance articles?
8. **[Deterministic LLM Triage]** Why does the technical specification mandate an LLM sampling temperature of $\text{temp}=0.1$ for automated deficiency drafting, and under what specific exploratory review scenarios would a reviewer legitimately override this to $\text{temp}=0.7$?

### Category C: High-Concurrency System Architecture & Memory Management
9. **[Container Ingress Port Isolation]** Explain the architectural necessity of hardcoding the Express server binding to `0.0.0.0:3000` behind Nginx reverse-proxy ingress, and why dynamically reading runtime `PORT` environment variables is strictly forbidden in this sandboxed topology.
10. **[Vite Middleware SPA Fallback]** In `server.ts`, why must API route handlers (`app.get('/api/*', ...)`) be registered strictly prior to mounting Vite development middleware or production static asset serving (`app.get('*all', ...)`), and what routing race conditions occur if this sequence is inverted?
11. **[Esbuild CJS Transpilation Invariants]** How does bundling the server entry point via esbuild into CommonJS (`dist/server.cjs`) eliminate Node.js runtime ES Module relative import path traps (`__dirname`, `__filename`, and mandatory `.js` file extensions) during production Cloud Run cold-starts?
12. **[DOM Virtualization Memory Ceilings]** When rendering a 15,000-line clinical trial data difference table in Tab 02, explain how windowed slice rendering (`react-window` paradigm) prevents browser main-thread blocking and JavaScript heap out-of-memory crashes.

### Category D: Cybersecurity, AST Redaction & Data Privacy
13. **[AST vs. Regex Redaction Security]** Why is Regular Expression string replacement considered structurally vulnerable for masking proprietary chemical formulas in STED dossiers, and how does Abstract Syntax Tree (AST) tokenization in Wow Feature C guarantee zero data leakage?
14. **[Volatile Cryptographic Salt Isolation]** In the AST Redaction Engine, why must the plaintext-to-REDACTED mapping dictionaries be stored strictly within volatile `sessionStorage` rather than persistent `localStorage` or backend Firestore collections?
15. **[Zero Client-Side Secret Exposure]** Explain the full-stack security attack vector that occurs if a developer inadvertently prefixes the Gemini API key with `VITE_GEMINI_API_KEY`, and how the server-side `/api/gemini/*` proxy routes defend against browser DevTools scraping.
16. **[Role-Based Forensic Lineage]** In the QMS Audit Trail Visualizer, how does linking each document revision log to a cryptographic SHA-256 snapshot hash satisfy ISO 13485 requirements for immutable document control and non-repudiation?

### Category E: Production DevOps & Failure Recovery
17. **[Payload Throttling & Container OOM]** What container failure modes occur when concurrent multiple assessors upload $100\text{MB}$ uncompressed PDF STED dossiers simultaneously, and how does Express body-parser size capping mitigate container eviction?
18. **[Asynchronous Backoff Circuit Breakers]** When the upstream Google GenAI API experiences transient HTTP 429 (Too Many Requests) rate-limiting, explain how exponential backoff with random jitter maintains UI responsiveness without triggering cascading client polling loops.
19. **[WCAG 2.1 AA Visual Hierarchy]** How does the intentional pairing of natural cream backgrounds (`#FDFBF7`) with deep charcoal slate text (`#2F3E46`) and olive accents (`#5A5A40`) reduce cognitive eye fatigue for regulatory reviewers examining dossiers for 8 hours daily compared to pure brutalist `#000000` on `#FFFFFF`?
20. **[Golden Sandbox State Restoration]** In the event of a catastrophic client state corruption during complex multi-tab FMEA matrix editing, explain how the unidirectional React state engine re-hydrates baseline golden master data without requiring a full browser window reload or backend server reboot.

---
*End of Technical Specification Document.*
