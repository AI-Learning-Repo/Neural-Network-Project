# Building MCP Servers Gradually: Phase 1: Math MCP Server and Client with FastMCP


<!-- > **Goal:** Build one MCP server that exposes a Python calculator function, and test it using an MCP client. Later phases add resources, prompts, the official MCP SDK, LangChain, and an LLM.
> 
> Each phase in this series runs in its own dedicated notebook, so execute this phase in a new colab. -->


## The five-phase learning journey

- Phase 1:Create an MCP **tool**, server and client with FastMCP 
- Phase 2: Extend a FastMCP server with **tools, resources and prompts** 
- Phase 3: Recreate the Trip server using the **official MCP SDK v2**; compare SDK and FastMCP clients 
- Phase 4: Connect MCP to **LangChain without an LLM** 
- Phase 5: Add a **Groq LLM** and compare agent tool use with resource/prompt chains 

**Learning strategy:** First verify each MCP capability with a client. Then verify the LangChain integration. Only after those tests work should you add an LLM.

## What you will learn

By the end of Phase 1 you should be able to:

1. Explain an **MCP server**, **MCP client**, **tool**, and **transport**.
2. Register a Python function as an MCP tool with `@mcp.tool()`.
3. Start and communicate with a local server using **stdio**.
4. Discover tools using `list_tools()` and call one using `call_tool()`.
5. Explain why you do **not** need an LLM to test an MCP server.

---

## 1. The main idea

With normal Python, a program may call a function directly:

```python
answer = safe_calculator("8273 * 1821")
```

With the **Model Context Protocol (MCP)**, a *different program* can discover and invoke the function through a standardized interface. That second program does not need to import the function.

<!-- ```text
FastMCP client (a notebook cell)
    |
    | 1. Connect using stdio
    | 2. Ask: list_tools()
    | 3. Ask: call_tool("safe_calculator", ...)
    v
FastMCP server (math_server.py)
    |
    | Registered tool: safe_calculator
    v
Python calculation → MCP result → client
``` -->

| Concept | In our exercise |
|---|---|
| **Server** | `math_server.py`, the program that makes functionality available |
| **Tool** | `safe_calculator(expression)`, a callable function registered with the server |
| **Client** | Notebook code that discovers and calls the tool |
| **Transport** | **stdio**, the channel between the client and the server subprocess |
| **FastMCP** | The Python framework used for both server and client in Phase 1 |

**Important:** There is **no LLM** in this architecture. We are testing that MCP communication itself works.

## 2. Setup

Some notebook stderr streams do not provide the file descriptor needed by a subprocess launcher. The following cell redirects diagnostics before the first MCP import. It prepares the later stdio examples; it does not start a server.

```python
import sys
sys.stderr = open("mcp_server.log", "w")
```

Install the client dependencies

```python
%pip install -q fastmcp
```


## 3. Build the server

We want an MCP server called `Math` with one tool, `safe_calculator`.

**Notebook cell: create `math_server.py`:**

> `%%writefile` must be **the first line** of its notebook cell. It creates the script on disk; it does not launch the server.

```python
%%writefile math_server.py
from fastmcp import FastMCP

mcp = FastMCP("Math")

@mcp.tool()
def safe_calculator(expression: str) -> str:
    """Evaluate a basic math expression, such as '8273 * 1821'.
    Supports +, -, *, /, parentheses, and numbers only."""
    try:
        result = eval(expression, {"__builtins__": None}, {})
        return str(result)
    except Exception as e:
        return f"Error: {e}"

if __name__ == "__main__":
    mcp.run(transport="stdio")
```

### Read the code

- `FastMCP("Math")` creates a named MCP server.
- `@mcp.tool()` **registers** the function with the server so clients can discover it.
- `expression: str` declares the tool input; type annotations help clients understand its schema.
- The docstring describes the intended purpose of the tool.
- `mcp.run(transport="stdio")` runs the server using standard input/output.
- The `if __name__ == "__main__"` guard starts the server only when the file is executed as a program.

<!-- **Security warning:** The function name safe_calculator is illustrative only. Python's `eval()` is **not a safe expression parser**, even when `__builtins__` is set to `None`, and the stated operator restrictions are **not enforced**. This is a local demonstration using **trusted arithmetic expressions only**. Do **not** expose this implementation to untrusted inputs or an LLM. A production version would use an AST allowlist or a dedicated restricted math parser. -->

**Think:** What changes if you remove `@mcp.tool()`? The Python function still exists, but it is not advertised as an MCP tool.

## 4. Build the client

The client tells FastMCP how to launch `math_server.py`, discovers the available tool, and calls it.

**Notebook cell: create and use a FastMCP client:**

```python
import sys
from pathlib import Path

from fastmcp import Client
from fastmcp.client.transports import StdioTransport

transport = StdioTransport(
    command=sys.executable,
    args=[str(Path("math_server.py").resolve())]
)

client = Client(transport)

async with client:
    # Discover tools registered by the server
    tools = await client.list_tools()
    print("Available tools:", [t.name for t in tools])

    # Invoke the calculator through MCP
    result = await client.call_tool(
        "safe_calculator",
        {"expression": "8273 * 1821"}
    )

    print("Result:", result.data)
```

### Understand each step

