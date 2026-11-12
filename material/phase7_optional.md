# Optional Challenge: Migrate Legacy MCP Servers to Official SDK v2

The code below was written for **MCP SDK v1** (using the legacy `mcp.server.fastmcp` module).  
Your mission is to migrate these servers to the **latest official MCP Python SDK (v2)**.

### Target Specifications

* **Library:** Official `mcp` package (`pip install "mcp[cli]>=2.0.0"`).
* **Core Class:** Use `from mcp.server import MCPServer` (in v2, `FastMCP` was renamed to `MCPServer`).
* **Do not** use the third-party `fastmcp` package for this challenge.

### Resources

* [Official MCP Python SDK v2 Documentation](https://py.sdk.modelcontextprotocol.io/)
* [Official v1-to-v2 Migration Guide](https://py.sdk.modelcontextprotocol.io/migration/)
* **Attribution:** The example servers below are adapted from the [MCP Documentation & Examples](https://github.com/anish-natekar/mcp_docs/blob/main/examples.rst) created by [Anish Natekar](https://github.com/anish-natekar).


-------

## Basic server

A simple server demonstrating resources, tools, and prompts:

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("Echo")

@mcp.resource("echo://{message}")
def echo_resource(message: str) -> str:
    """Echo a message as a resource"""
    return f"Resource echo: {message}"

@mcp.tool()
def echo_tool(message: str) -> str:
    """Echo a message as a tool"""
    return f"Tool echo: {message}"

@mcp.prompt()
def echo_prompt(message: str) -> str:
    """Create an echo prompt"""
    return f"Please process this message: {message}"
```

## SQLite Explorer

A more complex example showing database integration:

```python
import sqlite3

from mcp.server.fastmcp import FastMCP

mcp = FastMCP("SQLite Explorer")

@mcp.resource("schema://main")
def get_schema() -> str:
    """Provide the database schema as a resource"""
    conn = sqlite3.connect("database.db")
    schema = conn.execute("SELECT sql FROM sqlite_master WHERE type='table'").fetchall()
    return "\n".join(sql[0] for sql in schema if sql[0])

@mcp.tool()
def query_data(sql: str) -> str:
    """Execute SQL queries safely"""
    conn = sqlite3.connect("database.db")
    try:
        result = conn.execute(sql).fetchall()
        return "\n".join(str(row) for row in result)
    except Exception as e:
        return f"Error: {str(e)}"

@mcp.prompt()
def analyze_table(table: str) -> str:
    """Create a prompt template for analyzing tables"""
    return f"""Please analyze this database table:
Table: {table}
Schema: 
{get_schema()}

What insights can you provide about the structure and relationships?"""
```

## Image Processing Example

A server that demonstrates working with images:

```python
from PIL import Image as PILImage
from mcp.server.fastmcp import FastMCP, Image, Context
from io import BytesIO
import requests

mcp = FastMCP("Image Processor")

@mcp.tool()
def create_thumbnail(image_url: str, size: int = 100) -> Image:
    """Create a thumbnail from an image URL"""
    # Download the image
    response = requests.get(image_url)
    img = PILImage.open(BytesIO(response.content))
    
    # Create a thumbnail
    img.thumbnail((size, size))
    
    # Convert to FastMCP Image and return
    return Image.from_pil(img)

@mcp.tool()
def load_image(path: str) -> Image:
    """Load an image from disk"""
    # FastMCP handles reading and format detection
    return Image(path=path)

@mcp.resource("image://{path}")
def get_image(path: str) -> Image:
    """Serve an image as a resource"""
    try:
        return Image(path=path)
    except FileNotFoundError:
        return Image(data=b"Image not found", format="txt")

@mcp.tool()
async def process_images(image_paths: list[str], ctx: Context) -> str:
    """Process multiple images with progress tracking"""
    results = []
    
    for i, path in enumerate(image_paths):
        ctx.info(f"Processing image {i+1}/{len(image_paths)}: {path}")
        
        # Report progress
        await ctx.report_progress(i, len(image_paths))
        
        try:
            # Load the image
            img = PILImage.open(path)
            results.append(f"Image {path}: {img.width}x{img.height}, format: {img.format}")
        except Exception as e:
            results.append(f"Error processing {path}: {str(e)}")
    
    return "\n".join(results)
```

## File Explorer

A server that provides access to files in a directory:

```python
import os
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("File Explorer")

@mcp.resource("file://{path}")
def read_file(path: str) -> tuple:
    """Read a file from the data directory."""
    safe_path = os.path.normpath(os.path.join("data", path))
    if not safe_path.startswith("data"):
        return "Access denied: path must be within the data directory", "text/plain"
        
    if not os.path.exists(safe_path):
        return f"File not found: {path}", "text/plain"
        
    with open(safe_path, "rb") as f:
        content = f.read()
        
    # Determine MIME type based on extension
    ext = os.path.splitext(path)[1].lower()
    mime_map = {
        ".txt": "text/plain",
        ".md": "text/markdown",
        ".json": "application/json",
        ".html": "text/html",
        ".pdf": "application/pdf",
        ".png": "image/png",
        ".jpg": "image/jpeg",
        ".jpeg": "image/jpeg",
    }
    mime_type = mime_map.get(ext, "application/octet-stream")
    
    return content, mime_type

@mcp.resource("dir://{path}")
def list_directory(path: str = "") -> dict:
    """List files in a directory."""
    safe_path = os.path.normpath(os.path.join("data", path))
    if not safe_path.startswith("data"):
        return {"error": "Access denied: path must be within the data directory"}
        
    if not os.path.isdir(safe_path):
        return {"error": f"Directory not found: {path}"}
        
    files = []
    directories = []
    
    for item in os.listdir(safe_path):
        item_path = os.path.join(safe_path, item)
        if os.path.isdir(item_path):
            directories.append(item)
        else:
            files.append(item)
            
    return {
        "path": path,
        "directories": directories,
        "files": files
    }

if __name__ == "__main__":
    os.makedirs("data", exist_ok=True)
    mcp.run()
```

## Server with Context and Lifespan

A more advanced example demonstrating context and lifespan:

```python
from contextlib import asynccontextmanager
from collections.abc import AsyncIterator
from dataclasses import dataclass
from mcp.server.fastmcp import FastMCP, Context
import aiosqlite

# Type-safe context class
@dataclass
class AppContext:
    db: aiosqlite.Connection

# Lifespan manager
@asynccontextmanager
async def app_lifespan(server: FastMCP) -> AsyncIterator[AppContext]:
    """Setup and teardown database connection"""
    # Initialize on startup
    db = await aiosqlite.connect("app.db")
    await db.execute("CREATE TABLE IF NOT EXISTS notes (id INTEGER PRIMARY KEY, content TEXT)")
    await db.commit()
    
    try:
        yield AppContext(db=db)
    finally:
        # Cleanup on shutdown
        await db.close()

# Create server with lifespan
mcp = FastMCP("Notes App", lifespan=app_lifespan)

@mcp.tool()
async def add_note(content: str, ctx: Context) -> str:
    """Add a new note to the database"""
    # Access the database from the lifespan context
    db = ctx.request_context.lifespan_context.db
    
    # Log the operation
    ctx.info(f"Adding note: {content}")
    
    # Insert the note
    cursor = await db.execute("INSERT INTO notes (content) VALUES (?)", (content,))
    note_id = cursor.lastrowid
    await db.commit()
    
    return f"Note added with ID: {note_id}"

@mcp.tool()
async def get_notes(ctx: Context) -> list[dict]:
    """Get all notes from the database"""
    db = ctx.request_context.lifespan_context.db
    
    # Query all notes
    async with db.execute("SELECT id, content FROM notes") as cursor:
        notes = await cursor.fetchall()
        
    # Convert to list of dictionaries
    return [{"id": note[0], "content": note[1]} for note in notes]

if __name__ == "__main__":
    mcp.run()
```

## Weather API Server

A server that provides weather information through an external API:

```python
import aiohttp
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("Weather API", dependencies=["aiohttp"])

# You'd normally use a real API key in a production environment
API_KEY = "YOUR_OPENWEATHERMAP_API_KEY"
BASE_URL = "https://api.openweathermap.org/data/2.5"

@mcp.resource("weather://{location}")
async def get_weather(location: str) -> dict:
    """Get current weather for a location."""
    async with aiohttp.ClientSession() as session:
        url = f"{BASE_URL}/weather"
        params = {
            "q": location,
            "appid": API_KEY,
            "units": "metric"
        }
        
        async with session.get(url, params=params) as response:
            if response.status != 200:
                return {"error": f"Failed to fetch weather: {await response.text()}"}
                
            data = await response.json()
            return {
                "location": location,
                "temperature": data["main"]["temp"],
                "humidity": data["main"]["humidity"],
                "conditions": data["weather"][0]["description"],
                "wind_speed": data["wind"]["speed"]
            }

@mcp.tool()
async def forecast(location: str, days: int = 5) -> dict:
    """Get weather forecast for a location."""
    if days > 7:
        return {"error": "Maximum forecast days is 7"}
        
    async with aiohttp.ClientSession() as session:
        url = f"{BASE_URL}/forecast"
        params = {
            "q": location,
            "appid": API_KEY,
            "units": "metric",
            "cnt": days * 8  # API returns data in 3-hour intervals (8 per day)
        }
        
        async with session.get(url, params=params) as response:
            if response.status != 200:
                return {"error": f"Failed to fetch forecast: {await response.text()}"}
                
            data = await response.json()
            
            # Process and simplify the forecast data
            forecast_data = []
            for item in data["list"]:
                forecast_data.append({
                    "time": item["dt_txt"],
                    "temperature": item["main"]["temp"],
                    "conditions": item["weather"][0]["description"]
                })
                
            return {
                "location": location,
                "forecast": forecast_data
            }

@mcp.prompt()
def weather_assistant(location: str = "") -> dict:
    """Create a prompt for a weather assistant."""
    system_content = "You are a weather assistant. You can help users check the weather using the weather resources and forecast tool."
    
    user_content = "I'd like to know about the weather."
    if location:
        user_content = f"I'd like to know about the weather in {location}."
        
    return {
        "messages": [
            {"role": "system", "content": system_content},
            {"role": "user", "content": user_content}
        ]
    }
```
