# Lecture companion: Building an MCP application in five phases


| Phase | New element | What stays unchanged |
|---|---|---|
| **1. Math + FastMCP** | A callable MCP tool and a standalone client | Python performs the calculation |
| **2. Trip + FastMCP** | Resources and prompts alongside a tool | A client discovers and requests server capabilities |
| **3. Trip + official MCP SDK** | A second server library and two client libraries | The MCP operations and Trip behavior |
| **4. Trip + LangChain, no LLM** | Tools, documents, and prompt templates in LangChain | The official-SDK Trip server |
| **5. Trip + LangChain + LLM** | Model-assisted tool selection and context-driven answers | The same underlying MCP capabilities |


**Terminology checkpoint:** 

- MCP is the *protocol*. 
- `fastmcp` and `mcp` are *Python libraries* that implement it. 
- In the architecture, **data-layer operations** (tools, resources, prompts, and MCP request/response messages) are distinct from the **transport layer** (stdio or Streamable HTTP). HTTP and stdio are transports—not competing MCP protocols.

---

## Phase 1: Math MCP server and client with FastMCP (tools)


### Diagram 1.1: MCP standard versus the library selected for this phase

```mermaid
flowchart TB
    standard["MCP: shared protocol"]:::input
    fast["Standalone FastMCP library"]:::process
    sdk["Official MCP Python SDK"]:::process
    math["Phase 1: Math server and client"]:::output
    later["Phase 3: alternative implementation"]:::output

    standard -->|"Implemented by"| fast
    standard -->|"Implemented by"| sdk
    fast --> math
    sdk --> later

    classDef input fill:#e1f5fe,stroke:#0288d1,color:#15324a;
    classDef process fill:#fff3e0,stroke:#ef6c00,color:#4b2b0a;
    classDef output fill:#e8f5e9,stroke:#388e3c,color:#17391e;
```


- Phase 1 uses `from fastmcp import FastMCP` on the server and `from fastmcp import Client` on the client.
- The official SDK is another implementation of the same MCP standard; it is introduced explicitly in Phase 3.
- `fastmcp` a different protocol from MCP, or a library implementing MCP?

### Diagram 1.2: Discover and call the calculator through a separate server process

```mermaid
sequenceDiagram
    autonumber
    participant N as Notebook
    participant C as FastMCP client
    participant S as Math MCP server
    participant F as safe_calculator function

    N->>C: Open stdio client connection
    C->>S: Start server subprocess and initialize MCP
    N->>C: list_tools()
    C->>S: tools/list
    S-->>C: safe_calculator schema
    C-->>N: Tool metadata
    N->>C: call_tool(expression)
    C->>S: tools/call
    S->>F: Evaluate requested expression
    F-->>S: Result or error text
    S-->>C: MCP tool result
    C-->>N: Display result.data
```


- Discovery (`list_tools`) describes a tool but does not execute it.
- Invocation (`call_tool`) sends arguments over MCP; the calculation runs in the **server process**, not in the client notebook.
- The notebook's `safe_calculator` currently uses Python `eval()`. Despite its name, this is **not a safe general-purpose expression parser**; it is a teaching prototype, not a production security pattern.
- At which step does the calculation actually happen?

### Diagram 1.3: The two architectural layers: capability messages versus transport

```mermaid
flowchart TB
    subgraph data["MCP data layer: meaning of the messages"]
        req["tools/list and tools/call"]:::input
        wire["MCP requests and responses (JSON-RPC)"]:::process
        req --> wire
    end

    subgraph transport["Transport layer: where messages travel"]
        stdio["stdio: subprocess pipes (used in Phase 1)"]:::process
        http["Streamable HTTP: server endpoint (alternative)"]:::process
    end

    server["Same MCP tool contract"]:::output
    wire --> stdio
    wire --> http
    stdio --> server
    http --> server

    classDef input fill:#e1f5fe,stroke:#0288d1,color:#15324a;
    classDef process fill:#fff3e0,stroke:#ef6c00,color:#4b2b0a;
    classDef output fill:#e8f5e9,stroke:#388e3c,color:#17391e;
```


