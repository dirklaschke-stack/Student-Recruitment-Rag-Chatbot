# RAG Recruitment Agent (Campus Recruitment Hub)

An end-to-end **Retrieval-Augmented Generation (RAG)** conversational assistant built in **n8n** to automate prospective student advisory in **German and English**.

This repository documents the architecture, design decisions, and implementation steps behind a domain-specific AI recruitment assistant—combining **20+ years of higher-education student recruitment and admissions expertise** with modern **workflow automation, vector search, and LLM orchestration**.

![Workflow Overview](assets/01_workflow_overview.png)

---

## Business Problem & Use Case

In university student recruitment and admissions, prospective students ask recurring, detail-heavy questions across multiple programs, admission requirements, timelines, and languages—often outside standard office hours.

* **The Challenge:** Generic chatbots hallucinate program specifics, while static FAQ pages create friction and drop-offs in the applicant funnel.
* **The Goal:** Build a conversational recruitment assistant that answers prospective student inquiries accurately in **German or English**, routes queries to verified program knowledge, and maintains context across multi-turn follow-up questions.

---

## Solution Architecture

The assistant is implemented as a modular **n8n AI Agent** workflow integrating hosted chat delivery, LLM reasoning, session memory, and vector-based knowledge retrieval:

| Layer | Technology / Node | Purpose |
| --- | --- | --- |
| **User Interface** | `When chat message received` (n8n Hosted Chat)[cite: 1, 2] | Publicly accessible web chat interface with custom onboarding greeting[cite: 1]. |
| **LLM Reasoning** | `OpenRouter Chat Model` | Flexible model routing for bilingual (DE/EN) responses, tone control, and instruction following. |
| **Conversation State** | `Simple Memory` | Retains multi-turn dialogue context so applicants can ask natural follow-up questions[cite: 2]. |
| **Knowledge Retrieval** | `Pinecone Vector Store` + Embeddings[cite: 2] | Semantic search (`search_*` retrieval tool) over curated program documentation to ground answers in verified facts[cite: 2]. |

---

## Key Implementation Steps

### Step 1: Conversational Entry Point & User Onboarding
* Configured n8n's **Chat Trigger** in *Hosted Chat* mode to provide an immediate, zero-frontend-overhead web interface for prospective students[cite: 1].
* Designed a clear initial welcome prompt setting expectations on supported programs and available information[cite: 1].
* Validated authentication and public access settings (`Authentication: None` in Fixed mode) for frictionless testing and stakeholder demos[cite: 1].

### Step 2: Knowledge Grounding via Pinecone Vector Search
* Structured program information into retrieval-ready knowledge chunks and indexed them in **Pinecone**.
* Connected the Pinecone retrieval tool (`search_*`) and embedding model directly to the AI Agent so every program-specific inquiry triggers semantic retrieval rather than relying on parametric model memory[cite: 2].

### Step 3: Bilingual Prompting, Memory & Guardrails
* Connected **Simple Memory** to handle multi-turn conversations (e.g., asking about admission criteria first and tuition or deadlines next)[cite: 2].
* Configured system instructions and guardrails to automatically detect and respond in the user's language (**German or English**)[cite: 2] while staying strictly grounded in retrieved program facts.

---

## Demo & Visual Walkthrough

### Live Chat Interaction (German / English)
*Screenshot showing a multi-turn conversation answering program-specific questions:*

![Chat Interface Demo](assets/02_chat_interface_demo.png)

### Agent & Retrieval Configuration
*Screenshot showing the tool calling, memory wiring, and retrieval execution inside n8n:*

![Pinecone Retrieval Setup](assets/03_pinecone_retrieval.png)

---

## Key Takeaways & Skills Demonstrated

* **Process & Funnel Translation:** Translating real-world admissions and student recruitment workflows into an automated self-service touchpoint.
* **RAG & AI Agent Engineering:** Connecting LLMs (via OpenRouter), vector databases (Pinecone), embeddings, and conversational memory in **n8n**[cite: 2].
* **Pragmatic Prototyping & Debugging:** Rapidly building, testing, and troubleshooting production-ready AI workflows—from webhook/chat trigger configuration to retrieval grounding[cite: 1, 2].

---

## Next Iterations (Roadmap)

* **Metadata Filtering & Program Routing:** Expanding multi-index / metadata-filtered retrieval for distinct degree levels (BSc, MSc, MBA)[cite: 2].
* **CRM & Lead Capture Handoff:** Connecting qualified prospective student inquiries directly into CRM workflows (e.g., HubSpot) for personalized follow-up by admissions advisors.
* **Conversation Analytics:** Logging anonymized topic clusters and unanswered questions to continuously improve recruitment content.