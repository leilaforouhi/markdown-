#!/usr/bin/env python3
"""
Markdown Counter (CLI version)
- Scans all Markdown files in the repo
- Prints counts of headings (#), links [text](url), and code blocks (```...```)
"""

import os, re

def analyze_markdown(path: str):
    with open(path, "r", encoding="utf-8", errors="ignore") as f:
        text = f.read()

    headings = len(re.findall(r"^#+ ", text, flags=re.MULTILINE))
    links = len(re.findall(r"\[[^\]]+\]\([^)]+\)", text))
    code_blocks = len(re.findall(r"```", text)) // 2  # each pair = 1 block

    print(f"\n📄 {path}")
    print(f"  Headings:    {headings}")
    print(f"  Links:       {links}")
    print(f"  Code blocks: {code_blocks}")

def main():
    found = False
    for root, _, files in os.walk("."):
        for name in files:
            if name.endswith(".md"):
                analyze_markdown(os.path.join(root, name))
                found = True
    if not found:
        print("No Markdown files found.")

if __name__ == "__main__":
    main()
