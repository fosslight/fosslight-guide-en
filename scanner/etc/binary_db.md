---
published: true
---

# FOSSLight Scanner Database Integration Guide
When running `fosslight` or `fosslight_source`, you can specify `--kb_url` and `--kb_token` to additionally query OSS Information (OSS Name, OSS Version, Download location) from the FOSSLight Scanner Database.

Example (fosslight, analyzing current directory):
````
fosslight --kb_url "https://kb.example.org/" --kb_token "example-token-1234567890abcdef" -p .
````

Example (fosslight_source, analyzing current directory):
````
fosslight_source --kb_url "https://kb.example.org/" --kb_token "example-token-1234567890abcdef" -p .
````

## Save Scanner Database Connection Settings
If you save FOSSLight Database connection settings as shown below, you do not need to enter `--kb_url` and `--kb_token` every time. The saved values are applied automatically.

### Linux / macOS
Apply only to the current terminal session:
````
export KB_URL="https://kb.example.org/"
export KB_TOKEN="example-token-1234567890abcdef"
````

Add to your shell startup file for persistent use:
````
echo 'export KB_URL="https://kb.example.org/"' >> ~/.bashrc
echo 'export KB_TOKEN="example-token-1234567890abcdef"' >> ~/.bashrc
source ~/.bashrc
````

For zsh, add the same lines to `~/.zshrc`.

### Windows Command Prompt
Apply only to the current session:
````
set KB_URL=https://kb.example.org/
set KB_TOKEN=example-token-1234567890abcdef
````

Save as user environment variables:
````
setx KB_URL "https://kb.example.org/"
setx KB_TOKEN "example-token-1234567890abcdef"
````

### Windows PowerShell
Apply only to the current session:
````
$env:KB_URL = "https://kb.example.org/"
$env:KB_TOKEN = "example-token-1234567890abcdef"
````

Save as user environment variables:
````
[System.Environment]::SetEnvironmentVariable("KB_URL", "https://kb.example.org/", "User")
[System.Environment]::SetEnvironmentVariable("KB_TOKEN", "example-token-1234567890abcdef", "User")
````
