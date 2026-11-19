# Schedule — Week 5 

**Thursday, 19.11.2026 | 09:00–16:30 | Room: MMC223**


---

## Morning: From Embeddings to Agentic RAG with Deep Agents

> [!NOTE]
> **Groq vs. Gemini vs. Local Qwen:** Activities 1 and 2 are provided in both **Groq** and **Gemini** versions. They cover the **exact same RAG pipeline, dataset, and exercises**, differing only in the LLM provider (`ChatGroq` vs. `ChatGoogleGenerativeAI`). You are also encouraged to try both activities with **Qwen 3.5 (`2B` or `4B`) in Google Colab** by running Ollama on a free **T4 GPU** runtime. More info [here](./material/activity-1-2-local-qwen.ipynb)


- [Summary](./material/theory.md)
- **Activity 1: RAG with Markdown & `InMemoryVectorStore`** *(choose one provider)*
  - [Groq](./material/activity1.md)
  - [Gemini](./material/activity1gemini.md)
- **Activity 2: RAG with PDF/DOCX, ChromaDB & Grading** *(choose one provider)*
  - [Groq](./material/activity2.md)
  - [Gemini](./material/activity2gemini.md)
- **Helper:**
  - [Convert Markdown to DOCX and PDF](./material/markdown_to_docx_and_pdf.ipynb)
  - [Gemini vs Groq](./material/gemini.md) <!-- verify filename -->

---

<details>
<summary>Links used in Session</summary>

- https://docs.langchain.com/oss/python/integrations/chat/google_generative_ai
- https://docs.langchain.com/oss/javascript/langchain/overview
- https://docs.langchain.com/oss/python/deepagents/rag
- https://github.com/langchain-ai/docs/blob/main/src/oss/deepagents/rag.mdx
- https://docs.langchain.com/oss/python/deepagents/skills
- https://docs.langchain.com/oss/python/integrations/document_loaders
- https://github.com/polzerdo55862/RAG-with-Python-Cookbook
- https://docs.langchain.com/oss/python/deepagents/customization

<!-- - https://www.tavily.com/ -->

</details>

----


## Afternoon: Sprint 2 Review & Sprint 3 Planning

- **Mock Zoom Poll** *(join the class Zoom session muted with audio off to test voting)*
- **Sprint 2 Presentations: Jigsaw Challenge**
  - **Before voting in your assigned Jigsaw group:**
    - **Record the group number** of every representative in your Jigsaw group.
    - **Best Presentation:** Vote for **exactly one** other group from your Jigsaw group (exclude your own group).
    - **Best Concept Connectors:** Select **two distinct other groups** in a **4-person** Jigsaw group, or **one other group** in a **3-person** Jigsaw group (exclude your own group).
    - **Important:** Vote only for groups represented in your assigned Jigsaw group. Double-check group numbers before submitting.
  - [Jigsaw Voting Guide](./material/sprint2_voting_guide.md)
  - [Jigsaw Scoring](./material/sprint2_scoring.md)
- **Sprint 3 Planning**
  - Review feedback and lessons learned from Sprint 2.
  - Update and prioritize the Product Backlog.
  - Define a clear Sprint 3 Goal.
  - Divide Sprint 3 Backlog Items into implementation tasks.
  - Assign responsibilities and identify important risks and dependencies.
  - Plan the next working increment, including RAG, fine-tuning, and multi-agent capabilities.
  - **Reminder:** Sprint 3 builds on Sprint 2. Existing functionality should be preserved and improved.
  - [More here](./material/sprint3-planning.md)

---

### Misc. & Optional Resources


- **Bonus — Weight-Space Programming:**
  - [Qwen 3.8 27B on my laptop can interpret model weights, too](https://simonwillison.net/2026/Nov/3/qwen-can-interpret-model-weights/)
  - [LLMs can natively read & write neural network weights](https://rikiyat.github.io/blog/weight-space-programming/)
  - [Weight-Space Programming Works Beyond GPT-6 Astra, Replications Show](https://rits.shanghai.nyu.edu/ai/weight-space-programming-works-beyond-gpt-6-astra-replications-show/)
  - [Weight-space programming](https://en.wikipedia.org/wiki/Weight-space_programming)
- **Video(s):**
  - [What are Deep Agents? (LangChain)](https://www.youtube.com/watch?v=IVts6ztrkFg)
  - [Complete Deep Agents Course With Langchain In 3 Hours](https://www.youtube.com/watch?v=J8DzuMmSDEU)


<!-- 
- [Outline](./material/toc.md) 
- [Deep Agents](./material/deep-agents.md)

-->

<!-- - [Vector Databases Full Course for Beginners – Embeddings, Similarity Search & ChromaDB](https://www.youtube.com/watch?v=lR931Iz8Z6U) *(Note: this video uses Gemini embeddings, whereas our labs use HuggingFace `all-MiniLM-L6-v2` + Groq.)* -->



