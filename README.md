# Flame in Freefall

## AI-Powered Fire Safety Insights from Microgravity Combustion Data

Team project for the **NASA Space Apps Challenge 2026**.

Challenge: **Flame in Freefall: AI-Powered Fire Safety Insights from Microgravity Combustion Data**

---

# 1. Project Goal

NASA has accumulated decades of experiments studying flames and fire behavior in microgravity. The challenge is that the amount of scientific material is large and difficult to search, compare, and understand.

Our goal is to build an **interactive AI-powered research dashboard** that turns NASA's combustion research into an accessible knowledge system.

The system must allow users to:

1. Search NASA combustion experiments using natural language.
2. Find and rank the most relevant experiments and findings.
3. View structured experiment metadata and results.
4. Generate concise AI summaries grounded in NASA sources.
5. Compare multiple experiments.
6. Identify recurring patterns across experiments.
7. Generate evidence-backed fire-safety insights for human space exploration.
8. Always show the NASA source/evidence behind an AI-generated statement.

The project is **not** intended to prove new physics or automatically claim that a fire is safe or unsafe.

The system is a research and decision-support interface over NASA experimental knowledge.

---

# 2. Core Product

Working name:

**FlameLens**

Alternative names can be changed later.

The final application should contain:

### A. Overview Dashboard

Display:

* number of indexed experiments
* number of findings
* number of documents
* fuel types
* experimental conditions
* gravity conditions
* major research areas

Provide quick access to search and exploration.

---

### B. AI Research Search

User can enter a natural-language question.

Example:

> Which experiments studied flame extinction under reduced oxygen conditions?

or:

> Show experiments involving liquid fuel droplets and flame extinction.

The system should:

1. Parse the query.
2. Retrieve relevant documents/records.
3. Rank the results.
4. Display the top relevant experiments.
5. Explain why each result is relevant.
6. Show source references.

Search should use **semantic retrieval**, not only keyword matching.

Preferred architecture:

```text
User Query
    ↓
Embedding
    ↓
Vector Search
    +
Metadata / Keyword Filters
    ↓
Ranking
    ↓
Top Results
```

---

### C. Experiment Explorer

Users must be able to browse experiments through filters.

Initial filters:

* Investigation
* Fuel
* Fuel type
* Oxygen concentration
* Pressure
* Flow
* Gravity condition
* Experiment type
* Flame behavior
* Fire-safety topic

Example:

```text
Investigation: FLEX
Fuel: Methanol
Gravity: Microgravity
Phenomenon: Flame Extinction
```

---

### D. Experiment Details

Each experiment page should contain:

* investigation name
* NASA PSI identifier
* objective
* experiment description
* platform
* dates
* fuel
* environmental conditions
* measured variables
* important findings
* related publications
* related experiments
* AI-generated summary
* source files
* NASA source citation/reference

Example:

```text
FLEX

Flame Extinguishment Experiment

Focus:
Droplet combustion and extinction in microgravity

Fuel:
Methanol / other tested fuels

Conditions:
Oxygen
Pressure
Flow
Suppressants

Key findings:
...

Sources:
NASA PSI-69
...
```

---

# 3. AI Features

The project should contain several AI components rather than a generic chatbot.

## 3.1 Semantic Search

Use embeddings to represent:

* experiment descriptions
* findings
* publications
* extracted scientific text
* structured experiment records

Use a vector database.

Recommended options:

* Qdrant
* pgvector

Pick one and keep the implementation simple.

---

## 3.2 Ranking

Search results must be ranked.

Ranking should combine:

```text
semantic similarity
+
keyword match
+
metadata match
+
experiment relevance
```

The ranking system must be deterministic and explainable.

Example:

```text
FLEX experiment #17
Relevance: 0.91

Why:
- same flame-extinction topic
- matching oxygen condition
- matching liquid-fuel category
```

Do not invent scientific importance scores.

The ranking score should represent **retrieval relevance**, not scientific truth or safety level.

---

## 3.3 AI Summarization

The user can request:

> Summarize this experiment.

The LLM should generate:

* objective
* setup
* conditions
* observations
* findings
* relevance to fire safety

Every important statement must be traceable to retrieved NASA content.

Use a **RAG architecture**:

```text
User
 ↓
Retrieve NASA evidence
 ↓
Relevant chunks
 ↓
LLM
 ↓
Answer + Sources
```

The LLM must not answer from unsupported knowledge when the answer concerns a NASA experiment.

---

## 3.4 Experiment Comparison

User can select two or more experiments.

Example:

```text
Compare FLEX and BASS-II
```

Show:

| Parameter             | FLEX             | BASS-II                              |
| --------------------- | ---------------- | ------------------------------------ |
| Fuel type             | Liquid           | Solid                                |
| Main phenomenon       | Flame extinction | Ignition / flame spread / extinction |
| Gravity               | Microgravity     | Microgravity                         |
| Oxygen                | Available        | Available                            |
| Flow                  | Available        | Available                            |
| Fire-safety relevance | High             | High                                 |

Comparison must be based on structured metadata and source documents.

---

## 3.5 Fire Safety Insights

This is one of the most important features.

The system should identify patterns across experiments.

Example:

```text
Potential Fire Safety Insight

Across the selected experiments, oxygen concentration
appears repeatedly as an important experimental variable
for flame behavior.

Evidence:
- FLEX
- BASS-II

Source:
NASA PSI records and associated publications
```

Important:

AI-generated insights must always contain evidence.

Do not present speculative conclusions as scientific facts.

Use language such as:

* "The selected experiments indicate..."
* "The data shows..."
* "A recurring pattern is..."
* "This may be relevant to..."
* "Potential safety implication..."

Avoid:

* "NASA proved..."
* "This guarantees..."
* "This fire is safe..."
* "This means astronauts will..."

---

# 4. Data Strategy

## IMPORTANT: Do NOT download the entire NASA PSI repository.

NASA PSI contains very large datasets including raw videos, engineering files, images, and other material.

For the MVP we will use a **curated subset**.

Do not place multi-gigabyte raw datasets into Git.

Do not store raw videos in the repository.

Do not commit large binary files.

---

# 5. Primary Dataset

## FLEX — Flame Extinguishment Experiment

NASA PSI identifier:

**PSI-69**

FLEX is the primary dataset for the first version of the application.

Why FLEX:

* directly related to spacecraft fire safety
* microgravity combustion
* liquid fuel droplets
* flame ignition and extinction
* oxygen conditions
* pressure
* flow
* fire suppressant experiments
* many publications and scientific results
* structured metadata
* NASA PSI source
* CC0-1.0 dataset license

The first MVP should be built around FLEX.

Do not attempt to process every FLEX file.

---

# 6. Secondary Dataset

## BASS-II — Burning and Suppression of Solids-II

NASA PSI identifier:

**PSI-25**

BASS-II extends the project from liquid droplets to solid fuels.

Relevant topics include:

* ignition
* flame growth
* flame spread
* extinction
* oxygen conditions
* forced flow
* solid fuel behavior
* fire detection and suppression

BASS-II should be added after the FLEX pipeline is working.

---

# 7. Optional Dataset

Additional combustion investigations can be added later.

Potential candidates:

* SPICE
* SAFFIRE
* other NASA PSI combustion investigations

They should not block the MVP.

Priority:

```text
1. FLEX
2. BASS-II
3. SPICE
4. SAFFIRE
5. Other investigations
```

The first working system only needs:

```text
FLEX
```

A second dataset should be added once the ingestion/search/RAG pipeline works.

---

# 8. What Data Should Be Downloaded?

Prioritize:

### MUST HAVE

* investigation metadata
* experimental metadata
* processed/analyzed data
* structured tables
* science documents
* reports
* relevant publications metadata
* experiment descriptions
* findings
* representative images when useful

### DO NOT DOWNLOAD FOR MVP

* raw video archives
* huge image archives
* engineering drawings
* unrelated engineering documentation
* duplicate files
* raw binary telemetry
* files that are not useful for search/analysis

---

# 9. Data Pipeline

The data pipeline must be reproducible.

```text
NASA PSI
   ↓
Metadata / documents / selected processed data
   ↓
Raw staging
   ↓
Extraction
   ↓
Cleaning
   ↓
Normalization
   ↓
Structured experiment records
   ↓
Document chunks
   ↓
Embeddings
   ↓
Vector database
   ↓
Search / Ranking / RAG
```

---

# 10. Data Storage

