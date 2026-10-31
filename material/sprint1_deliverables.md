# Sprint 1 Deliverables & Submission Requirements

## 1. Purpose of Sprint 1

Sprint 1 turns the product idea selected during brainstorming into a **small, working, demonstrable AI assistant or agent**. The aim is to deliver one useful end-to-end user workflow, evaluate its behavior, and use the findings to refine the Product Backlog.

A successful Sprint 1 is a working increment that can be run, demonstrated, and checked against its intended behavior. 

By the end of the sprint, your team should be able to explain:

1. Who the product helps and what problem it addresses.
2. How the assistant uses an LLM, tools, data, and  external APIs.
3. What was completed, what is not yet working, and what evidence supports your conclusions.
4. How your team used Scrum to plan, coordinate, inspect, and improve its work.

## 2. Technical scope and expectations


| Area | Sprint 1 expectation | Evidence to provide |
|---|---|---|
| **Working assistant/agent** | A user can enter a meaningful request and receive a response from the integrated application. | Runnable demonstration of a real user task. |
| **Qwen** | Use a suitable Qwen model in Colab and accessed through Groq. | Identify the model and how your application accesses it. |
| **Tools** | Integrate meaningful callable tools into the assistant workflow; a tool may be a generic Python function or an adapter to a service. Tool results should influence the answer. | Demonstrate the tool being invoked and its returned result. |
| **Database** | Use a database relevant to the product, with data inserted/stored and then accessed and filtered through application functionality. | Demonstrate requests using the database and show how the result matches stored records. |
| **External API** | Aim to connect a project-relevant external APIs through
 tools. | Demonstrate a successful integration. |
| **User interaction** | Provide a usable way to interact with the assistant (for example, notebook interaction, a CLI, or a simple interface). | Show the user request and the resulting answer. |
| **Assistant evaluation** | **Required:** run representative end-to-end scenarios and compare expected with observed behavior. | Test records and a short evaluation summary (see Section 4). |
| **Streamlit GUI** | **Optional in Sprint 1.** A Streamlit interface is planned as a Sprint 2 requirement. | Not required for Sprint 1 submission. |
| **Function unit tests** | **Optional in Sprint 1.** Unit testing of functions becomes required in Sprint 2. | Include tests if implemented; they do not replace assistant evaluation. |

**Integration matters.** For example, a user request should reach the assistant, lead to appropriate tool calls, use the returned database or API information, and produce a useful response. A standalone database exercise and a separate chatbot do not by themselves establish this integration.

### In scope

- A small, testable user task relevant to the product vision.
- Qwen-based assistant logic, integrated tool use, and database access/filtering.
- External API integration, including validation and error handling.
- Clear feedback when data is missing or a service fails.
- Basic documentation, Scrum artifacts, and **assistant-level evaluation**.

### Out of scope for Sprint 1

- Retrieval-Augmented Generation (**RAG**).
- Model Context Protocol (**MCP**).
- Multi-agent architectures.
- Fine-tuning.
- Streamlit GUI or a polished frontend.
- Testing.

Do not add advanced techniques simply to increase the number of technologies in the project.

## 3. Required submission deliverables

Submit one **shared GitHub repository**, containing the working implementation and the supporting artifacts below. 

| Deliverable | Minimum expected content | Suggested location |
|---|---|---|
| **1. Working code and data** | Runnable assistant, integrated tools, database setup/sample records, and API adapters. | `src/`, `data/`, `notebooks/`, as appropriate |
| **2. README and setup** | Product purpose, current capabilities, installation, configuration, run instructions, demo entry point, and known limitations. | `README.md` |
| **3. Product Vision** | Target users, problem, value, and first-version scope; update it if learning changed the original idea. | `docs/PRODUCT_VISION.md` |
| **4. Product Backlog** | Ordered candidate features/tasks, priorities, and acceptance criteria for important items; include next-sprint candidates. | `docs/PRODUCT_BACKLOG.md` |
| **5. Sprint 1 Backlog** | Sprint Goal; selected items; tasks, owners, estimates, status, acceptance criteria/evidence, and blockers. | `docs/SPRINT_1_BACKLOG.md` |
| **6. Assistant Evaluation** | Test scenarios, expected outcomes, actual results, observations, and conclusions about reliability and limitations. | `docs/SPRINT_1_EVALUATION.md` |
| **7. Sprint Review & Reflection** | Completed/partially completed/not completed work, evidence, feedback, changes to backlog, and a retrospective on teamwork using 4Ls framework. | `docs/SPRINT_1_REVIEW.md` |
| **8. Sprint 1 Presentation** | Slides. | Slides in the repository |

