# Activity: Tool Calling and Agentic AI with Local and Hosted LLMs  - Part 1

## Groq Account and API Access

In Parts H and I, we will compare our locally running Qwen model with models hosted through **GroqCloud**. Before reaching these sections, create a GroqCloud account at https://console.groq.com/ and generate an API key from https://console.groq.com/keys. Store the key securely in **Google Colab → Secrets** using the name `GROQ_API_KEY`, and grant the notebook access to it. Never paste API keys directly into notebook code or share them in submitted files. Groq provides a free usage tier with rate limits, so check https://console.groq.com/docs/rate-limits before running repeated experiments. Parts A–G do not require a Groq account.

## Image

**Image activity (Part H5):** Upload a JPEG or PNG image of a book cover of your choice to Colab, then change `H_IMAGE_PATH` in the Part H5 execution cell to the actual uploaded filename or path. The example `dl.jpg` is not a supplied file.

For the H5 image activity, upload a JPEG or PNG book-cover image you have permission to use to the Colab runtime. You will set its path in H5. Files in the course repository are not automatically copied into Colab.

Before Part A, select Runtime → Change runtime type → a supported NVIDIA GPU. Work through the notebook in order. Changing or deleting the runtime can remove files and variables. After a new runtime starts, rerun the setup and file-writing cells needed for your current part. After restarting only the Python session, check which files remain and rerun the required imports and model loader.

## Parts

- [Part H](./part_08_Part_H__Compare_Local_Qwen_with_Hosted_Groq_Models_Including_Real_Images.ipynb)
- [Part I](./part_09_Part_I__Evaluating_Tool-Calling_Agents.ipynb)