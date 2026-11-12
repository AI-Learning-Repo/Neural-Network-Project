# Sprint 2 Deliverables & Submission Requirements


## 1. Purpose of Sprint 2

Sprint 2 builds on the **working product delivered in Sprint 1**. It is an **increment**, not a separate MCP exercise or a replacement application. Your team should retain and improve the original assistant, add a **Model Context Protocol (MCP) server and client**, make the application usable through a **Streamlit GUI**, and introduce **function unit testing** while continuing to evaluate the complete assistant.

The outcome is an integrated, demonstrable product that still performs its Sprint 1 tasks and now has a useful end-to-end workflow involving MCP.

By the Sprint 2 Review, your team should be able to explain:

1. What the Sprint 1 product already did and what changed in Sprint 2.
2. How the Streamlit interface, Qwen assistant, existing tools, database, API integration, MCP client, and MCP server fit together.
3. What the MCP server exposes, how the client connects to and invokes it, and how the returned results are used by the assistant.
4. What your assistant-level evaluation and function unit tests show, including any regression or failure.
5. How your team used Scrum to plan, implement, inspect, and adapt the increment.

## 2. Sprint 2 scope: Sprint 1 baseline + new increment

**All Sprint 1 expectations carry forward.** If a Sprint 1 feature was working, it should continue to work. If a Sprint 1 item was incomplete or blocked, keep it visible in the Product Backlog and explain its current status; do not silently treat it as completed.

| Area | Sprint 2 expectation | Evidence to provide |
|---|---|---|
| **Existing assistant/agent** | Maintain the meaningful user workflows implemented in Sprint 1. | Demonstrate that an original user task still works. |
| **Qwen model** | Continue using a suitable Qwen model, in Colab **and** through Groq. Model comparison is  required. | Identify the model/access method in the updated architecture and README. |
| **Existing tools** | Keep meaningful generic Python and API-wrapped tools integrated into the assistant. | Show the assistant using an existing tool and its result. |
| **Database** | Preserve the ability to store, access, and filter project-relevant data as needed for the existing workflow. | Demonstrate a database-backed request and verify the returned data. |
| **External API** | Preserve the Sprint 1 API integration target; repair or advance previously blocked integration where feasible. | Show working use, or clearly document evidence of the remaining blocker and next action. |
| **MCP server: new** | Implement a working MCP server exposing **at least one project-relevant tool**. The tool should serve a real need of the product. | Show the server implementation, its exposed tool, and a successful invocation. |
| **MCP client: new** | Implement an MCP client that connects to your MCP server, accesses its offered tool, and integrates that capability into the assistant workflow. | Show a user request reaching the assistant, the MCP client/server interaction, and the returned result. |
| **Streamlit GUI: new** | Provide a **required** Streamlit interface for interacting with the integrated assistant. The GUI must connect to real application functionality. | Demonstrate a user request and response through Streamlit, including a tool-backed task. |
| **Assistant evaluation: continued** | Evaluate the complete Sprint 2 assistant, including a previously working task and the new MCP-enabled workflow. | Recorded expected vs. observed outcomes, source/tool evidence, and a summary of failures and limitations. |
| **Function unit tests: new** | Write and run unit tests for the project-defined functions, including relevant business logic, data operations, tool logic, and MCP-facing functions. | Test code, repeatable run command, and test results. |
| **Scrum and documentation** | Keep the Product Backlog and vision current, create Sprint 2 artifacts, and document the increment and retrospective. | Current repository documents, acceptance evidence, and review notes. |

### How MCP should be integrated

The **MCP server** provides one or more useful tools. The **MCP client** connects to the server and invokes the selected tool. The assistant should be able to use the resulting information or operation in a meaningful user task.

For example, an existing database search or a project-relevant API function may be exposed as an MCP tool. You **do not** need to convert every existing tool to MCP, build multiple servers, or host a remote production service. One correctly integrated MCP server and one working client are sufficient if they support a useful product workflow.

**A standalone MCP demo that is not connected to your assistant does not meet the integration objective.** A Streamlit page that only displays static example responses does not meet the GUI objective either.

### In scope

- Continued functionality from Sprint 1, including Qwen, existing tools, database operations, and the external API target.
- A working **MCP server**, **MCP client**, and at least one meaningful MCP tool integrated into the application.
- A working **Streamlit GUI** for the assistant.
- **Unit tests for project-defined functions**, together with records of the test run.
- Continued **assistant-level, end-to-end evaluation** and evidence that earlier functionality still works.
- Updated GitHub code, documentation, Product Backlog, Sprint 2 Backlog, Sprint Review, and retrospective.

### Out of scope for Sprint 2