- **Data layer:** The client asks to list or call a tool; MCP defines what those operations mean.
- **Transport layer:** The notebook uses stdio to communicate with a launched process. A remotely hosted server can use Streamable HTTP instead.
- stdio carries MCP messages through stdin/stdout; the server should send diagnostics to stderr rather than mixing them with protocol output.
- What changes if the same server capability is offered over HTTP instead of stdio?

**Phase 1 takeaway:** A local Python function becomes a discoverable MCP tool; the client communicates through the protocol without importing the function.

---

## Phase 2: Trip MCP server and client with FastMCP (tools, resources, prompts)


### Diagram 2.1: One Trip server exposes three different capabilities

```mermaid
flowchart LR
    client["FastMCP client"]:::input

    subgraph trip["FastMCP TripUtilities server"]
        tool["Tool: estimate_daily_budget"]:::process
        resource["Resource: trip://notes/helsinki"]:::process
        prompt["Prompt: outline_day_trip"]:::process
    end

    calc["Computed budget"]:::output
    notes["Illustrative Helsinki notes"]:::output
    request["Rendered itinerary instruction"]:::output

    client -->|"call_tool"| tool
    client -->|"read_resource"| resource
    client -->|"get_prompt"| prompt
    tool --> calc
    resource --> notes
    prompt --> request

    classDef input fill:#e1f5fe,stroke:#0288d1,color:#15324a;
    classDef process fill:#fff3e0,stroke:#ef6c00,color:#4b2b0a;
    classDef output fill:#e8f5e9,stroke:#388e3c,color:#17391e;
```

- **Tool:** Performs work; `estimate_daily_budget(days, daily_cost)` calculates a result.
- **Resource:** Provides content identified by a URI; Helsinki notes are illustrative, **not live travel data**.
- **Prompt:** Produces reusable instructions from arguments; `outline_day_trip(city, focus)` renders a request, but does not itself generate an itinerary.
- Why should the Helsinki notes be a resource rather than a budget-calculation tool?

### Diagram 2.2: Three distinct request patterns over the same MCP connection

```mermaid
sequenceDiagram
    autonumber
    participant N as Notebook
    participant C as FastMCP client
    participant S as TripUtilities MCP server

    N->>C: list_tools()
    C->>S: tools/list
    S-->>C: Budget tool metadata
    N->>C: call_tool(days, daily_cost)
    C->>S: tools/call
    S-->>C: Computed budget

    N->>C: list_resources()
    C->>S: resources/list
    S-->>C: Helsinki resource URI
    N->>C: read_resource(trip://notes/helsinki)
    C->>S: resources/read
    S-->>C: Helsinki notes

    N->>C: list_prompts()
    C->>S: prompts/list
    S-->>C: outline_day_trip template metadata
    N->>C: get_prompt(city, focus)
    C->>S: prompts/get
    S-->>C: Rendered prompt messages
```


- `list_*` discovers capability metadata; `call_tool`, `read_resource`, and `get_prompt` request the corresponding results.
- The *same client connection and stdio transport* support all three operations.
- The client chooses each request explicitly. No LLM makes decisions in this phase.
- Which operation executes code, which reads data, and which prepares instructions?

### Diagram 2.3: The data layer grows; the transport does not change

```mermaid
flowchart TB
    subgraph data["MCP data layer: Phase 2"]
        t["Tools: list and call"]:::input
        r["Resources: list and read"]:::input
        p["Prompts: list and get"]:::input
        messages["MCP request and response messages"]:::process
        t --> messages
        r --> messages
        p --> messages
    end

    subgraph transport["Transport layer"]
        used["stdio used in the Trip notebook"]:::process
        alternative["Streamable HTTP is another possible transport"]:::process
    end

    server["TripUtilities server"]:::output
    messages --> used
    messages -.->|"Alternative deployment"| alternative
    used --> server
    alternative --> server

    classDef input fill:#e1f5fe,stroke:#0288d1,color:#15324a;
    classDef process fill:#fff3e0,stroke:#ef6c00,color:#4b2b0a;
    classDef output fill:#e8f5e9,stroke:#388e3c,color:#17391e;
```

