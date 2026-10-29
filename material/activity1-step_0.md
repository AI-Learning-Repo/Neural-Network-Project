# Step 0: About the Activity: From APIs to LLM Tools

```mermaid
flowchart LR
    classDef input fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef process fill:#fff3e0,stroke:#f57c00,stroke-width:2px;
    classDef output fill:#e8f5e9,stroke:#388e3c,stroke-width:2px;

    s1["Step 1: Test APIs"]:::input
    s2["Step 2: Write Python functions"]:::process
    s3["Step 3: Create LangChain tools"]:::process
    s4["Step 4: Connect Groq-hosted Qwen"]:::process
    s5["Step 5: Connect local Qwen and evaluate"]:::output

    s1 --> s2
    s2 --> s3
    s3 --> s4
    s4 --> s5
```

## Activity overview

**Main objective:** Build a small collection of API-powered tools and examine how a language model can request them to obtain current information.

The activity begins with direct HTTP requests. The same requests are then packaged as ordinary Python functions, converted into LangChain tools, and tested with two ways of running a Qwen model: remotely through **Groq** and locally on a **Google Colab GPU**. The final stage evaluates whether the model selects suitable tools, supplies valid inputs, and uses tool results accurately.

The APIs support a simple weather-and-local-time scenario. Their outputs can later contribute to a larger trip-planning application; this activity does **not** yet implement a complete trip planner.

| External API | Purpose | Example input |
|---|---|---|
| [Open-Meteo Geocoding](https://open-meteo.com/en/docs/geocoding-api) | Convert a city name into coordinates and timezone | `Berlin` |
| [Open-Meteo Weather](https://open-meteo.com/en/docs) | Retrieve current temperature, humidity, and wind | Latitude and longitude |
| [Time.Now](https://time.now/developer) | Retrieve local time for a timezone | `Europe/Berlin` |

## The five steps

| Step | Focus | Main result | Model required? |
|---|---|---|---|
| **1: Standalone API requests** | Call each API using `requests.get()`, inspect HTTP responses and JSON | Three working API requests | No |
| **2: Python functions** | Wrap each request in a function with inputs and returned dictionaries | `geocoding_api.py`, `weather_api.py`, `time_api.py` | No |
| **3: LangChain tools only** | Add `@tool`, inspect names/descriptions/input schemas, and test with `.invoke()` | `api_tools.py` and three tested LangChain tools | No |
| **4: Groq-hosted Qwen** | **4A:** Test model access without tools. **4B:** Let Qwen request the existing tools | Model responses and tool-call traces | Yes; hosted by Groq |
| **5: Local Qwen3.5-4B** | **5A:** Set up GPU and test the model. **5B:** Connect the tools. **5C:** Evaluate tool calling and answers | Local model traces and an evaluation table | Yes; runs in Colab |

**Sequence:** Complete the steps in order. Each new step reuses the functions or tools created earlier rather than replacing the API logic.

## Before starting: Google Colab and the Groq API key

**Platform:** Google Colab with Python. A CPU runtime is sufficient for Steps 1–4. Step 5 uses a GPU runtime to load the local Qwen3.5-4B model.

**Groq API key requirement:** An API key is required for **Step 4A and Step 4B**, because those parts call a Qwen model hosted by Groq. A Groq API key is **not required** for Steps 1–3 or for the local model used in Step 5.

### Prepare Groq access before Step 4

1. Open the [GroqCloud Console](https://console.groq.com/) and create an account if needed.
2. Generate an API key in the [Groq API keys page](https://console.groq.com/keys).
3. Open the Google Colab notebook and select the **Secrets** panel (key icon in the left sidebar).
4. Add a secret named **`GROQ_API_KEY`** and save the generated key as its value.
5. **Enable notebook access** using the toggle beside that secret.
6. In Step 4A, run the provided setup code to read the secret securely and check that the Groq client connects.

**Security rule:** Never paste the key directly into notebook code, Markdown, screenshots, printed output, or submitted files. Do not share or commit the key. Groq API requests are subject to account limits and any applicable usage charges.

### Prepare the runtime for each step

- **Steps 1–4:** Start with a CPU runtime; a GPU is not needed for a Groq-hosted model.
- **Step 5:** Save the notebook, then select **Runtime → Change runtime type → GPU** and reconnect. Changing the runtime can terminate the existing session and remove temporary files, installed packages, and Python variables.
- **After the Step 5 runtime change:** Re-execute the preparation and `%%writefile` cells needed to recreate `geocoding_api.py`, `weather_api.py`, `time_api.py`, and `api_tools.py`. If all relevant cells are included in the same notebook in the proper order, **Runtime → Run all** can rebuild the environment, but it also reruns unrelated cells, including any previous Groq requests or model-loading steps. If the work is split across notebooks, restore the required cells in the current GPU session instead.
- **Before testing local tools:** Run the Step 5 file-existence and import checks. Confirm that all four `.py` files are present and that all three LangChain tools import successfully.

Files created using `%%writefile` in the Colab runtime are temporary. Preserve required notebooks and source files in durable storage, such as Google Drive or local downloads.

## Working files and how they connect

```mermaid
flowchart LR
    classDef file fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef wrapper fill:#fff3e0,stroke:#f57c00,stroke-width:2px;
    classDef model fill:#e8f5e9,stroke:#388e3c,stroke-width:2px;

    f1["geocoding_api.py"]:::file
    f2["weather_api.py"]:::file
    f3["time_api.py"]:::file
    tools["api_tools.py"]:::wrapper
    groq["Step 4: Groq-hosted Qwen"]:::model
    local["Step 5: Local Qwen"]:::model

    f1 --> tools
    f2 --> tools
    f3 --> tools
    tools --> groq
    tools --> local
```

The three API files contain the HTTP requests. `api_tools.py` wraps those existing functions for LangChain. The hosted and local model activities reuse **the same three tool definitions**.

## Learning outcomes

At the end of the activity, demonstrate the ability to:

1. Describe the difference between an API request, a Python function, and a LangChain tool.
2. Inspect API responses and return useful fields in a predictable format.
3. Explain how a tool name, description, and input schema help an LLM request an appropriate operation.
4. Distinguish **tool selection by a model** from **tool execution by Python code**.
5. Compare remote model access through Groq with local model inference in Colab.
6. Evaluate tool-calling behavior using execution traces and evidence from API results rather than judging only the final answer.

## Completion evidence

Preserve runnable code, successful output checks, example tool-call traces, and a Step 5 evaluation table. Ensure that submitted notebooks and files contain **no API key values**.

**Start with:** Step 1: Test each API as a standalone request. Read the companion **Theory: From API Calls to LLM Tools** chapter alongside the activity.

## References

- [Open-Meteo Geocoding API](https://open-meteo.com/en/docs/geocoding-api)
- [Open-Meteo Weather API](https://open-meteo.com/en/docs)
- [Time.Now Developer API](https://time.now/developer)
- [LangChain tools](https://docs.langchain.com/oss/python/langchain/tools)
- [Groq API keys](https://console.groq.com/keys)
- [Groq tool-use overview](https://console.groq.com/docs/tool-use/overview)
