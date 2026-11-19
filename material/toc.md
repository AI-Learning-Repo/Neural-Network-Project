# Course Outline: RAG with LangChain and Deep Agents

This module covers three complementary parts:

- **Theory — Understanding RAG:** Concepts, architecture, embeddings, agentic RAG, and evaluation
- **Lab 1 — RAG with Markdown and In-Memory Storage:** Build the complete RAG and Deep Agents workflow
- **Lab 2 — RAG with PDF/DOCX and ChromaDB:** Reinforce the workflow with richer document formats, persistent storage, and automated grading

---

## 1. Lab 1 — RAG with Markdown, `InMemoryVectorStore`, and Deep Agents

**Main objective:** Build an agentic RAG question-answering system from Markdown files while understanding every stage of the pipeline.

### Topics Covered

1. **Introduction to RAG**
   - What RAG is and why external knowledge matters
   - The fictional travel-company dataset
   - Baseline LLM versus RAG-enabled agent

2. **Environment and dataset setup**
   - Google Colab and Groq API configuration
   - Installing LangChain, Deep Agents, and HuggingFace dependencies
   - Downloading three Markdown handbooks

3. **Loading documents**
   - Reading Markdown with `Path.read_text()`
   - LangChain `Document` objects
   - `page_content` and source metadata

4. **Text splitting and chunking**
   - `RecursiveCharacterTextSplitter`
   - `chunk_size=600`, `chunk_overlap=150`
   - Inspecting chunk boundaries and comparing overlap settings

5. **Embeddings and vector storage**
   - `HuggingFaceEmbeddings`
   - `all-MiniLM-L6-v2` (384-dimensional vectors)
   - `InMemoryVectorStore`
   - Similarity search and inspecting retrieved chunks
   - Optional preview of ChromaDB

6. **Building the Deep Agent**
   - Custom `search_trip_docs` tool
   - `StateBackend` filesystem
   - Offloading retrieved chunks into files
   - `create_deep_agent`
   - `TodoListMiddleware` and `write_todos`
   - Orchestrator and `chunk-analyst` subagents
   - `task()` delegation, tool isolation, and context management

7. **Testing the RAG agent**
   - Single-document questions
   - Multi-document and multi-part questions
   - Inspecting tool calls and agent messages
   - Partial-answer and unanswerable-question tests

8. **Additional topics**
   - Prompt-injection security considerations
   - All-in-one Colab reference implementation
   - Optional LangSmith tracing

**Key learning outcome:** Understand the complete agentic RAG pipeline using straightforward Markdown inputs and in-memory vector storage.

---

## 2. Lab 2 — RAG with PDF/DOCX, ChromaDB, and Deep Agents

**Main objective:** Reinforce the agentic RAG workflow while introducing richer document formats, persistent vector storage, and automated quality evaluation.

### Topics Covered

1. **Introduction and baseline test** *(reinforced)*
   - Why RAG is useful
   - Same fictional travel-company knowledge base
   - Baseline LLM versus RAG agent

2. **Environment and dataset setup** *(reinforced + new dependencies)*
   - Google Colab, Groq, and HuggingFace setup
   - Installing `pypdf`, `python-docx`, `langchain-chroma`, and ChromaDB
   - Downloading PDF and DOCX handbooks

3. **Loading PDF and DOCX documents** *(new)*
   - Extracting PDF text with `pypdf.PdfReader`
   - Extracting Word paragraphs with `python-docx`
   - Creating LangChain `Document` objects
   - Preserving PDF page numbers and file-type metadata
   - Limitations of DOCX table extraction

4. **Text splitting and chunking** *(reinforced)*
   - `RecursiveCharacterTextSplitter`
   - Chunk size and overlap
   - Chunk-boundary experiments

5. **Embeddings and vector storage** *(reinforced + extended)*
   - Same HuggingFace embedding model (`all-MiniLM-L6-v2`)
   - ChromaDB instead of `InMemoryVectorStore`
   - Persisting and reloading a vector collection
   - Resetting versus reusing a collection
   - Metadata filtering
   - Inspecting similarity-search results

6. **Building the Deep Agent** *(reinforced)*
   - Custom retrieval tool
   - `StateBackend` and filesystem offloading
   - Orchestrator and `chunk-analyst`
   - `TodoListMiddleware`, `write_todos`, and `task()`
   - Tool inheritance and isolated subagent contexts
   - Model options and sequential/parallel tool calls

7. **Testing the RAG agent** *(reinforced)*
   - Single-document retrieval
   - Multi-document questions
   - Inspecting tool calls and final answers
   - Partial-answer and out-of-corpus tests