| Client code | Meaning |
|---|---|
| `sys.executable` | Use the current Python interpreter to launch the server |
| `Path(...).resolve()` | Pass the absolute path of the server file |
| `StdioTransport(...)` | Specify the local subprocess communication channel |
| `Client(transport)` | Create a client object |
| `async with client:` | Open the connection and clean it up afterward |
| `await client.list_tools()` | Discover server tools without importing them |
| `await client.call_tool(...)` | Send the tool name and named inputs to the server |
| `result.data` | Read FastMCP's parsed result |

**Expected essential output:**

```text
Available tools: ['safe_calculator']
Result: 15065133
```

The expression `8273 * 1821` equals **15,065,133**.

**What actually happens?** When you enter `async with client`, the client launches the server process, communicates over stdio, and asks it which tools it offers. The tool invocation then runs **inside the server**, not directly inside your notebook’s client code. The result comes back over MCP.

You **do not** need to run `python math_server.py` in another notebook cell. The stdio client launches the server itself; running it directly in a notebook would block while waiting for protocol messages.

## 5. Practice: test the server before adding more layers

### Exercise A: Explore discovery

Inside the existing `async with` block, inspect the tool descriptions:

```python
for tool in tools:
    print("NAME:", tool.name)
    print("DESCRIPTION:", tool.description)
```

**Question:** How does the client know the tool name and description without importing `safe_calculator`?

### Exercise B: Change the input

Call `safe_calculator` with the following **trusted arithmetic expressions**:

| Expression | Expected result |
|---|---:|
| `"12 * (3 + 4)"` | `84` |
| `"(100 / 4) - 7"` | `18.0` |
| `"8 / 2"` | `4.0` |

**Question:** Why do some outputs end in `.0`?

### Exercise C: Observe an error

Try `"8 / 0"`.

The server function **catches exceptions and returns an `"Error: ..."` string**. That is not the same as reporting an MCP tool execution error. Discuss how a real client should distinguish failed operations from successful string results.

### Exercise D: Explain the architecture

Answer each in two or three sentences:

1. What role does `@mcp.tool()` play?
2. Why is `list_tools()` useful before `call_tool()`?
3. What does stdio transport carry?
4. What is the difference between a *direct Python function call* and an *MCP tool call*?
5. Why is it useful to test the server **without** an LLM first?
6. Why is `eval()` unsuitable for a calculator that may receive untrusted input?

## 6. Troubleshooting

| Symptom | Check |
|---|---|
| `ModuleNotFoundError: fastmcp` | Install FastMCP in the environment used to run the notebook and launch the server |
| `math_server.py` not found | Rerun the `%%writefile` cell and check the current working directory |
| `SyntaxError` on `%%writefile` | Put `%%writefile` on the **first line** of the notebook cell |
| Connection errors | Confirm `sys.executable` points to the intended Python environment and that the server uses stdio |
| Tool is not listed | Check the `@mcp.tool()` decorator, save the latest file and reconnect |
| Error appears as ordinary text | The example catches Python errors and returns strings rather than a protocol-level tool error |

<!-- 
## 7. Exit ticket

Before continuing, submit:

- A screenshot or copied output listing `safe_calculator`.
- The result of `8273 * 1821`.
- One new successful arithmetic call.
- A short paragraph distinguishing **server**, **client**, **tool**, and **transport**.
- One sentence explaining why the current calculator implementation is **not secure**. 
-->

---

## 7. What happens in Phases 2–5?

### Phase 2: FastMCP Trip server

You will extend this pattern to **three different MCP capabilities**:

- **Tool:** `estimate_daily_budget(days, daily_cost)` calculates a budget.
- **Resource:** `trip://notes/helsinki` provides illustrative trip-planning notes.
- **Prompt:** `outline_day_trip(city, focus)` provides a reusable itinerary request.

You will test all three with the FastMCP client using `list_tools()`, `read_resource()`, and `get_prompt()`.

### Phase 3: Official MCP SDK v2

You will reimplement the same Trip server with `from mcp.server import MCPServer`. You will test it using both an **official SDK client** and a **FastMCP client**.

The key question: *Can different client and server libraries communicate?* Yes, because they implement the same MCP protocol, provided their versions and transports are compatible. Their Python interfaces and result objects are not always identical.

### Phase 4: MCP + LangChain, without an LLM

You will:

- Use `MCPAdapter` to expose MCP tools as executable LangChain tools.
- Read MCP resources through the official SDK client and wrap text as LangChain `Document` objects.
- Fetch rendered MCP prompts and convert their messages to LangChain `ChatPromptTemplate` objects.

These conversions test the integration **without model-driven behavior**.

### Phase 5: MCP + LangChain + LLM

Finally, you will use Groq:

- A **LangChain agent** allows the LLM to select and invoke the MCP budget tool.
- A **LangChain prompt chain** supplies previously retrieved resource text as context.
- Another **prompt chain** passes a rendered MCP prompt to the model.

The distinction matters: **automatic tool calling by an agent** is different from an application **preloading resource content or a prompt**.

## References

- [FastMCP documentation](https://gofastmcp.com/)
- [Official MCP Python SDK v2](https://py.sdk.modelcontextprotocol.io/v2/)
- [LangChain MCP integration](https://docs.langchain.com/oss/python/langchain/mcp)

<!-- **Main takeaway:** Expose → discover → call → inspect. Test every layer before adding the next one. -->
