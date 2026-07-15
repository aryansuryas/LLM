
# 🚀 FreeLLMAPI + Docker + VS Code + Cline Complete Setup Guide (Windows)

> Complete end-to-end setup guide from scratch.
> This is the exact workflow to recreate everything if your PC is formatted.

---
               Your VS Code
                    │
                    ▼
          http://localhost:3001/v1
                    │
                    ▼
        ┌──────────────────────────┐
        │      Docker Container     │
        │                           │
        │      FreeLLMAPI Server    │
        │                           │
        └──────────────────────────┘
                    │
        ┌───────────┼────────────┐
        ▼           ▼            ▼
     Google       Groq      GitHub Models
        │           │            │
        ▼           ▼            ▼
             AI Response
                    │
                    ▼
              Your Project
# Goal

Run **FreeLLMAPI** locally using Docker, connect free AI providers (Google, Groq, etc.), and use it inside VS Code (Cline/OpenAI compatible clients).

---

# Prerequisites

Download and install:

## 1. Git

https://git-scm.com/downloads

Verify

```powershell
git --version
```

Example

```
git version 2.xx.x
```

---

## 2. Docker Desktop

Download

https://www.docker.com/products/docker-desktop/

Install.

Open Docker Desktop.

Wait until Docker says

```
Engine Running
```

Verify

```powershell
docker --version
docker compose version
```

---

## 3. VS Code

Download

https://code.visualstudio.com/

Install.

---

# Clone FreeLLMAPI

Open PowerShell

Move wherever you keep projects

Example

```powershell
cd C:\Users\aryan
```

Clone

```powershell
git clone https://github.com/tashfeenahmed/freellmapi.git
```

Go inside

```powershell
cd freellmapi
```

---

# Create Encryption Key

Generate a random encryption key

```powershell
$ENCRYPTION_KEY = -join ((1..64) | ForEach-Object {
"{0:x}" -f (Get-Random -Maximum 16)
})
```

---

# Create .env

Create

```
.env
```

Inside write

```
ENCRYPTION_KEY=YOUR_64_CHARACTER_KEY
PORT=3001
```

Example

```
ENCRYPTION_KEY=299cc54d0d6db9978246a563e00e4ad09d2b2ee472879b50d17a8d71cc180b57
PORT=3001
```

### IMPORTANT

Save as

```
UTF-8
```

NOT

```
UTF-16
```

Otherwise Docker gives

```
unexpected character
```

---

# Start Docker

Inside PowerShell

```powershell
docker compose up -d
```

---

Check

```powershell
docker ps
```

Should show

```
freellmapi
```

---

# Open FreeLLMAPI

Browser

```
http://localhost:3001
```

---

# Create Dashboard Account

Create

- Email
- Password

Login.

---

# Add Provider Keys

Go to

```
Keys
```

Choose provider

Example

- Google AI Studio
- Groq

---

# Google API Key

Go to

https://aistudio.google.com/

Login.

Create Project.

Click

```
Get API Key
```

Create key.

Copy.

It starts with

```
AIza....
```

NOT

```
AQ...
```

The AQ key is **not** an API key.

---

Paste AIza key into

```
Google AI Studio
```

Click

```
Save
```

---

Health should become

🟢 Green

If Red

Go to

```
Logs
```

or

PowerShell

```powershell
docker logs freellmapi-freellmapi-1
```

Example error

```
API_KEY_INVALID
```

Means

Wrong key.

---

# Groq API

Go

https://console.groq.com/

Create account.

Create API Key.

Starts with

```
gsk_
```

Paste into

```
Groq
```

Save.

Should become

🟢 Green.

---

# Unified API Key

Inside dashboard

Go

```
Keys
```

Top of page

You'll see

```
freellmapi-xxxxxxxxxxxxxxxx
```

Example

```
freellmapi-1d3241d58485731...
```

Copy it.

This is the ONLY key your applications use.

Never use Google or Groq keys directly inside apps.

---

# Test Playground

