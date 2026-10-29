# Activity: From API Calls to LLM Tools

Build API-powered tools for weather and local time, then let a language model request those tools. Python executes the requests; evaluate the final answers against the returned data.

Start with the [activity overview](activity1-step_0.md). Read the companion [theory chapter](./summary.md) alongside the practical steps.

## Part A: Steps 1–4

Complete these steps in order using a **Google Colab CPU runtime**. Each step reuses the functions or tools created earlier.

- **[Step 1: Test standalone API requests](activity1-steps-1-2.md):** Call Geocoding, Weather, and Time.Now directly. Inspect HTTP responses, JSON fields, and units.
- **[Step 2: Wrap API requests in Python functions](activity1-steps-1-2.md#step-2-wrap-each-api-request-in-a-python-function):** Create reusable functions that return structured results.
- **[Step 3: Create and test LangChain tools](activity1-steps-3.md):** Inspect tool names, descriptions, and input schemas, then test the tools without a model.
- **[Step 4: Connect Groq-hosted Qwen](activity1-steps-4.md):** Test hosted generation, connect the tools, and inspect model requests, execution results, and final answers.

**Before Step 4:** Add `GROQ_API_KEY` to Colab Secrets and enable notebook access. Keep the key out of notebook code, outputs, and submitted files. No GPU is needed for Part A.

**Part A evidence:** Preserve runnable code, API/function/tool tests, hosted model/tool traces, and the reflection answers requested in the steps. Check that reported weather and time values agree with the tool results from the same run.

## Part B: Step 5 (optional)

- **[Step 5: Local Qwen3.5-4B, tools, and evaluation](activity1-steps-5.md):** Run Qwen locally, reuse the tools from Part A, inspect generated tool requests, and evaluate the results. Compare with the hosted configuration using equivalent tasks.

Part B is an **optional extension to this activity**. Its exercises, evaluation, and submission checklist apply when completing the extension; they are not required to complete Part A.

Use a **Google Colab GPU runtime** for Part B. Save Part A before switching runtimes, then restore the required packages and source files. Do not rerun Groq requests merely to recreate files. The local model does not require a Groq API key.

**Course-project distinction:** Step 5 is optional in this activity. The course project's required local-versus-Groq model benchmark remains a separate requirement.

<!-- ; see the [sprint deliverables](./03_Sprint_Deliverables.md). -->
