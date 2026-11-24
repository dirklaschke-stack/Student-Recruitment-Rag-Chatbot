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