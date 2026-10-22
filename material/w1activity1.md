# Activity: Tool Calling and Agentic AI with Local and Hosted LLMs  - Part 1

In this lab, you will build a bookstore assistant using Python, SQLite, LangChain, and Qwen. You will examine how a language model requests tools, how Python validates and executes those requests, and how tool results support a final answer. You will build a multi-step workflow, extend its tools, and evaluate its behavior. Optional activities compare local and hosted models and examine book-cover images.


## Important Notes Before Starting

This lab runs in Google Colab and uses several machine learning libraries, including PyTorch, Transformers, and LangChain. During installation or execution, you may encounter warnings related to optional dependencies, hardware acceleration, or package compatibility. Some warnings are informational; for example, Qwen may use a slower PyTorch implementation when optimized kernels are unavailable. These messages do not necessarily mean that an experiment has failed. **Read the warnings carefully and investigate their meaning.** If a cell produces the expected output, you can generally continue, but dependency conflicts, exceptions, and failed executions should be investigated and resolved. Understanding the difference between a warning and an error is part of this lab.


**Prerequisites:** In Colab select **Runtime → Change runtime type → T4 GPU** (or another supported NVIDIA GPU).

## Parts

- [Part A](./part_01_Part_A_Build_the_Bookstore_Backend.ipynb)
- [Part B](./part_02_Part_B__Install_and_Load_Local_Qwen35-4B.ipynb)
- [Part C](./part_03_Part_C__Understanding_Tools_Contracts_Validation_and_Safe_Execution.ipynb)
- [Part D](./part_04_Part_D__Understanding_Tool_Selection_Qwen_Requests_Python_Executes.ipynb)
- [Part E](./part_05_Part_E__Multi-step_Tool_Calling_Tool_Result__Model__Tool_Result__Final_Answer.ipynb)
- [Part F](./part_06_Part_F__Extend_the_Agent_Write_and_Register_Your_Own_Tool.ipynb)
- [Part G](./part_07_Part_G__Exercises_Edge_Cases_and_Reflection.ipynb)



```python
%%writefile d_tool_protocol.py
"""Part D1 — Parse Qwen3.5 tool requests; validate before execution.

Qwen3.5 may emit the native XML-like form:
  <tool_call><function=search_books><parameter=genre>sci-fi</parameter></function></tool_call>
Other checkpoints may emit JSON inside <tool_call>. Support both here.
This parser is a teaching implementation, not a production general XML parser.
"""
import json
import re

BLOCK = re.compile(r"<tool_call(?:\s[^>]*)?>(.*?)</tool_call>", re.I | re.S)
FUNCTION = re.compile(r"<function=([^\s>]+)>(.*?)</function>", re.I | re.S)
PARAM = re.compile(r"<parameter=([^\s>]+)>(.*?)</parameter>", re.I | re.S)


def parse_scalar(text):
    value = text.strip()
    if not value:
        return ""
    try:
        return json.loads(value)  # 12 -> int; 15.5 -> float; true/false -> bool
    except json.JSONDecodeError:
        if value.lower() in ("true", "false"):
            return value.lower() == "true"  # 'True', 'FALSE' from weaker outputs
        if value.lower() in ("none", "null"):
            return None
        return value


def parse_tool_calls(raw_text):
    calls = []
    for block in BLOCK.findall(raw_text):
        block = block.strip()
        function = FUNCTION.search(block)
        if function:
            name = function.group(1).strip()
            body = function.group(2)
            args = {key: parse_scalar(value) for key, value in PARAM.findall(body)}
            calls.append({"name": name, "arguments": args})
            continue
        try:
            payload = json.loads(block)
        except json.JSONDecodeError:
            continue  # malformed call stays visible in raw model output, not executed
        if not isinstance(payload, dict):
            continue
        if isinstance(payload.get("function"), dict):
            payload = payload["function"]
        name = payload.get("name")
        args = payload.get("arguments", {})
        if isinstance(args, str):
            try:
                args = json.loads(args)
            except json.JSONDecodeError:
                continue
        if isinstance(name, str) and isinstance(args, dict):
            calls.append({"name": name, "arguments": args})
    return calls


def proposed_calls(raw_text):
    """Return parsed calls and status for inspection."""
    calls = parse_tool_calls(raw_text)
    if calls:
        return {"calls": calls, "status": "tool_call_detected"}
    if "<tool_call" in raw_text:
        return {"calls": [], "status": "invalid_tool_call"}
    return {"calls": [], "status": "no_tool_requested"}
```