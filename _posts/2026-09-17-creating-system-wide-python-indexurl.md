---
title: Get Python packages from a different Index URL
layout: post
category: Technical
---

# Set the default package index for uv (Windows and macOS)

Configure this once per user on each machine. New and existing uv projects will use the Microsoft feed by default.

## 1. Open the user configuration file

**Windows:** Run in PowerShell:

```powershell
New-Item -ItemType Directory -Force -Path "$env:APPDATA\uv" | Out-Null
notepad "$env:APPDATA\uv\uv.toml"
```

If Notepad asks to create the file, accept.

**macOS:** Run in Terminal:

```sh
config_dir="${XDG_CONFIG_HOME:-$HOME/.config}/uv"
mkdir -p "$config_dir"
nano "$config_dir/uv.toml"
```

The default file location is `~/.config/uv/uv.toml`. If `XDG_CONFIG_HOME` is set, the commands use `$XDG_CONFIG_HOME/uv/uv.toml` instead.

## 2. Add the index and save

```toml
[[index]]
url = "https://packagefeedproxy.microsoft.io/pypi/simple/"
default = true
```

Preserve any other settings. If a default index already exists, update that entry instead of adding another default.

On Windows, save with **Ctrl+S**. In macOS nano, press **Ctrl+O**, then **Return** to save, and **Ctrl+X** to exit.

**Important:** Use `[[index]]` in `uv.toml`. The `[[tool.uv.index]]` form is only for a project's `pyproject.toml`.

## 3. Use uv normally

No per-project setup is needed. Commands such as `uv add`, `uv sync`, and `uv run` inherit this configuration. This feed replaces PyPI as the default index; project-specific settings, environment variables, or command-line options can override it.

**File location on this Windows machine:**

```text
C:\Users\royakash\AppData\Roaming\uv\uv.toml
```

**Reference:** [uv configuration files](https://docs.astral.sh/uv/concepts/configuration-files/)