The repository should NOT contain large raw NASA files.

Use:

```text
data/
    raw/
    processed/
    manifests/
```

`raw/` should contain only small files required for development.

Large source files should remain outside Git.

The repository must contain:

```text
data/manifests/
    flex_manifest.json
    bass_manifest.json
```

The manifest should describe:

* NASA PSI identifier
* file name
* file category
* source
* version
* local path
* file size
* whether the file is used
* extraction status

Example:

```json
{
  "investigation": "FLEX",
  "psi_id": "PSI-69",
  "file": "example.pdf",
  "category": "science_document",
  "used": true,
  "processed": true
}
```

---

# 11. Canonical Internal Data Model

Every experiment should eventually be normalized into a common schema.

Example:

```json
{
  "experiment_id": "FLEX-001",
  "investigation": "FLEX",
  "psi_id": "PSI-69",
  "title": "...",
  "objective": "...",
  "gravity_condition": "microgravity",
  "fuel": "...",
  "fuel_type": "liquid",
  "oxygen_concentration": null,
  "pressure": null,
  "flow": null,
  "suppressant": null,
  "phenomena": [
    "ignition",
    "flame extinction"
  ],
  "findings": [
    "...",
    "..."
  ],
  "source_documents": [],
  "source_publications": []
}
```

The schema must be extensible.

Do not make FLEX-specific fields impossible for other investigations to support.

---

# 12. Document Model

Documents indexed for RAG should contain:

```json
{
  "document_id": "...",
  "investigation": "FLEX",
  "psi_id": "PSI-69",
  "title": "...",
  "section": "...",
  "text": "...",
  "source_file": "...",
  "page": null,
  "metadata": {}
}
```

For PDF documents, keep page numbers whenever possible.

This allows the UI to show evidence such as:

```text
Source: FLEX report
Page: 23
```

---

# 13. Frontend Requirements

Frontend stack:

**React + TypeScript**

Preferred:

* Vite
* React
* TypeScript
* Tailwind CSS
* Recharts
* TanStack Query

The UI should be modern and scientific.

Avoid making it look like a generic chatbot.

---

## Main pages

### `/`

Overview dashboard

### `/experiments`

Experiment explorer

### `/experiments/:id`

Experiment details

### `/compare`

Experiment comparison

### `/search`

AI research search

### `/insights`

Fire safety insights

---

# 14. Backend Requirements

Backend stack:

**Python + FastAPI**

Responsibilities:

* API
* experiment retrieval
* search
* ranking
* filtering
* comparison
* AI orchestration
* database access

Suggested endpoints:

```text
GET  /api/experiments
GET  /api/experiments/{id}

POST /api/search

POST /api/compare

POST /api/summarize

POST /api/insights

GET  /api/filters

GET  /api/health
```

---

# 15. AI Service Architecture

Recommended:

```text
backend/
    services/
        retrieval_service.py
        ranking_service.py
        rag_service.py
        summary_service.py
        comparison_service.py
        insight_service.py
```

Each service should have one responsibility.

Do not put all AI functionality inside one huge file.

---

# 16. Retrieval Architecture

Recommended MVP:

```text
Query
 ↓
Query embedding
 ↓
Vector search
 ↓
Top 20 candidates
 ↓
Metadata filtering
 ↓
Reranking
 ↓
Top 5 results
 ↓
LLM context
```

The system should preserve source IDs throughout the pipeline.

Never lose provenance.

---

# 17. Citation / Evidence Requirement

Every AI answer should contain evidence.

Example:

```text
Answer

The experiment investigated flame extinction
under reduced oxygen conditions.

Evidence

1. FLEX experiment report — page 14
2. FLEX analyzed dataset
3. NASA PSI-69 metadata
```

The user must be able to click/open the source record.

---

# 18. Visualization Requirements

The dashboard should use charts where they improve understanding.

Potential charts:

* oxygen concentration vs flame behavior
* pressure vs extinction condition
* fuel types
* experiment counts
* gravity condition distribution
* flame behavior categories
* experiment comparison

Charts must use actual data.

Never invent scientific measurements for visualization.

---

# 19. Repository Structure

Use the following structure:

```text
flame-in-freefall/
│
├── README.md
├── LICENSE
├── .gitignore
├── .env.example
├── docker-compose.yml
│
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── layouts/
│   │   ├── charts/
│   │   ├── api/
│   │   ├── hooks/
│   │   ├── types/
│   │   └── utils/
│   ├── package.json
│   └── tsconfig.json
│
├── backend/
│   ├── app/
│   │   ├── main.py
│   │   │
│   │   ├── api/
│   │   │   ├── search.py
│   │   │   ├── experiments.py
│   │   │   ├── compare.py
│   │   │   ├── summarize.py
│   │   │   └── insights.py
│   │   │
│   │   ├── services/
│   │   │   ├── retrieval_service.py
│   │   │   ├── ranking_service.py
│   │   │   ├── rag_service.py
│   │   │   ├── summary_service.py
│   │   │   ├── comparison_service.py
│   │   │   └── insight_service.py
│   │   │
│   │   ├── models/
│   │   ├── schemas/
│   │   ├── db/
│   │   └── core/
│   │
│   └── requirements.txt
│
├── ai/
│   ├── embeddings/
│   ├── retrieval/
│   ├── ranking/
│   ├── prompts/
│   └── evaluation/
│
├── data/
│   ├── raw/
│   ├── processed/
│   ├── manifests/
│   └── samples/
│
├── scripts/
│   ├── ingest_flex.py
│   ├── ingest_bass.py
│   ├── extract_documents.py
│   ├── process_data.py
│   ├── build_embeddings.py
│   └── build_index.py
│
├── notebooks/
│   └── exploration.ipynb
│
└── docs/
    ├── architecture.md
    ├── data-model.md
    ├── data-sources.md
    └── api.md
```

---

# 20. Team Structure

There are four people.

Each member owns one major area.

Everyone works in separate branches.

---

# 21. Team Member 1 — Data Engineer

Branch:

```text
feature/data-pipeline
```

Responsibilities:

### NASA data

* investigate NASA PSI
* select usable FLEX files
* create FLEX manifest
* download only required files
* determine file categories
* extract useful data
* clean data
* normalize fields
* create canonical experiment JSON
* prepare documents for RAG
* add source metadata

### Deliverables

```text
data/manifests/flex_manifest.json

data/processed/experiments.json

data/processed/findings.json

data/processed/documents.jsonl

scripts/ingest_flex.py

scripts/extract_documents.py

scripts/process_data.py
```

### Definition of Done

The AI and backend developers must be able to run:

```text
python scripts/ingest_flex.py
```

and obtain reproducible processed data.

Data must include source/provenance information.

---

# 22. Team Member 2 — AI / ML Engineer

Branch:

```text
feature/ai-rag
```

Responsibilities:

* embeddings
* vector database
* semantic search
* retrieval
* reranking
* RAG
* AI summaries
* experiment comparison logic
* insight generation
* evaluation

### Deliverables

```text
ai/embeddings/

ai/retrieval/

ai/ranking/

ai/prompts/

ai/evaluation/

backend/app/services/retrieval_service.py

backend/app/services/ranking_service.py

backend/app/services/rag_service.py

backend/app/services/summary_service.py

backend/app/services/comparison_service.py

backend/app/services/insight_service.py
```

### AI pipeline

```text
query
 ↓
embedding
 ↓
vector search
 ↓
reranking
 ↓
retrieved evidence
 ↓
LLM
 ↓
answer
 ↓
citations
```

### Definition of Done

The following must work:

```text
"Which experiments studied flame extinction?"
```

and return:

```text
Top experiments
+
relevance scores
+
reason
+
evidence
```

RAG must not operate without source context.

---

# 23. Team Member 3 — Backend Engineer

Branch:

```text
feature/backend
```

Responsibilities:

* FastAPI
* PostgreSQL/vector DB integration
* API contracts
* experiment endpoints
* search endpoint
* comparison endpoint
* AI endpoint integration
* filtering
* validation
* error handling
* configuration
* Docker integration

### Deliverables

```text
backend/app/main.py

backend/app/api/

backend/app/models/

backend/app/schemas/

backend/app/db/

backend/app/core/
```

### API

Implement:

```text
GET  /api/experiments
GET  /api/experiments/{id}

POST /api/search

POST /api/compare

POST /api/summarize

POST /api/insights

GET /api/filters

GET /api/health
```