8. **Evaluation and quality control** *(new)*
   - `RubricMiddleware`
   - LLM-as-a-judge
   - Evaluator–optimizer feedback loop
   - Revision against grading criteria
   - Limitations of rubric-based groundedness checks

9. **Additional topics**
   - Prompt-injection security
   - Optional LangSmith tracing
   - Persistence and production considerations

**Key learning outcome:** Reinforce the full RAG workflow while learning to process PDF and Word documents, work with a persistent vector database, and apply automated answer evaluation.

---

## 3. Theory — From Embeddings to Agentic RAG

**Main objective:** Explain the concepts and architectural decisions behind both practical labs.

1. **Why Retrieval-Augmented Generation?**
   - LLM knowledge limitations, private and recent data, hallucinations, context windows, and incorporating external evidence.

2. **Embeddings and semantic similarity**
   - Keyword versus semantic search, word versus sentence embeddings, vector representations, cosine similarity, 2D vector visualization, and a concrete 384-dimensional embedding example. Also covers dense retrieval limitations and hybrid search.

3. **The LangChain RAG pipeline**
   - Indexing (`load → split → embed → store`), querying (`retrieve → augment → generate`), document metadata, chunking, `InMemoryVectorStore` versus ChromaDB, and similarity search.

4. **Deep Agents and agentic RAG**
   - Traditional versus agentic RAG, planning, custom retrieval tools, filesystem offloading, subagents, context isolation, tool inheritance, delegation, model routing, and latency/cost trade-offs.

5. **Evaluation and grading rubrics**
   - Retrieval quality, evidence groundedness, answer correctness, abstention, `RubricMiddleware`, the evaluator–optimizer loop, and offline evaluation with LangSmith.

6. **Documentation and references**
   - Official resources for LangChain, Deep Agents, ChromaDB, Groq, and LangSmith.

---

## 4. Lab 1 vs. Lab 2 — Comparison

The table below summarizes which concepts are reinforced across both labs and which components change in Lab 2.

| Concept / Component | Lab 1 | Lab 2 |
| --- | --- | --- |
| RAG fundamentals | Introduced | Reinforced |
| Baseline LLM test | Introduced | Reinforced |
| Document format | Markdown (`.md`) | PDF + DOCX |
| Document loader | `Path.read_text()` | `pypdf`, `python-docx` |
| LangChain `Document` | Introduced | Reinforced |
| Text chunking | `600 / 150` | `600 / 150` |
| Embeddings | HuggingFace MiniLM | Same model |
| Vector database | `InMemoryVectorStore` | ChromaDB |
| Persistence | Ephemeral | Persistent on disk |
| Metadata | Source filename | Filename, file type, PDF page |
| Similarity search | Introduced | Reinforced |
| Custom retrieval tool | `search_trip_docs` | Same pattern |
| Filesystem offloading | `StateBackend` | Same pattern |
| Task planning | `TodoListMiddleware` | Same pattern |
| Subagents | `chunk-analyst` | Same pattern |
| Multi-document questions | Introduced | Reinforced |
| Abstention tests | Introduced | Reinforced |
| Automated grading | Mentioned conceptually | `RubricMiddleware` example |
| LangSmith tracing | Optional | Optional |

*Note:* The `InMemoryVectorStore` vs. `ChromaDB` distinction reflects the primary pipeline of each lab. Lab 1 may include a separate optional preview of ChromaDB, which should be kept independent from the in-memory store used by the Lab 1 agent.

---

## 5. How the Three Parts Connect

- **Theory — Concepts and Architecture:** Why RAG is needed, how embeddings and retrieval work, and the design trade-offs behind Deep Agents and evaluation.
- **Lab 1 — Practice:** Implement the full agentic RAG pipeline from scratch using lightweight Markdown files and `InMemoryVectorStore`.
- **Lab 2 — Reinforcement and Extension:** Repeat the same core workflow while focusing on what changes when introducing PDF/DOCX ingestion, persistent ChromaDB storage, and rubric-based evaluation.

---

## 6. Learning Objectives

By the end of the theory and two labs, learners will be able to:

1. Explain how RAG enhances language models with external information.
2. Describe embeddings, semantic similarity, and vector search.
3. Build a document indexing pipeline using LangChain.
4. Explain and experiment with chunk size and overlap.
5. Compare in-memory and persistent vector storage.
6. Process Markdown, PDF, and DOCX knowledge sources.
7. Build a Deep Agent with retrieval tools, task planning, and subagents.
8. Explain filesystem offloading and context management.
9. Test retrieval, groundedness, answer quality, and abstention.
10. Identify trade-offs in agentic RAG, including tool calls, token consumption, latency, and rate limits.