# Archiving and Downloading Project Files from Google Colab

Google Colab instances run on temporary virtual machines. When a session disconnects, closes, or is recycled, all files stored in the local file system are permanently deleted.

To preserve project files, scripts, and evaluation results, execute the following standalone code block as the final cell of the notebook.

---


> [!IMPORTANT]  
> If large language model (LLM) weights or checkpoints (e.g., Qwen 3.5 4B, LLaMA, or custom `.bin` / `.safetensors` / `.gguf` files) were downloaded into the environment, ensure they are excluded from the archive. Model parameter files consume several gigabytes of space, which will exhaust disk quotas, cause memory errors during compression, and lead to browser download timeouts. Only scripts, configurations, lightweight databases, and generated reports should be included.

---

### Standalone Export Code

```python
"""
Automatic session exporter for Google Colab.
Collects generated files and folders, filters out large model weights
and system directories, compresses them into a single ZIP archive, 
and triggers a browser download.
"""

import shutil
from pathlib import Path

# Identify the working directory
WORKING_DIR = Path("/content") if Path("/content").exists() else Path(".")
EXPORT_STAGING = Path("colab_export_staging")
ARCHIVE_NAME = "project_files"

# Built-in system directories, cache paths, and staging folders to ignore
EXCLUDE_NAMES = {
    ".config",
    "sample_data",
    ".ipynb_checkpoints",
    EXPORT_STAGING.name,
    f"{ARCHIVE_NAME}.zip",
}

# File extensions for large LLM checkpoints and model parameters to exclude
EXCLUDE_MODEL_EXTENSIONS = {
    ".safetensors",
    ".bin",
    ".pt",
    ".pth",
    ".onnx",
    ".gguf",
}

# Specific model folders to skip (e.g., local cache or weights directories)
EXCLUDE_MODEL_DIRS = {
    "qwen",
    "weights",
    "checkpoints",
    "models",
}


def should_exclude(path: Path) -> bool:
    """Determine whether a file or directory should be excluded from export."""
    # Check exact names
    if path.name in EXCLUDE_NAMES:
        return True
    
    # Check directory names against known model/weight folders
    if path.is_dir() and any(model_dir in path.name.lower() for model_dir in EXCLUDE_MODEL_DIRS):
        return True
    
    # Check file extensions against large model weight formats
    if path.is_file() and path.suffix.lower() in EXCLUDE_MODEL_EXTENSIONS:
        return True
        
    return False


# 1. Prepare clean staging directory
if EXPORT_STAGING.exists():
    shutil.rmtree(EXPORT_STAGING)
EXPORT_STAGING.mkdir(parents=True, exist_ok=True)

# 2. Collect project files and directories (omitting system and model weights)
for entry in WORKING_DIR.iterdir():
    if should_exclude(entry):
        continue

    target_path = EXPORT_STAGING / entry.name

    if entry.is_dir():
        # Copy directory tree while ignoring nested large model files
        shutil.copytree(
            entry,
            target_path,
            dirs_exist_ok=True,
            ignore=lambda src, names: [
                name for name in names
                if Path(name).suffix.lower() in EXCLUDE_MODEL_EXTENSIONS
                or any(m in name.lower() for m in EXCLUDE_MODEL_DIRS)
            ]
        )
    elif entry.is_file():
        shutil.copy2(entry, target_path)

# 3. Compress into a ZIP archive
archive_file = shutil.make_archive(ARCHIVE_NAME, "zip", EXPORT_STAGING)
print(f"Archive created: {archive_file}")

# 4. Remove staging directory
shutil.rmtree(EXPORT_STAGING)

# 5. Trigger download
try:
    from google.colab import files
    files.download(archive_file)
    print("Download initiated.")
except ImportError:
    print(f"Running outside Colab. File saved at: {archive_file}")
```


---

### How the Script Functions

1. **Automatic Detection**: 
   The script inspects the root workspace (`/content`) and detects every generated file, script, plot, database, and nested folder without requiring manual filename lists.

2. **System File Exclusion**: 
   Colab includes system directories (such as `.config` and `sample_data`) by default. These are filtered out to keep the resulting download clean.

3. **Safe Compression**: 
   Files are copied into a temporary staging folder first. This prevents the compression engine from trying to read the archive while simultaneously writing to it.

4. **Browser Download Trigger**: 
   `files.download()` sends an automated prompt to the browser to download the final `.zip` file directly to the local storage drive.

---

### Key Operational Characteristics

1. **Model Parameter Filtering**: 
   Files ending in `.safetensors`, `.bin`, `.pt`, `.gguf`, etc., as well as directories dedicated to model storage, are automatically bypassed to keep the export lightweight.
2. **Dynamic File Discovery**: 
   Scans `/content` directly to capture all scripts, evaluation tables, logs, and generated visual plots without requiring manual file lists.
3. **Colab System Isolation**: 
   Filters standard platform folders (`.config`, `sample_data`) to prevent environment artifacts from cluttering the resulting download.
4. **Staging Mechanism**: 
   Files are copied to an isolated directory prior to compression, avoiding file-lock collisions and self-referencing archive loops.