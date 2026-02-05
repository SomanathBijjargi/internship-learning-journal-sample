# Week 1, Session 2 and Session 3
## Git
A Version Control System 

## Linux

Layer    |	What it does
---------|---------------
Hardware |	The physical components (CPU, RAM, Disk).
Kernel	 |  The brain (Linux) that manages the hardware.
Shell	 |  The terminal environment (Bash) where you type commands.
Userspace|	The tools you install, like uv, vi, and llm.

## Installation of uv 
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```
## Installation of LLM
```
uv tool install llm
```

## Install Gemini
```
llm install llm-gemini
```

## setup an gemini API keys
```
llm keys set gemini
```
Now it will ask for a API KEY
Go to Google AI Studio and generate API Key 
```
Enter key:<APIKEY>
```
Now you can run the Gemini Locally
```
llm -m gemini-2.5-flash "Who are you?"
```
Now it will respond you with the proper answer