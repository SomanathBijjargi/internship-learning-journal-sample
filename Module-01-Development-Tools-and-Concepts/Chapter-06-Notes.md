# Week 2 Session 3
## Github Codespaces
GitHub Codespaces is a cloud-hosted development environment built right into GitHub that gets you coding faster with pre-configured containers, adjustable compute power, and seamless integration with workflows like Actions and Copilot.

Why Codespaces helps

- **Reproducible onboarding:** Say goodbye to “works on my machine” woes—everyone uses the same setup for assignments or demos.
-** Anywhere access:** Jump back into your project from a laptop, tablet, or phone without having to reinstall anything.
- **Rapid experimentation & debugging:** Spin up short-lived environments on any branch, commit, or PR to isolate bugs or test features, or keep longer-lived codespaces for big projects.

### Quick Setup
1. From Github UI
    - Go to your repo and click Code → Codespaces → New codespace.
    - Pick the branch and machine specs (2–32 cores, 8–64 GB RAM), then click Create codespace.
2. In Visual Studio Code
    - Press Ctrl+Shift+P (or Cmd+Shift+P on Mac), choose Codespaces: Create New Codespace, and follow the prompts.

## Key Features to remember

1. **Dev-Container:**

    When you work in a codespace, the environment you are working in is created using a development container, or dev container, hosted on a virtual machine.

    The primary file in a dev container configuration is the devcontainer.json file. You can use this file to determine the environment of codespaces created for your repository.

    If you create a codespace from a repository without a devcontainer.json file, or if you start from GitHub's blank template, the default dev container configuration is used.

2. **Prebuilds:**

    A prebuild assembles the main components of a codespace for a particular combination of repository, branch, and devcontainer.json configuration file. It provides a quick way to create a new codespace. For complex and/or large repositories in particular, you can create a new codespace more quickly by using a prebuild

3. **Github copilot:**

    GitHub Copilot is an AI pair programmer that you can use in any codespace that you open in the VS Code web client or desktop application. For more information about GitHub Copilot

## Model Deployment on Hugging-Face

If you prefer a traditional git push flow, you must initialize Git LFS (Large File Storage) to handle the heavy model weights.
```
# 1. Clone your new repo
git clone https://huggingface.co/username/my-awesome-model
cd my-awesome-model

# 2. Initialize LFS (Crucial for files >10MB) Important while using Hugging Face
git lfs install

# 3. Track specific large file types
git lfs track "*.bin"
git lfs track "*.onnx"
git add .gitattributes

# 4. Add, commit, and push
git add .
git commit -m "Initial model upload"
git push