### Definition of Done

Frontend can use the backend without directly accessing the database or AI services.

---

# 24. Team Member 4 — Frontend Engineer

Branch:

```text
feature/frontend
```

Responsibilities:

* React dashboard
* UI/UX
* experiment browser
* search
* filters
* charts
* comparison interface
* AI response interface
* source/evidence display
* responsive design

### Deliverables

Main screens:

```text
Dashboard

Experiment Explorer

Experiment Details

AI Search

Compare

Insights
```

### Definition of Done

A user can:

```text
open dashboard
 ↓
search question
 ↓
see ranked experiments
 ↓
open experiment
 ↓
view AI summary
 ↓
view evidence
 ↓
compare experiments
 ↓
see fire safety insight
```

---

# 25. Git Branch Strategy

Main branches:

```text
main
develop
```

Feature branches:

```text
feature/data-pipeline
feature/ai-rag
feature/backend
feature/frontend
```

Bug branches:

```text
fix/...
```

Documentation:

```text
docs/...
```

Rules:

1. Never commit directly to `main`.
2. Feature work happens in feature branches.
3. Pull requests merge into `develop`.
4. `main` contains stable versions.
5. Keep commits small and meaningful.
6. Never commit `.env`.
7. Never commit large NASA raw files.
8. Never commit API keys.

---

# 26. Environment Variables

Use:

```text
.env
```

Never commit it.

Create:

```text
.env.example
```

Possible variables:

```text
LLM_API_KEY=
EMBEDDING_API_KEY=
DATABASE_URL=
VECTOR_DB_URL=
VECTOR_DB_API_KEY=
```

The exact provider can be selected later.

---

# 27. MVP Scope

The MVP must prioritize functionality over the amount of data.

Minimum viable system:

```text
FLEX dataset
     ↓
processed experiments
     ↓
document index
     ↓
semantic search
     ↓
ranking
     ↓
AI summary
     ↓
comparison
     ↓
evidence-backed insight
     ↓
interactive dashboard
```

A smaller clean dataset with strong retrieval and clear provenance is preferable to downloading hundreds of gigabytes of raw data.

---

# 28. Phase 1 — Repository Setup

Create:

```text
frontend/
backend/
ai/
data/
scripts/
docs/
```

Set up:

* Git
* branches
* Python environment
* Node environment
* Docker
* `.env.example`
* linting
* formatting
* README
* basic CI if time allows

---

# 29. Phase 2 — FLEX Data

Start ONLY with FLEX.

Tasks:

1. Download metadata.
2. Inspect files.
3. Select useful scientific documents.
4. Select processed/analyzed data.
5. Ignore large raw videos.
6. Normalize experiment information.
7. Create JSON/JSONL outputs.
8. Build the data manifest.

Target:

```text
clean structured dataset
+
searchable scientific documents
```

---

# 30. Phase 3 — AI Retrieval

Implement:

```text
document chunking
embedding
vector indexing
semantic search
reranking
```

Test using at least 10–20 manually written research questions.

Example questions:

```text
Which experiments studied flame extinction?

Which experiments used liquid fuel droplets?

How does oxygen concentration affect flame behavior?

Which experiments studied fire suppression?

Which experiments used microgravity conditions?

Show experiments related to flame spread.

Compare experiments studying extinction.
```

---

# 31. Phase 4 — Backend

Connect:

```text
Frontend
    ↓
FastAPI
    ↓
AI services
    ↓
Database / Vector DB
```

Implement search, filtering and experiment retrieval.

---

# 32. Phase 5 — Frontend

Build the dashboard.

Priority order:

```text
1. Search
2. Experiment results
3. Experiment details
4. AI summary
5. Compare
6. Insights
7. Charts
```

---

# 33. Phase 6 — BASS-II

Only after FLEX works.

Add:

```text
BASS-II
```

Reuse the same canonical schema and ingestion architecture.

Do NOT create a separate AI system for every investigation.

The system must support multiple investigations through a common data model.

---

# 34. Phase 7 — Evaluation

Create a small evaluation set.

Example:

```text
Question
Expected relevant experiments
Expected evidence
```

Test:

* retrieval relevance
* ranking
* citation correctness
* hallucination rate
* summary quality

At minimum, manually evaluate:

```text
20 search questions
10 summaries
10 comparisons
10 insights
```

Keep evaluation results in:

```text
ai/evaluation/
```

---

# 35. Hallucination Protection

The system must follow these rules:

### Rule 1

No evidence → no scientific claim.

### Rule 2

AI must only use retrieved NASA evidence for experiment-specific answers.

### Rule 3

Every generated insight must include sources.

### Rule 4

Do not invent missing measurements.

### Rule 5

Do not replace missing data with guesses.

### Rule 6

Clearly distinguish:

```text
NASA finding
AI interpretation
Potential implication
```

---

# 36. Demo Scenario

The final demo should show one complete user journey.

Example:

### Step 1

User asks:

> How does oxygen concentration affect flame extinction in microgravity?

### Step 2

System returns:

```text
Relevant experiments
1. FLEX ...
2. ...
```

### Step 3

User opens FLEX.

Dashboard shows:

```text
conditions
measurements
findings
publication
```

### Step 4

User clicks:

```text
AI Summary
```

### Step 5

User selects two experiments.

```text
Compare
```

### Step 6

Dashboard displays:

```text
Key differences
Key similarities
Relevant conditions
```

### Step 7

User opens:

```text
Fire Safety Insight
```

The system generates an evidence-backed interpretation and shows the supporting NASA sources.

---

# 37. Non-Goals

Do NOT spend time on:

* training a giant neural network from scratch
* downloading all NASA raw data
* processing every available video
* building a generic ChatGPT clone
* claiming to predict actual spacecraft fires
* inventing scientific conclusions
* unnecessary microservices
* complex Kubernetes infrastructure
* over-engineered authentication
* mobile application

Focus on the challenge.

---

# 38. Final Architecture

```text
                   NASA PSI
                      │
                      ▼
               Selected Data
                      │
                      ▼
              Data Processing
                      │
          ┌───────────┴───────────┐
          ▼                       ▼
    Structured Data          Documents
          │                       │
          │                       ▼
          │                  Chunking
          │                       │
          │                       ▼
          │                  Embeddings
          │                       │
          │                       ▼
          │                  Vector DB
          │                       │
          └───────────┬───────────┘
                      ▼
                AI Retrieval
                      │
                  Ranking
                      │
            ┌─────────┼─────────┐
            ▼         ▼         ▼
         Search    Summary   Comparison
            │         │         │
            └─────────┼─────────┘
                      ▼
                AI Insights
                      │
                      ▼
                  FastAPI
                      │
                      ▼
              React Dashboard
```

---

# 39. Product Principle

The product should always follow:

```text
Find
  ↓
Rank
  ↓
Understand
  ↓
Compare
  ↓
Interpret
  ↓
Act
```

The application should make NASA combustion research significantly easier to discover and understand.

---

# 40. Definition of Done

The project is considered complete when a user can:

1. Open the dashboard.
2. Search NASA combustion knowledge using natural language.
3. Receive ranked relevant experiments.
4. Open an experiment.
5. Read an AI-generated summary.
6. See the underlying evidence.
7. Compare experiments.
8. Explore charts and metadata.
9. Receive a fire-safety insight.
10. Trace the insight back to NASA source material.

The final result must be a **working interactive AI-powered dashboard**, not only a research report or static visualization.

---

# 41. First Development Task

Before adding advanced functionality, create the repository structure and make the following work independently:

```text
DATA ENGINEER
→ FLEX ingestion + normalized JSON

AI ENGINEER
→ semantic search prototype

BACKEND ENGINEER
→ FastAPI skeleton + API contracts

FRONTEND ENGINEER
→ dashboard UI skeleton
```

After these four parts are ready, integrate them.

The first integration target is:

```text
React
  ↓
POST /api/search
  ↓
AI retrieval
  ↓
FLEX data
  ↓
ranked results
  ↓
React UI
```

Then add:

```text
experiment details
→ summary
→ comparison
→ insights
→ evidence
```

---

# 42. Source Strategy

Primary scientific source:

**NASA Physical Sciences Informatics (PSI)**

Primary investigation:

**FLEX — PSI-69**

Secondary investigation:

**BASS-II — PSI-25**

All scientific claims in the product must retain NASA source provenance.

NASA data usage and source information must also be documented in the final project submission.