- Retrieval-Augmented Generation (**RAG**).
- Multi-agent architectures.
- Fine-tuning.
- Production deployment or replacing all existing tools with MCP implementations.

Do not add unrelated new technologies merely to increase the apparent complexity of the project.

## 3. Required submission deliverables

Submit **a GitHub repository** for the same product and the **Sprint 2 Review presentation**. 

| Deliverable | Minimum expected content | Suggested location |
|---|---|---|
| **1. Updated working application** | Sprint 1 assistant functionality plus integrated Streamlit GUI and MCP client/server workflow. | `src/`, `app.py`, or an equivalent structure |
| **2. MCP server and client** | Server code, exposed tool(s), client connection/invocation code, and integration into the assistant. | `src/mcp_server/`, `src/mcp_client/` or equivalent |
| **3. Existing tools, database, and API** | Previously implemented tools and data operations remain functional; incomplete API work has visible status and evidence. | `src/`, `data/`, configuration, and relevant docs |
| **4. Function unit tests** | Tests for project-defined functions, including normal, edge, and error behavior where relevant; instructions and results. | `tests/`, test-run output/summary |
| **5. Assistant evaluation** | End-to-end Sprint 2 scenarios, actual behavior, verification against tools/data, regressions, failures, and conclusions. | `docs/SPRINT_2_EVALUATION.md` |
| **6. Updated README and setup** | Setup and launch steps for dependencies, database, MCP server, MCP client/application, Streamlit, and evaluation/tests. | `README.md` |
| **7. Updated Product Vision and Product Backlog** | Current user need/scope, prioritized backlog, acceptance criteria, and any changes based on Sprint 1 feedback. | `docs/PRODUCT_VISION.md`, `docs/PRODUCT_BACKLOG.md` or linked GitHub Issues |
| **8. Sprint 2 Backlog** | Sprint Goal, selected PBIs, tasks, owners, estimates, status, acceptance evidence, risks, and blockers. | `docs/SPRINT_2_BACKLOG.md` |
| **9. Sprint 2 Review and retrospective** | Completed/partial/blocked work, increment evidence, feedback and backlog changes, team-process reflection, and specific improvement. | `docs/SPRINT_2_REVIEW.md` |
| **10. Sprint 2 presentation** | Slides, prepared for the Jigsaw Review and live demonstration. | Presentation file, also included in OMA submission |

Suggested filenames are not mandatory. All required information must be easy to locate. Do not delete or overwrite the historical Sprint 1 review, evaluation, or Sprint Backlog.

<!-- 
### Suggested repository layout

```text
ai-project/
├── README.md
├── requirements.txt                 # or equivalent dependency file
├── .env.example                     # names only: no credentials
├── app.py                            # Streamlit entry point (example)
├── src/
│   ├── assistant/                   # Qwen integration and orchestration
│   ├── tools/                       # existing project tools
│   ├── database/                    # data access and filtering
│   ├── mcp_server/                  # MCP server and exposed tools
│   └── mcp_client/                  # MCP connection/invocation
├── tests/                            # required function unit tests
├── data/                             # permitted sample data only
├── docs/
│   ├── PRODUCT_VISION.md
│   ├── PRODUCT_BACKLOG.md
│   ├── SPRINT_1_BACKLOG.md
│   ├── SPRINT_1_EVALUATION.md
│   ├── SPRINT_1_REVIEW.md
│   ├── SPRINT_2_BACKLOG.md
│   ├── SPRINT_2_EVALUATION.md
│   └── SPRINT_2_REVIEW.md
└── notebooks/                        
``` 
-->

### What the README must explain

Another person should be able to run the integrated product without an oral walkthrough:

- The user problem, the product's **Sprint 1 baseline**, and the **Sprint 2 increment**.
- Which Qwen model/access route is used (Colab / Groq); required configuration without exposing secrets.
- How to install dependencies and prepare approved sample data/database.
- How to start the **MCP server**, start/connect the **MCP client** as part of the application, and launch **Streamlit**; state the required order when relevant.
- What the MCP server exposes and which user request demonstrates its integration.
- How to run the function unit tests and where to find test results and assistant evaluation evidence.
- Current limitations and any incomplete features.

## 4. Required evaluation of the AI assistant

**Assistant-level evaluation remains mandatory** and is different from unit testing. Evaluate the complete user flow: **Streamlit request → Qwen assistant → existing tools and/or MCP client → MCP server → database/API/function → tool result → assistant response**. Actual applications may use more than one path.

### Minimum evaluation requirements

