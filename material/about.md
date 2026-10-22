# Neural Network Project:


This course runs in parallel with the **Neural Networks** course. While the Neural Networks course focuses on understanding, training, and evaluating neural networks, this project course focuses on **engineering neural-network-based systems into useful AI products**.

Over eight weeks, you will work in a team to design, build, test, and present a **functional AI assistant or AI agent** that addresses a real-world problem of your choosing.

> An AI assistant primarily helps a user with a task. An AI agent can also decide which actions to take, use tools, observe the results, and continue toward a goal. However, they are not mutually exclusive. An AI assistant can be implemented as an agent, and many products described as agents are really assistants with tool-calling capabilities.

Your goal is to build an application that can understand requests, use information, interact with tools and external services, and perform meaningful actions.

The central question is:

**How do we turn an AI idea into a working, useful, and evaluated product?**

The course is practical and project-driven. You will experiment, solve engineering problems, learn from failures, and improve your product through three development sprints. 

## Mindset

Building AI products is not about using as many models, libraries, or frameworks as possible.

It is about understanding the problem, choosing appropriate technologies, testing assumptions, evaluating alternatives, and explaining your engineering decisions. A simpler, reliable single-agent solution may be better than a more complicated multi-agent system.

The emphasis is on **learning by doing**. Morning sessions introduce ideas and demonstrate techniques; afternoon sessions give you time to apply them to your own project.

You are encouraged to explore unfamiliar tools, discuss alternatives with your team, ask questions, and learn from mistakes. 

## Course Description

In this course, you will apply the AI and software engineering process from **problem analysis and data acquisition to experimentation, implementation, evaluation, and product delivery**.

The main focus is the engineering of **LLM-powered AI assistants and agents**. You will learn to give language models access to tools, databases, external APIs, and relevant information, and to organize these capabilities into useful applications.

The main topics include:

- Agent fundamentals, reasoning, and tool use
- LangChain and tool/function calling
- External APIs, databases, and user interfaces
- Model Context Protocol (MCP) clients and servers
- Retrieval-Augmented Generation (RAG)
- Single-agent and multi-agent architectures
- Fine-tuning and comparison of model alternatives
- Evaluation, reliability, security, and responsible AI

**Evaluation is part of every week**, not only the final presentation. You will investigate what succeeds, what fails, and how changes to your system affect its performance.