The suggested filenames and folders may be adapted. **The content and ability to find it are more important than the exact folder layout.** 

### Suggested repository layout

```text
ai-project/
├── README.md
├── requirements.txt              # or other dependency specification
├── .env.example                  # if used: variable names only; no secrets
├── docs/
│   ├── PRODUCT_VISION.md
│   ├── PRODUCT_BACKLOG.md
│   ├── SPRINT_1_BACKLOG.md
│   ├── SPRINT_1_EVALUATION.md
│   └── SPRINT_1_REVIEW.md
├── src/
├── tests/                        # optional in Sprint 1
├── data/                         # only data permitted to be shared
└── notebooks/                    
```

### What the README must make possible

A reader should be able to determine **what the product currently does and how to run it**. Include:

- A concise product description and one representative user request.
- How to install dependencies and start the application, including Colab and Groq setup.
- Required environment-variable names and how to configure them safely; never publish actual API keys.
- How to create/load the database or use a safe sample dataset.
- How to run the demonstrated workflow and where to find the evaluation record.
- A short list of known limitations or unfinished components.

## 4. Required evaluation of the AI assistant

**Assistant-level evaluation is mandatory in Sprint 1, even if you do not write automated unit tests.** Evaluate the integrated system from the perspective of a user's task: request → model/assistant → tool(s) → database/API → answer.

### Minimum evaluation requirements

1. Define **at least three representative test scenarios** for the first working feature.
2. Include a **normal successful task**, a **database retrieval/filtering task**, and an **edge or failure case** such as no matching data, invalid input, or a temporarily unavailable service. A case may cover more than one category.
3. Include evidence of at least one scenario that exercises external API use.
4. Record each input, expected behavior, observed behavior, and whether the outcome met the expectation.
5. Check answers against the **actual database records or API results**, not just whether the output sounds plausible.
6. Summarize the main strengths, failures, limitations, and one improvement to prioritize next.

A few well-chosen manual runs with recorded outcomes are acceptable for Sprint 1. If a model response varies between runs, note that variation rather than treating one successful response as proof of reliability.

### Evaluation record template

Copy the following table into `docs/SPRINT_1_EVALUATION.md` and replace the examples with scenarios relevant to your project.

| ID | User request / condition | Expected behavior | Actual behavior and tool/data evidence | Result |
|---|---|---|---|---|
| E-01 | `[normal user request]` | `[useful, correct result]` | `[observed answer; tool invoked; source checked]` | Pass / Partial / Fail |
| E-02 | `[request with a database filter]` | `[only matching records]` | `[records returned; comparison to stored data]` | Pass / Partial / Fail |
| E-03 | `[no match, invalid input, or failure]` | `[clear, honest handling]` | `[observed behavior]` | Pass / Partial / Fail |
| E-04* | `[request external API]` | `[validated API result or appropriate failure]` | `[observed behavior and response evidence]` | Pass / Partial / Fail |

**Short evaluation summary** (complete in your own words):

- **Scenarios completed:** `[number]`; **passed:** `[number]`; **partial/failed:** `[number]`.
- **What worked reliably:** `[observations with evidence]`.
- **Most important failure or limitation:** `[what happened, and likely reason]`.
- **What we would change or test next:** `[specific next action]`.

Evaluation should consider whether the assistant selected an appropriate tool, used valid arguments, returned facts grounded in source data, and handled unexpected results safely. 

### Assistant evaluation versus function unit testing

