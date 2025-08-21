# zizi-repo
like a moon
import datetime
import os

def ask_input(prompt, default=""):
    user_input = input(f"{prompt} " + (f"[{default}] " if default else ""))
    return user_input.strip() if user_input.strip() else defaul

def generate_readme(data):
    content = f"""# {data['project_name']}

{data['description']}

## Features
{format_list(data['features'])}

## Installation
```bash
git clone {data['repo_url']}
cd {data['project_name']}