- Adding resources and prompts expands **what** the client can request; it does not require changing **how** messages travel.
- An HTTP deployment is shown as an architectural alternative, **not as a notebook step**.

**Phase 2 takeaway:** MCP is more than tool calling. A server can expose actions, data, and reusable prompt definitions through the same interface.

---

## Phase 3: Trip MCP server with the official SDK and two client libraries

 
### Diagram 3.1: One official-SDK server, two interoperable clients

```mermaid
flowchart TB
    sdkclient["Official MCP SDK Client"]:::input
    fastclient["FastMCP Client"]:::input
    spec["Same MCP protocol and stdio transport"]:::process
    server["Official SDK MCPServer: TripUtilities"]:::process

    subgraph functions["Exposed server capabilities"]
        tool["Budget tool"]:::output
        resource["Helsinki resource"]:::output
        prompt["Itinerary prompt"]:::output
    end

    sdkclient --> spec
    fastclient --> spec
    spec --> server
    server --> tool
    server --> resource
    server --> prompt

    classDef input fill:#e1f5fe,stroke:#0288d1,color:#15324a;
    classDef process fill:#fff3e0,stroke:#ef6c00,color:#4b2b0a;
    classDef output fill:#e8f5e9,stroke:#388e3c,color:#17391e;
```

- The server changes to `from mcp.server import MCPServer` but still exposes the same tool, resource, and prompt.
- Both test clients use MCP; each launches its own stdio connection to the server program in its test cell.
- Compatibility depends on compatible MCP protocol and transport support, not matching Python imports.
- Why is the server implementation invisible to a client that relies on discovery?

### Diagram 3.2: Comparable operations, different high-level Python result wrappers

```mermaid
flowchart LR
    action["Ask server to list tools"]:::input
    sdk["Official SDK: list_tools()"]:::process
    fast["FastMCP: list_tools()"]:::process
    result1["ListToolsResult.tools"]:::output
    result2["List of Tool objects"]:::output

    action --> sdk
    action --> fast
    sdk --> result1
    fast --> result2

    classDef input fill:#e1f5fe,stroke:#0288d1,color:#15324a;
    classDef process fill:#fff3e0,stroke:#ef6c00,color:#4b2b0a;
    classDef output fill:#e8f5e9,stroke:#388e3c,color:#17391e;
```

- Both clients expose familiar methods such as `list_tools`, `call_tool`, `read_resource`, and `get_prompt`.
- The official SDK uses `response.tools`; the standalone FastMCP client directly returns a tool list in this notebook.
- **Similar protocol behavior does not guarantee identical Python return types**. For tool results, inspect SDK result content/structured content versus the FastMCP convenience property `result.data`.
- Which parts of a client implementation are standardized, and which are library-specific?

### Diagram 3.3: Equivalent tests with either client

```mermaid
sequenceDiagram
    autonumber
    participant N as Test notebook
    participant C as Selected MCP client library
    participant S as Official SDK Trip server

    N->>C: Open stdio connection
    C->>S: Initialize MCP session
    N->>C: list_tools, list_resources, list_prompts
    C->>S: MCP discovery requests
    S-->>C: Advertised capabilities
    C-->>N: Library-specific Python objects
    N->>C: call_tool / read_resource / get_prompt
    C->>S: MCP requests
    S-->>C: Budget / notes / rendered prompt
    C-->>N: Display and compare results
```

- Run the diagram's sequence **twice**: once with `mcp.Client`, once with `fastmcp.Client`.
- Comparing the same server behavior isolates differences in the clients rather than differences in underlying business logic.
- MCP interoperability is a protocol property; the Python client experience may differ without changing server capabilities.

---

## Phase 4: Trip MCP server with LangChain, without an LLM


