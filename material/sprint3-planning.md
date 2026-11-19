# Sprint 3 Planning (Final Sprint)

1. **Sprint Goal**

   - Define what the group intends to achieve by the **final Sprint 3 Review in Week 8**.
   - Explain how Sprint 3 will bring the product to a complete, final working state and provide additional value to its users.

2. **Sprint 3 Backlog**

   - Select and prioritize items from the Product Backlog, incorporating relevant lessons learned and ideas from the Sprint 2 Jigsaw Challenge.
   - Include the required Sprint 3 improvements:
     - **Preserve and improve Sprint 1 & Sprint 2 functionality:** LLM, tools, API integration, database, MCP server/client integration, and Streamlit GUI.
     - **Integrate RAG (Retrieval-Augmented Generation):** Document ingestion, chunking, embeddings, vector storage, and retrieval-grounded responses.
     - **Implement Multi-Agent capabilities:** Multi-agent orchestration and task delegation (e.g., specialized subagents / Deep Agents) integrated into the assistant workflow.
     - **Add Fine-Tuning:** Fine-tune a model (or adapter) for a clearly defined project task and evaluate it against the base model.
     - **Testing and Evaluation:** Expand unit tests and continue end-to-end assistant evaluation across existing and new Sprint 3 features.

3. **Tasks and Responsibilities**

   - Break selected backlog items into manageable implementation tasks.
   - Identify the team members responsible for each task.
   - Provide rough estimates and identify dependencies.
   - Assign technical development work to every team member.

4. **Timeline and Milestones**

   - **Weeks 5–6:** Design, implement, and test **RAG** integration (knowledge sources, embeddings, vector database, and retrieval workflow).
   - **Weeks 6–7:** Implement and integrate **multi-agent** workflows (planning, subagent delegation, and coordination with existing tools, MCP, and Streamlit).
   - **Week 8:** Complete **fine-tuning** and baseline comparisons, finalize full-system integration, run final tests and evaluations, fix critical issues, update documentation, and prepare the **final Sprint 3 Review** presentation in Week 8.

   Because this is the final sprint and includes three major features, parallelize work cleanly where possible and adjust the sequence to keep the planned scope realistic and deliverable by Week 8.

5. **Acceptance Criteria and Evaluation**

   - Define how the group will determine whether each selected backlog item is complete.
   - Identify representative test cases for the assistant, covering:
     - **RAG:** Retrieval quality, evidence-grounded answers, and abstention on out-of-corpus questions.
     - **Multi-agent workflows:** Task planning, agent delegation/routing, and tool/MCP execution.
     - **Fine-tuning:** Quantitative and/or qualitative comparison against the base model.
   - Plan regression checks to verify that existing Sprint 1 and Sprint 2 functionality (tools, APIs, database, MCP, and Streamlit GUI) still works reliably.
   - Include evidence of function-level unit testing and assistant-level evaluation.

6. **Risks and Definition of Done**

   - Identify important technical risks, dependencies, and possible blockers (e.g., fine-tuning compute/time constraints, API rate limits or latency from RAG + multi-agent calls, and Week 8 integration bottlenecks).
   - Agree on a **Final Sprint Definition of Done**: all features are integrated, runnable end-to-end, tested, evaluated, committed to GitHub, documented (including clear setup and usage instructions), and ready for the **Sprint 3 Review in Week 8**.