1. **Re-run the three representative Sprint 1 scenarios** (or equivalent scenarios covering the same core behavior) to check that earlier functionality still works.
2. Include **at least one end-to-end success scenario that uses the MCP client and MCP server**, not just a direct server call.
3. Include **at least one MCP-related edge or failure scenario**, such as invalid parameters, no matching data, server unavailable, or a tool error.
4. Perform and record **at least one scenario through the Streamlit GUI**; this may also be one of the scenarios above.
5. Record the input, expected behavior, actual behavior, tool/MCP/data evidence, result (Pass / Partial / Fail), and relevant conditions.
6. Compare answers against actual database records, API responses, or MCP tool results rather than judging output solely by fluency.
7. Summarize which Sprint 1 capabilities were preserved, what the new MCP path adds, and the main limitations or improvements needed.

This normally produces **at least five distinct end-to-end scenarios**: three regression scenarios plus MCP success and MCP edge/failure. Some scenarios can also verify the Streamlit interface and external API. If an earlier Sprint 1 test cannot be rerun, record why.

### Assistant evaluation record template

Copy and adapt this table in `docs/SPRINT_2_EVALUATION.md`.

| ID | Scenario / user request | Path exercised | Expected behavior | Observed behavior and source/tool evidence | Result |
|---|---|---|---|---|---|
| E-01 | `[Sprint 1 normal task]` | Existing assistant/tool | `[expected]` | `[observed + evidence]` | Pass / Partial / Fail |
| E-02 | `[Sprint 1 database/filter task]` | Database tool | `[expected]` | `[observed + verified records]` | Pass / Partial / Fail |
| E-03 | `[Sprint 1 no-match/invalid-input task]` | Existing error handling | `[expected]` | `[observed]` | Pass / Partial / Fail |
| E-04 | `[MCP-enabled user task]` | Assistant → MCP client → server → tool | `[expected]` | `[MCP invocation/result + response]` | Pass / Partial / Fail |
| E-05 | `[MCP invalid input/no match/server failure]` | MCP failure/edge path | `[safe, clear handling]` | `[observed]` | Pass / Partial / Fail |

**Short evaluation summary**

- **Sprint 1 functions preserved:** `[what worked / what regressed]`.
- **New MCP-enabled task:** `[the value for the user]`.
- **Streamlit interaction tested:** `[scenario and result]`.
- **Results:** `[number passing / partial / failing, based on recorded cases]`.
- **Important limitation and next improvement:** `[specific evidence and action]`.

If model responses vary between runs, record that behavior. Do not claim a general reliability rate from a small or undocumented sample.

## 5. Required function unit tests

Function unit tests are **required in Sprint 2**. They check the behavior of functions or small components in isolation; they do **not** replace assistant-level evaluation.

### What to test

Create tests for the **project-defined functions** in your application, including where applicable:

- Data insertion, retrieval, filtering, and validation.
- Generic functions and API tool adapters.
- Tool input validation and result processing.
- Functions exposed by the MCP server.
- MCP client-side processing or wrappers that contain project-specific logic.
- Business logic called from the Streamlit interface (separate this from UI rendering where practical).

For each function, verify relevant **normal behavior, boundary/empty results, and errors**. Use controlled input and expected output. For services or the Qwen model that are not deterministic or available offline, use test doubles/mocks where appropriate. You do not need to unit-test the internals of third-party libraries.

Use a framework such as `pytest`; the choice is yours. If a project-defined function is not directly unit-testable, document the reason and the alternative validation performed. The expectation is comprehensive coverage of your own meaningful functions, not a claim that all possible system behavior has been tested.

### Unit test evidence template

| Function/component | Test input or condition | Expected result | Actual result | Status |
|---|---|---|---|---|
| `[filter_records]` | `[valid filter]` | `[matching rows only]` | `[observed]` | Pass / Fail |
| `[filter_records]` | `[no match]` | `[empty result]` | `[observed]` | Pass / Fail |
| `[MCP-exposed function]` | `[valid parameters]` | `[validated tool result]` | `[observed]` | Pass / Fail |
| `[API adapter]` | `[mocked service error]` | `[clear error handling]` | `[observed]` | Pass / Fail |

Include the tests in the repository, a command to execute them, and a short test-run summary. Fix failing tests or make remaining failures clearly visible. **Unit tests and assistant evaluation are two separate required forms of evidence.**

## 6. Scrum artifacts and process evidence

Use the existing Scrum framework to manage this increment. Your documentation and presentation must connect Scrum practices to decisions actually made during Sprint 2.

### Roles

- **Product Owner:** clarifies the value of the next increment and helps order the Product Backlog.
- **Scrum Master:** facilitates the process and makes blockers visible.
- **Developers:** **all members** contribute to implementation, integration, testing, and understanding, including those holding the other responsibilities.