The course uses **LangChain as the default agent framework**. Teams may choose LangGraph for more explicit orchestration, including multi-agent workflows, if all team members agree. You will also encounter research examples such as [**Toolformer**](https://arxiv.org/abs/2302.04761) and [**Gorilla**](https://gorilla.cs.berkeley.edu/), which help explain how language models learn to select and use tools; these are topics for investigation, not required development frameworks.

Alongside our own demonstrations and exercises, selected concepts and learning materials will be adapted from the following courses:
- Carnegie Mellon University's [11-768: AI Agents](https://www.cmu-agents.com/) 
- Stanford's [CS 329Z: Engineering AI Agents](https://cs329z.stanford.edu/)

## Learning Outcomes

The official learning outcome for the project is:

> The student applies neural networks to solve a real-world problem. This involves analysing the problem domain, acquiring and exploring data, searching for, experimenting with, and evaluating solution alternatives, implementation and validation of the chosen solution, building data processing pipelines, and deploying the solution.

By the end of this course, you should be able to:

- Analyse a real-world problem and define useful requirements for an AI product.
- Acquire, prepare, and manage the data and external resources needed by your application.
- Design and implement an AI assistant or agent using suitable models, tools, and software libraries.
- Connect an agent to databases, external APIs, MCP services, and retrieval systems where appropriate.
- Experiment with and compare technical alternatives, including different models, agent architectures, and adaptation approaches.
- Create and use evaluations to measure functionality, reliability, and the quality of AI-assisted actions.
- Identify important privacy, security, and responsible-AI risks and apply appropriate safeguards.
- Develop, test, document, deploy, and demonstrate a working AI product as a team.
- Explain the implementation, evaluation results, trade-offs, and your individual contribution.

## Teaching Methods

The course combines guided learning with independent team development.

- **Morning, 09:00–12:00:** Short explanations, practical demonstrations, and guided hands-on exercises introducing technologies you can use in your project.
- **Afternoon, 13:00–16:00:** Three hours for team projects, Scrum activities, coding marathons, sprint planning, reviews, and implementation.

The morning and afternoon sessions are connected: the morning introduces a capability, and the afternoon provides an opportunity to apply it.

The course follows an **iterative and incremental Scrum-based approach**. Your team will develop successive versions of its product, and make improvements.

## Course Schedule

The following schedule is preliminary and may be adjusted to support project progress.

| Week | Morning: concepts and demonstrations | Afternoon: project activities |
|---|---|---|
| **1** | Course introduction; LangChain; tool calling. Research connection: Toolformer. | Project brainstorming and **Sprint 1 planning**. Create a Product Backlog and Sprint 1 Backlog in GitHub. |
| **2** | LangChain; External APIs; Streamlit review; evaluation. Research connection: Gorilla. | **Coding Marathon 1:** tools, LangChain, and external APIs. |
| **3** | MCP clients with LangChain; evaluation. | **Sprint 1 Review/Presentation** and **Sprint 2 planning**. Refine backlogs for the next product version. |
| **4** | MCP servers with LangChain; evaluation. | **Coding Marathon 2:** MCP client/server integration. |
| **5** | RAG with LangChain; evaluation. | **Sprint 2 Review/Presentation** and **Sprint 3 planning**. Plan retrieval and investigate multi-agent possibilities. |
| **6** | Multi-agent systems with LangChain; evaluation. | Team project work. Compare single-agent and multi-agent approaches for your problem. |
| **7** | Fine-tuning; comparison with closed models such as Gemini; Security and reliability. | **Coding Marathon 3:** Tools, MCP, RAG and multi-agent workflows. |
| **8** |  **Sprint 3 Review/Presentation**, final product demonstration, and project evaluation. | **Sprint 3 Presentation...** |


### Sprint Milestones

Each sprint should deliver a **working improvement** to your product:

- **Sprint 1: Week 3:** A first usable version with a user interface, a small Qwen model, tools, a database, and external API integration.
- **Sprint 2: Week 5:** An improved version with MCP integration, additional functionality, and stronger evaluation.
- **Sprint 3: Week 8:** An integrated product with MCP, RAG, investigation of single-agent versus multi-agent designs, security considerations, and systematic evaluation. Use multi-agent collaboration when it provides a clear benefit rather than adding complexity for its own sake.

At each sprint planning session, maintain your **Product Backlog using DEEP principles** and write **Sprint Backlog user stories using INVEST principles**. Record sprint goals, progress, and deliverables in GitHub.

### Coding Marathons

Three coding marathons take place in **Weeks 2, 4, and 7**. You will work in pairs with another member of your project team.

- **Coding Marathon 1:** LangChain, tool calling, and external APIs.
- **Coding Marathon 2:** MCP client/server integration.
- **Coding Marathon 3:** tools, MCP, RAG, and multi-agent workflows.

These sessions are opportunities to practise implementation, experimentation, debugging, and collaboration while producing working solutions.

## Course Mechanics and Teamwork

You will manage your project using **Scrum**. This includes Product and Sprint Backlogs, Sprint Goals, user stories, source code, reviews, tests, and documentation.

The project should develop through several increasingly capable versions. Teams should divide work fairly, integrate their contributions regularly, and be prepared to explain both team-level decisions and individual work.

You are expected to continue project development and preparation **outside scheduled class sessions**. In-class time supports the project but is not the full time available for completing it.

**Evaluation throughout the project:** Begin with a small set of representative test cases in Week 1. As your application evolves, improve the tests and collect evidence about task completion, tool and API behavior, retrieval quality, reliability, and security. Use the results to decide what to change next.

## Technologies and Optional Investigations

The main development environment is **Google Colab**, using small Qwen models such as **Qwen3.5-2B or Qwen3.5-4B**. You will also use Python, GitHub, LangChain, and suitable libraries for interfaces, data storage, and evaluation.

Depending on your project, you may use Streamlit, external APIs, retrieval systems, and MCP clients and servers.

Interested teams may investigate additional tools and approaches, including:

- [Google Colab integration with VS Code](https://github.com/googlecolab/colab-vscode) 
- [Colab CLI](https://github.com/googlecolab/google-colab-cli)
- Alternative Qwen models and quantization techniques
- [Experimental model compression for larger models](https://timdettmers.com/2026/09/21/dlab-open-source-week/)
- LangGraph for more explicit agent and multi-agent orchestration
- Tool-use research such as [Toolformer](https://arxiv.org/abs/2302.04761) and [Gorilla](https://gorilla.cs.berkeley.edu/)

These investigations are **optional** and should support a clear project need or meaningful technical comparison.

## Course Workload

The course is worth **5 ECTS credits**, corresponding to approximately **135 hours of student work** (27 hours per ECTS credit).

Over eight weeks, this is approximately **17 hours per week**:

- **Classroom sessions:** 6 hours per week
- **Autonomous work:** approximately 11 hours per week, including development, experimentation, testing, team coordination, documentation, and preparation

If your team is consistently struggling to make progress or spending substantially more time than expected, contact the teacher early so that the scope and approach can be discussed.

## Assessment

Assessment combines working project increments, practical coding skills, teamwork, feedback, and understanding of your own implementation.

| Component | Weight |
|---|---:|
| Sprint 1 | 10% |
| Sprint 2 | 10% |
| Sprint 3 | 15% |
| Coding Marathon 1 | 10% |
| Coding Marathon 2 | 10% |
| Coding Marathon 3 | 15% |
| Course Feedback | 7% |
| Project Exam | 23% |
| **Total** | **100%** |

The final Sprint Review takes place in **Week 8**. The **Project Exam** is held on a separate day, alongside examination arrangements for the parallel Neural Networks course.

### Overall Grade Thresholds

| Grade | Required percentage |
|---|---:|
| **5** | 93% |
| **4** | 83% |
| **3** | 75% |
| **2** | 67% |
| **1** | 61% |

The course is designed to give everyone an opportunity to succeed through consistent effort, active participation, sound technical understanding, and meaningful contributions to the team project.

## Principles for Using AI

You may use AI coding assistants such as ChatGPT, Codex, and Claude to support your learning and project development.

However:

- **Understand your code:** You should be able to explain, modify, and debug the implementation you submit.
- **Verify results:** Review, run, test, and evaluate AI-generated suggestions; do not assume they are correct.
- **Learn actively:** Use AI to explore ideas, understand unfamiliar tools, and compare alternatives, not as a substitute for your own understanding.
- **Take responsibility:** Be able to explain architectural choices and your individual contributions to the project.
- **Build responsibly:** Consider privacy, security, bias, copyright, safety, and ethical implications when working with data, models, and external services.

**You remain responsible for the code, evaluations, and decisions in your submitted work.**

## Learning Materials

The main recommended book is **[AI Engineering](https://www.oreilly.com/library/view/ai-engineering/9781098166298/) by Chip Huyen**.

Additional materials will include lecture notes, Colab notebooks, demonstrations, code examples, documentation, and coding exercises. We will draw on selected material from [CMU 11-768: AI Agents](https://www.cmu-agents.com/) and Stanford's [CS 329Z: Engineering AI Agents](https://cs329z.stanford.edu/).

There is no expectation that you will master every model or framework. You are encouraged to investigate technologies and research papers relevant to your project and to consult official documentation as needed.

## Where to Go Next

By the end of the course, your team should have a working AI product and the ability to explain **what it does, how it works, how you evaluated it, and why you made your engineering choices**.

The goal is not to showcase the largest number of AI features. It is to develop the habits needed to turn an idea into a useful product: **build, test, learn, improve, and deliver**.