### Diagram 4.1: The three MCP capabilities enter LangChain differently

```mermaid
flowchart LR
    server["Official SDK Trip server"]:::input

    toolpath["MCPAdapter: list_tools()"]:::process
    respath["MCP SDK Client: read_resource()"]:::process
    prompath["MCP SDK Client: get_prompt()"]:::process

    tool["LangChain Tool: ainvoke()"]:::output
    doc["LangChain Document"]:::output
    tmpl["LangChain ChatPromptTemplate"]:::output

    server -->|"Tool discovery"| toolpath
    server -->|"Resource read"| respath
    server -->|"Prompt retrieval"| prompath
    toolpath --> tool
    respath -->|"Wrap text"| doc
    prompath -->|"Wrap messages"| tmpl

    classDef input fill:#e1f5fe,stroke:#0288d1,color:#15324a;
    classDef process fill:#fff3e0,stroke:#ef6c00,color:#4b2b0a;
    classDef output fill:#e8f5e9,stroke:#388e3c,color:#17391e;
```

- `MCPAdapter` automatically adapts **tools** in the notebook.
- The official SDK client explicitly retrieves **resources** and **prompts**; notebook code wraps their contents in `Document` and `ChatPromptTemplate` objects.
- A LangChain `Document` is context, not an executable MCP tool; a rendered prompt template is instructions, not a server operation performed by the model.
- Why do tools remain callable while resource and prompt conversions produce in-memory objects?

### Diagram 4.2: Direct invocation proves the adapter works without an LLM

```mermaid
sequenceDiagram
    autonumber
    participant N as Notebook test code
    participant A as LangChain MCPAdapter
    participant S as Official SDK Trip server

    N->>A: Open adapter connection
    N->>A: list_tools()
    A->>S: tools/list over MCP
    S-->>A: estimate_daily_budget schema
    A-->>N: LangChain tool object
    N->>A: budget_tool.ainvoke(days=5, daily_cost=120)
    A->>S: tools/call with arguments
    S-->>A: 600.0
    A-->>N: LangChain tool result
```

- The **notebook** selects the tool and supplies `days=5` and `daily_cost=120`.
- The adapter handles the MCP connection and conversion; **no LLM** chooses the tool or arguments.
- Keep the connection open during invocation; standalone callable wrappers need a live connection to reach the server.
- If this test succeeds, which potential source of error has been ruled out before adding an LLM?

### Diagram 4.3: Resources and prompts are retrieved, then represented locally

```mermaid
flowchart TB
    client["Official SDK MCP Client"]:::input
    resource["Read Helsinki notes"]:::process
    prompt["Render outline_day_trip(Helsinki, museums)"]:::process
    document["Document: page_content + metadata"]:::output
    messages["ChatPromptTemplate: rendered messages"]:::output
    inspect["Inspect in notebook (no LLM)"]:::input

    client --> resource
    client --> prompt
    resource --> document
    prompt --> messages
    document --> inspect
    messages --> inspect

    classDef input fill:#e1f5fe,stroke:#0288d1,color:#15324a;
    classDef process fill:#fff3e0,stroke:#ef6c00,color:#4b2b0a;
    classDef output fill:#e8f5e9,stroke:#388e3c,color:#17391e;
```

- Retrieved content is a **snapshot at retrieval time**, not a live tool proxy.
- The MCP prompt is already rendered with its arguments before it becomes a LangChain template in this notebook; changing its city/focus requires a new MCP prompt request.

**Phase 4 takeaway:** MCP integration should be tested in isolation: discovery, conversion, direct execution, and content inspection all work without model-generated decisions.

---

## Phase 5: Trip MCP server with LangChain and an LLM


### Diagram 5.1: Tool workflow: the model proposes; LangChain and MCP execute