State who held each responsibility and how technical work was shared. Roles may remain the same or change from Sprint 1; record what actually happened.

### Artifacts

- **Product Backlog:** updated priorities and acceptance criteria based on Sprint 1 feedback and new knowledge.
- **Sprint 2 Backlog:** Sprint Goal, selected items, tasks, responsibilities, progress, and acceptance evidence.
- **Working Increment:** the Sprint 1 product **plus** functioning MCP client/server, Streamlit UI, and function testing improvements.

The **Definition of Done** supports decisions about what is actually complete.

### Scrum events / ceremonies

| Event | Evidence to record |
|---|---|
| **Sprint Planning** | Sprint 2 Goal, selected PBIs, estimates, scope choices and dependencies. |
| **Daily Scrum / regular check-ins** | Actual frequency and method; one obstacle, adjustment, or coordination decision. |
| **Sprint Review** | Demonstrable increment, accepted/incomplete items, feedback, and Product Backlog updates. |
| **Sprint Retrospective** | One effective practice, one process problem, and one concrete improvement for the following sprint. |

Do not claim an event occurred if it did not. Explain adaptations honestly.

### Sprint 2 Backlog template

| Product Backlog item | Implementation tasks | Owner(s) | Estimate | Status | Acceptance evidence |
|---|---|---|---|---|---|
| `[PBI-...]` | `[preserve/improve existing user workflow]` | `[names]` | `[size/time]` | Done / In progress / Blocked | `[demo/test]` |
| `[PBI-...]` | `[implement MCP server tool]` | `[names]` | `[size/time]` | Done / In progress / Blocked | `[successful server tool call]` |
| `[PBI-...]` | `[connect MCP client to assistant]` | `[names]` | `[size/time]` | Done / In progress / Blocked | `[assistant → client → server evidence]` |
| `[PBI-...]` | `[integrate Streamlit GUI]` | `[names]` | `[size/time]` | Done / In progress / Blocked | `[live GUI task]` |
| `[PBI-...]` | `[add function unit tests and re-evaluate assistant]` | `[names]` | `[size/time]` | Done / In progress / Blocked | `[test run + evaluation report]` |

## 7. Definition of Done for Sprint 2

Use the checklist for the integrated increment; incomplete items remain visible in the backlog.

- [ ] The **Sprint 1 baseline** still performs its selected user tasks; any regression is recorded and addressed or clearly marked incomplete.
- [ ] The Qwen assistant and existing tools/database functions are integrated and runnable.
- [ ] The status of the external API integration is documented and supported by working evidence or a specific unresolved blocker.
- [ ] An **MCP server** runs and exposes at least one project-relevant tool.
- [ ] An **MCP client** connects to that server and invokes its tool **as part of the assistant workflow**.
- [ ] The required **Streamlit GUI** accepts user requests and displays real assistant responses, including an MCP-backed workflow.
- [ ] The updated assistant has been **evaluated end to end**, including regression, MCP success, and failure/edge cases.
- [ ] **Function unit tests** for project-defined functions have been written, executed, and saved with results.
- [ ] The relevant acceptance criteria are met, and important failures are handled appropriately.
- [ ] GitHub contains the updated runnable code, setup instructions, Product Backlog, Sprint 2 Backlog, evaluation, and review evidence.
- [ ] No credentials, secrets, personal data, or datasets without permission are committed.
- [ ] Every team member can explain and demonstrate what was implemented, what failed, and what remains incomplete.

<!-- **Written code is not automatically Done.** Integration must run, demonstrate the intended behavior, meet acceptance criteria, and have recorded evidence. -->

## 8. Submission checklist

Before the Sprint 2 Review, check:

- [ ] The **project repository** is accessible to reviewers.
- [ ] Sprint 1 code and evidence remain available; current functionality is documented.
- [ ] The README explains how to start MCP components and Streamlit and how to run unit tests.
- [ ] The MCP server and client form a genuine integrated path in the assistant.
- [ ] Streamlit is functional and demonstrates both existing capability and MCP-enabled capability.
- [ ] Unit test code and executed results are included.
- [ ] The updated assistant evaluation includes Sprint 1 regression cases and new MCP scenarios.
- [ ] Product Vision/Backlog, Sprint 2 Backlog, Sprint 2 Review, and retrospective are available.
- [ ] Presentation materials follow **Document B**.
- [ ] Every team member is prepared for the Jigsaw presentation and technical questions.

**Submit:** your **GitHub repository link** and **Sprint 2 presentation slides/file** to **OMA**. Keep supporting deliverables in the repository.


