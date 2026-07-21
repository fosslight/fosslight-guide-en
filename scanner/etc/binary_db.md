---
published: true
---

# FOSSLight Database Integration Guide
This guide explains how to connect to FOSSLight Database from FOSSLight Source Scanner.

## Scanner Database Connection
To extract OSS information (OSS Name, OSS Version, License) from the database, configure database connection settings first.

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