- **Assistant evaluation (required):** Does the whole system perform the intended task correctly?
- **Function unit tests (optional now; required in Sprint 2):** Does an individual function behave correctly for controlled inputs, including errors and edge cases?

Writing unit tests in Sprint 1 is encouraged when useful, but it **does not replace** the required end-to-end assistant evaluation.

## 5. Scrum artifacts and process evidence

The team uses Scrum as a lightweight framework for making work visible and reviewing progress. The following must be reflected in the repository and in the presentation.

### Roles

- **Product Owner:** helps clarify intended value and backlog priority.
- **Scrum Master:** supports the process and helps make impediments visible.
- **Developers:** **all team members**, including the Product Owner and Scrum Master, contribute to implementation, integration, testing, and understanding the product.

Record who took the Product Owner and Scrum Master responsibilities, and explain briefly how technical work was shared.

### Artifacts

- **Product Backlog:** the evolving, ordered list of possible product work.
- **Sprint Backlog:** the Sprint Goal, selected items, and tasks for Sprint 1.
- **Increment:** the integrated, working application delivered by the end of the sprint.

The Product Vision and Definition of Done support these Scrum artifacts. Acceptance criteria should describe outcomes that can be checked.

### Scrum events (often called ceremonies)

Document how the team actually conducted or adapted these activities:

| Event | What to record |
|---|---|
| **Sprint Planning** | Sprint Goal, selected work, key scope decisions. |
| **Daily Scrum / regular progress check-ins** | How the team synchronized, surfaced obstacles, and adjusted plans. State the actual frequency. |
| **Sprint Review** | Working increment shown, completed vs unfinished items, feedback, backlog changes. |
| **Sprint Retrospective** | One thing that helped the team, one problem in the way of working, and one specific improvement for the next sprint. |


### Sprint Backlog example

| Product Backlog item | Sprint task | Owner(s) | Estimate | Status | Acceptance evidence |
|---|---|---|---|---|---|
| PBI-01 | `[implement tool-backed user task]` | `[name(s)]` | `[size/time]` | Done / In progress / Blocked | `[test/demo/link]` |
| PBI-02 | `[integrate database filtering]` | `[name(s)]` | `[size/time]` | Done / In progress / Blocked | `[test/demo/link]` |
| PBI-03 | `[integrate external API]` | `[name(s)]` | `[size/time]` | Done / In progress / Blocked | `[working call or blocker evidence]` |

## 6. Definition of Done for Sprint 1

Use this checklist when deciding whether an item or the increment is ready for review:

- [ ] The selected user workflow can be run and demonstrated from documented instructions.
- [ ] The implementation meets the relevant acceptance criteria.
- [ ] The assistant and its tools are meaningfully integrated; database access and filtering work for the selected use case.
- [ ] External API integration is demonstrated, **or** incomplete work is explicitly recorded with evidence and next actions.
- [ ] The required assistant evaluation has been completed and saved.
- [ ] Important errors and no-result cases are handled, without invented data.
- [ ] Code and documentation are committed/pushed to the shared repository.
- [ ] No passwords, API keys, private records, or unapproved data are committed.
- [ ] The team can explain and demonstrate what is complete, partial, or blocked.

A feature is **Done** when it can run, meet its acceptance criteria, and have evidence of its behavior. Incomplete work stays visible in the backlog.

## 7. Submission checklist

Before the Sprint Review, confirm:

- [ ] The GitHub repository is shared and accessible to the team.
- [ ] `README.md` provides working setup and run instructions.
- [ ] Product Vision, Product Backlog, and Sprint 1 Backlog are available.
- [ ] The current assistant and integrated tool/database workflow can be demonstrated.
- [ ] API integration status and evidence are clear.
- [ ] At least three recorded **assistant-level** evaluation scenarios are included.
- [ ] Sprint Review and retrospective notes are available.
- [ ] Presentation Slides.
- [ ] All group members are prepared for the Jigsaw presentation and technical questions.

**Submit:** the **GitHub repository link** and the **presentation slides** in OMA. 

---

**Sprint 1 guiding question:** *Can a user complete one meaningful task using the assistant, and can your team show evidence that the result is correct?*