```mermaid
sequenceDiagram
    autonumber
    participant U as User
    participant A as LangChain agent
    participant M as Groq chat model
    participant C as MCPAdapter client
    participant S as Trip MCP server

    U->>A: Ask for a 5-day trip budget
    A->>M: User request + budget tool schema
    M-->>A: Tool call: days=5, daily_cost=120
    A->>C: Invoke selected LangChain tool
    C->>S: MCP tools/call
    S-->>C: Computed budget: 600.0
    C-->>A: Tool result
    A->>M: Tool result as data
    M-->>A: Final explanation
    A-->>U: Grounded answer
```

- `create_agent(model=chat_model, tools=...)` lets the model **request** a tool call.
- LangChain and the MCP client execute the request; the model does not run the Python function itself.
- A model may choose not to call a tool or may produce bad arguments. Inspect the actual message trace to determine what happened.
- Which participant chose the arguments, and which participant executed the calculation?

### Diagram 5.2: Resource workflow: retrieve context before the LLM call

```mermaid
flowchart LR
    resource["MCP resource: Helsinki notes"]:::input
    sdk["Official SDK Client: read_resource"]:::process
    doc["LangChain Document"]:::output
    template["ChatPromptTemplate: context + question"]:::process
    model["Groq chat model"]:::process
    answer["Answer using supplied notes"]:::output

    resource --> sdk
    sdk --> doc
    doc --> template
    template --> model
    model --> answer

    classDef input fill:#e1f5fe,stroke:#0288d1,color:#15324a;
    classDef process fill:#fff3e0,stroke:#ef6c00,color:#4b2b0a;
    classDef output fill:#e8f5e9,stroke:#388e3c,color:#17391e;
```

- The **application**, not the LLM, reads the MCP resource and puts the content into the prompt.
- The server notes are explicitly illustrative. The answer should not present them as proof of live operating hours, weather, route times, or availability.
- This is context-augmented generation, not dynamic MCP resource selection by the model.
- What would need to change for the application to retrieve only relevant resources on demand?

### Diagram 5.3: Prompt workflow: render MCP instructions, then call the model

```mermaid
flowchart LR
    args["city=Helsinki, focus=museums"]:::input
    sdk["Official SDK Client: get_prompt"]:::process
    rendered["MCP rendered prompt messages"]:::output
    lc["LangChain ChatPromptTemplate"]:::process
    model["Groq chat model"]:::process
    itinerary["Generated itinerary draft"]:::output

    args --> sdk
    sdk --> rendered
    rendered --> lc
    lc --> model
    model --> itinerary

    classDef input fill:#e1f5fe,stroke:#0288d1,color:#15324a;
    classDef process fill:#fff3e0,stroke:#ef6c00,color:#4b2b0a;
    classDef output fill:#e8f5e9,stroke:#388e3c,color:#17391e;
```

- The MCP prompt provides a **request template**, not verified travel facts and not a tool execution.
- The notebook converts the rendered MCP messages into LangChain messages and calls `trip_prompt | chat_model`.
- The resulting itinerary can be plausible but unverified; the example provides **no live verification tools** for opening hours or travel times.
- Why does a well-worded prompt alone not establish that travel details have been checked?

**Phase 5 takeaway:** The agent may execute tools dynamically, while the resource and prompt chains use content fetched beforehand. In all three paths, results must be inspected for correctness and grounding.

---

## Cross-phase recap: what changed, and what remained the same?

| Concept | Phase 1 | Phase 2 | Phase 3 | Phase 4 | Phase 5 |
|---|---|---|---|---|---|
| Server library | FastMCP | FastMCP | Official MCP SDK | Official MCP SDK | Official MCP SDK |
| MCP capabilities | Tool | Tool, resource, prompt | Same three | Same three | Same three |
| Client interface | FastMCP | FastMCP | Official SDK and FastMCP | LangChain MCPAdapter for tools; SDK Client for content | Same integration paths |
| Model involved? | No | No | No | No | Yes (Groq) |
| Who selects requests? | Notebook | Notebook | Notebook | Notebook | Agent/model for tools; application for resources/prompts |
| Transport shown in notebooks | stdio | stdio | stdio | stdio | stdio |
