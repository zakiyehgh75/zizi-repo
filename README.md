# zizi-repo
like a moon
name: Run Python Script and Commit Changes

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  run:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: "3.10"

      - name: Install dependencies
        run: |
          if [ -f requirements.txt ]; then pip install -r requirements.txt; fi

      - name: Run Python script
        run: python your_script.py

      - name: Commit and push changes
        run: |
          git config --global user.name "github-actions[bot]"
          git config --global user.email "github-actions[bot]@users.noreply.github.com"
          git add .
          git commit -m "Auto-update from GitHub Actions" || echo "No changes to commit"
          git push

