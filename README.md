# Student Recruitment RAG Chatbot (Campus Recruitment Hub)

An **n8n**-based **AI chat assistant** that answers prospective student questions in **German and English** using official study program PDFs stored in **Pinecone**.

Instead of giving generic AI answers or forcing prospective students to dig through static web pages, this project connects an AI chat agent to program-specific documents. It brings together **20+ years of experience in higher-education student recruitment and admissions** with hands-on **workflow automation, vector search, and AI tooling**.

![Full Workflow Overview](assets/01_full_workflow_canvas.png)

---

## Why I Built This

Prospective students regularly ask specific questions about degree programs, tuition fees, admission requirements, deadlines, and curriculum structure—often outside regular office hours.

* **The Challenge:** General AI answers can be inaccurate or outdated, and searching across multiple program PDFs manually takes time.
* **The Solution:** A two-part automation in **n8n** that automatically keeps program PDFs up to date in a vector database (**Pinecone**) and lets an **AI Agent** answer questions in the user's language based on those documents.

---

## Repository Structure

```text
Student-Recruitment-RAG-Chatbot/
├── README.md                             # Project overview, step-by-step workflows & takeaways
└── assets/
    ├── 01_full_workflow_canvas.png       # Complete n8n canvas showing both workflows
    ├── 02_ingestion_flow.png             # Step-by-step PDF sync & Pinecone update flow
    └── 03_rag_agent_chat_demo.png        # Example chat response in German or English

```
---

## Step-by-Step Walkthrough: How Both Workflows Work

### Part 1: Automatic PDF Sync & Vector Database Update (Campus Recruitment Hub)

This workflow keeps the knowledge base current whenever new or updated program PDFs are added to Google Drive[cite: 3]:

1. **Watch Google Drive Folder (`DriveTriggerFileCreated` & `DriveTriggerFileUpdated`):** Monitors a dedicated `campus-recruitment-hub` Google Drive folder for newly created or updated program PDFs[cite: 3].
2. **Download the PDF (`DownloadFile`):** Downloads the file from Google Drive into the workflow[cite: 3].
3. **Identify the Study Program (`RouteByProgram`):** Checks the file name to route documents for programs such as `BScBBA`, `MScGL`, `MScMGT`, `MScBADS`, and `MBA`[cite: 3].
4. **Set Namespace & Metadata (`SetNameSpaceProgramTags`):** Assigns a dedicated Pinecone namespace (`prog-bscbba`, `prog-mscgl`, `prog-mscmgt`, `prog-mscbads`, `prog-mba`) along with metadata (`program_code`, `degree_level`, `source_doc`)[cite: 3].
5. **Remove Old Vectors Before Re-Indexing (`DeleteOldVectors` & `WaitForDelete`):** Sends a delete request to Pinecone for existing vectors matching `source_doc` in that namespace—preventing duplicate chunks when a PDF is updated—and waits safely before inserting new data[cite: 3].
6. **Split, Embed & Store in Pinecone (`TextSplitter`, `DefaultDataLoader`, `EmbeddingsWithGemini`, `Pinecone Vector Store`):** Splits the PDF text into chunks (`900` characters with `100` overlap), creates embeddings with `models/gemini-embedding-001`, and stores them in the `campus-recruitment-hub` Pinecone index[cite: 3].

---

### Part 2: Bilingual RAG Recruitment Agent (AI Agent)

This workflow handles live conversations with prospective students in German or English[cite: 3]:

1. **Receive User Message (`When chat message received`):** Starts the conversation via n8n's hosted chat interface with a welcome message outlining available program topics (facts & figures, tuition, deadlines, curriculum, admission requirements)[cite: 1, 3].
2. **Keep Conversation Context (`Simple Memory`):** Remembers recent messages in the chat (window of `10` turns) so students can ask natural follow-up questions without repeating themselves[cite: 3].
3. **Search the Right Program Namespace (`search_bscbba`, `search_mscgl`, `search_mscmgt`, `search_mscbads`, `search_mba`):** The AI Agent picks the matching retrieval tool for the requested study program (or multiple tools for comparisons) and searches the corresponding Pinecone namespace using `models/gemini-embedding-001`[cite: 3].
4. **Generate Grounded Bilingual Answers (`OpenRouter Chat Model` — `google/gemini-2.5-flash`):** Replies in the same language the student uses (German or English), sticks strictly to retrieved facts, asks a quick clarifying question if the program is unclear, and cites the source document (`source_doc`) at the end[cite: 3].

---

## Tools & Technologies Used

* **Workflow & Chat Orchestration:** n8n (Chat Trigger, AI Agent, Switch, Merge, HTTP Request, Document Loader & Text Splitter)[cite: 1, 3]
* **LLM & Embeddings:** OpenRouter (`google/gemini-2.5-flash`) & Google Gemini Embeddings (`models/gemini-embedding-001`)[cite: 3]
* **Vector Database:** Pinecone (`campus-recruitment-hub` index with program-specific namespaces)[cite: 3]
* **Source Storage:** Google Drive[cite: 3]

---

## Key Skills Demonstrated

* **Domain-to-Workflow Translation:** Turning real student recruitment and admissions knowledge into clear rules, namespaces, and guardrails (no guessing, source citations, clarifying questions)[cite: 3].
* **Clean Data Updates (Idempotency):** Designing a delete-before-insert step so updated PDFs replace old chunks cleanly instead of creating duplicates[cite: 3].
* **Practical RAG Architecture:** Separating data by study program (`prog-*` namespaces) and pairing conversation memory with retrieval tools[cite: 3].

---

## Possible Next Steps

* **CRM Handoff:** Offer interested applicants an easy way to share their contact details or book an advising call directly into a CRM (such as HubSpot).
* **Topic & Question Tracking:** Review common student questions and missing answers to improve program brochures and FAQs.
* **Automated Quality Checks:** Test sample German and English questions regularly after PDF updates to verify answer accuracy and citations.