Go

```
Playground
```

Prompt

```
Hello
```

If reply comes

Everything works.

---

# Open VS Code

Open VS Code.

---

# Install Cline

Extensions

Search

```
Cline
```

Install.

---

# Configure Cline

Open Cline.

Choose

```
OpenAI Compatible
```

---

Base URL

```
http://localhost:3001/v1
```

---

API Key

Paste

```
freellmapi-xxxxxxxxxxxx
```

---

Model

```
auto
```

or

```
gemini-2.5-flash
```

or

```
llama-3.3-70b
```

---

Save.

---

# First Test

Ask

```
Create a React Todo App
```

If Cline starts creating files

Everything works.

---

# How FreeLLMAPI Works

```
VS Code
      │
      ▼
Cline
      │
      ▼
localhost:3001/v1
      │
      ▼
FreeLLMAPI Router
      │
 ┌────┴───────────────┐
 │                    │
 ▼                    ▼
Google            Groq
 │                    │
 ▼                    ▼
AI Response
```

You never directly call Google.

Everything goes through

```
FreeLLMAPI
```

---

# Why Docker?

Docker runs

FreeLLMAPI

inside an isolated Linux container.

Instead of installing

- Node.js
- npm
- SQLite
- dependencies

Docker already contains everything.

You only run

```powershell
docker compose up -d
```

---

# Daily Workflow

Start Docker Desktop.

Run

```powershell
cd C:\Users\aryan\freellmapi
```

Start

```powershell
docker compose up -d
```

Open

```
http://localhost:3001
```

Open VS Code.

Open Cline.

Start coding.

---

# Stop Server

```powershell
docker compose down
```

---

# Restart

```powershell
docker compose up -d
```

---

# Check Running Containers

```powershell
docker ps
```

---

# View Logs

```powershell
docker logs freellmapi-freellmapi-1
```

---

# Restart Container

```powershell
docker restart freellmapi-freellmapi-1
```

---

# Update FreeLLMAPI

```powershell
git pull
docker compose pull
docker compose up -d
```

---

# Common Errors

## API_KEY_INVALID

Wrong Google key.

Need

```
AIza...
```

Not

```
AQ...
```

---

## .env not found

Create

```
.env
```

---

## unexpected character

Saved as UTF-16.

Resave as

```
UTF-8
```

---

## Docker not running

Open Docker Desktop.

Wait until

```
Engine Running
```

---

## localhost:3001 not opening

Run

```powershell
docker ps
```

Container should be

```
Up
```

---

# Recommended Models

## Coding ⭐⭐⭐⭐⭐

```
auto
```

Best overall; the router automatically picks the strongest healthy model.

Alternative pinned models:

- `gemini-2.5-flash`
- `llama-3.3-70b`
- `codestral`
- `qwen3-coder`

---

## General Chat

```
gemini-2.5-flash
```

---

## Long Context

```
gemini-2.5-flash
```

---

## Fast Responses

```
llama-3.3-70b
```

---

# Complete Workflow Summary

1. Install Git.
2. Install Docker Desktop.
3. Install VS Code.
4. Clone the FreeLLMAPI repository.
5. Create a `.env` file with an encryption key and port.
6. Start the Docker container with `docker compose up -d`.
7. Open `http://localhost:3001`.
8. Create your dashboard account.
9. Add Google AI Studio (`AIza...`) and/or Groq (`gsk_...`) API keys.
10. Verify provider health turns green.
11. Copy the unified `freellmapi-...` key.
12. Test the Playground.
13. Install the Cline extension in VS Code.
14. Configure Cline:
    - Base URL: `http://localhost:3001/v1`
    - API Key: `freellmapi-...`
    - Model: `auto`
15. Start building projects using AI inside VS Code.

---

# You're Ready 🎉

You now have a local AI gateway that can route requests across multiple free providers, fail over automatically when one reaches its limit, and be used by VS Code, Cline, OpenAI-compatible SDKs, or your own applications.
````
