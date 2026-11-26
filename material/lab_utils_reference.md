# Lab utilities reference

The repository module `lab_utils.py` manages local Colab infrastructure. It is not an A2A SDK and does not perform model reasoning. Import it after cloning the current repository.

```python
import sys
from pathlib import Path
REPO = Path('/content/w6-demo')
sys.path.insert(0, str(REPO))
import lab_utils as lab
lab.configure(REPO, delay=20)
```

| Function | Parameters | Behavior / return |
|---|---|---|
| `configure` | `repo, delay=20, log_dir='/content/seller_logs'` | Sets service paths, ports, log location and delay; returns `AGENTS`; refuses reconfiguration while managed processes run. |
| `set_delay` | `seconds` | Sets default pause; rejects negative values. |
| `pause` | `seconds=None` | Blocking sleep, using default delay when omitted. Use `asyncio.sleep` in async tools instead. |
| `run_command` | `command, cwd=None` | Runs a subprocess, prints stdout/stderr, returns stdout; raises on nonzero status. |
| `create_environments` | none | Creates missing Python 3.12 virtual environments via uv. |
| `install_requirements` | `directory` (Path) | Installs `requirements-colab.txt` into that seller's `.venv`. |
| `start_seller` | `name` | Starts or returns an existing subprocess; inherits environment; writes log. Returns `Popen`. |
| `wait_for_agent` | `name, timeout=120` | Polls the modern card endpoint; returns decoded card or raises on failure/timeout. |
| `show_processes` | none | Displays a DataFrame of managed service names, PIDs, ports and status. |
| `show_log` | `name, lines=30` | Displays tail of the seller log. |
| `show_source` | `path` | Displays Python source with syntax highlighting. |
| `stop_seller` | `name` | Terminates and waits, kills if needed, removes registry entry. |
| `stop_all` | none | Stops all managed services. |
| `switch_provider` | `provider, model` | Checks key, stops both services, sets environment, restarts and waits; returns cards by name. |

## Shared state

`AGENTS` contains directory, URL and port for each service. `processes` maps names to subprocess handles. `LOG_DIR` points to the logs, outside the repository by default. `REQUEST_DELAY` is the default blocking pause. Do not replace the process dictionary while servers are running.

## Examples

```python
lab.create_environments()
for cfg in lab.AGENTS.values():
    lab.install_requirements(cfg['directory'])
cards = lab.switch_provider('groq', 'openai/gpt-oss-120b') # key must be configured
lab.show_processes()
lab.show_log('burger')
lab.stop_all()
```

```python
import inspect
from IPython.display import Code, display
display(Code(data=inspect.getsource(lab.start_seller), language='python'))
```

## A2A client module

`clients/a2a_client.py` is separate from process management. `discover(url)` retrieves and checks the card; `await send(url, text, show_json=False, timeout=180)` creates a non-streaming SDK client, sends a typed message, validates a completed task, extracts text artifacts and closes the connection. Returns `task_id`, `context_id`, `state`, `text`. The timeout bounds the client wait; it does not necessarily cancel server execution. It is specific to the task-returning sellers in this repository, not a universal response parser.

## Operational notes

Model provider keys stay in Colab Secrets. Never display the environment or commit credentials. Local services bind to loopback. Inspect logs for 429 or provider quota errors; a timeout alone is not proof of quota exhaustion. Restarting services loses in-memory task state. The delay is a pacing control, not a quota guarantee.
