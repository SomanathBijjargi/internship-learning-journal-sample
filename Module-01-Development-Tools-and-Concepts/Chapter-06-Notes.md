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
```

## Github actions:

1. Creating your first workflow

    In your repository on GitHub, create a workflow file called github-actions-demo.yml in the .github/workflows directory. To do this:

    If the .github/workflows directory already exists, navigate to that directory on GitHub, click Add file, then click Create new file, and name the file github-actions-demo.yml.

    If your repository doesn't have a .github/workflows directory, go to the main page of the repository on GitHub, click Add file, then click Create new file, and name the file .github/workflows/github-actions-demo.yml. This creates the .github and workflows directories and the github-actions-demo.yml file in a single step.

2. Copy the following YAML contents into the github-actions-demo.yml file:

```
name: GitHub Actions Demo
run-name: ${{ github.actor }} is testing out GitHub Actions 🚀
on: [push]
jobs:
  Explore-GitHub-Actions:
    runs-on: ubuntu-latest
    steps:
      - run: echo "🎉 The job was automatically triggered by a ${{ github.event_name }} event."
      - run: echo "🐧 This job is now running on a ${{ runner.os }} server hosted by GitHub!"
      - run: echo "🔎 The name of your branch is ${{ github.ref }} and your repository is ${{ github.repository }}."
      - name: Check out repository code
        uses: actions/checkout@v5
      - run: echo "💡 The ${{ github.repository }} repository has been cloned to the runner."
      - run: echo "🖥️ The workflow is now ready to test your code on the runner."
      - name: List files in the repository
        run: |
          ls ${{ github.workspace }}
      - run: echo "🍏 This job's status is ${{ job.status }}."
```
3. **Click Commit changes.**

4. In the "Propose changes" dialog, select either the option to commit to the default branch or the option to create a new branch and start a pull request. Then click Commit changes or Propose changes.

Committing the workflow file to a branch in your repository triggers the push event and runs your workflow.

If you chose to start a pull request, you can continue and create the pull request, but this is not necessary for the purposes of this quickstart because the commit has still been made to a branch and will trigger the new workflow.