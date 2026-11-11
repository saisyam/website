---
title: "Photo AI"
description: "A nondestructive photo editor you talk to. Ask it to “light up the sky” and a small local model turns that into real adjustments."
stage: "v1"               # idea | prototype | v1 | beta | released
label: "Desktop app"
art: 2
weight: 1
# featured: true          # put this Lab item in the home page featured spot
cta:
  label: "Follow progress"
  url: "/index.xml"
facts:
  - { k: "Version", v: "v1" }
  - { k: "Runs on", v: "Any modern laptop, no GPU needed" }
  - { k: "Model", v: "Qwen (Apache 2.0) via llama.cpp" }
  - { k: "Raw engine", v: "LibRaw + own pipeline" }
  - { k: "License", v: "Free, closed source" }
roadmap:
  done:
    - "Plain-language edits"
    - "Nondestructive pipeline"
    - "Auto-correct and sliders"
    - "Blur-based culling"
  now:
    - "Object detection (faces, sky, cars)"
    - "Portrait-specific corrections"
  next:
    - "Looks: autumn, winter, vivid, cinematic"
    - "Early-access download with email opt-in"
    - "Feedback page"
# Lab log: newest first. EXAMPLE ENTRIES, edit the dates and text to match reality.
log:
  - { date: 2026-11-08, text: "Project page published on saisyam.dev." }
  - { date: 2026-10-21, text: "v1 complete: plain-language edits, auto-correct, sliders and blur culling, all running locally." }
stack: ["LibRaw", "llama.cpp", "Qwen", "Local-first"]
---
## What it does

Most people who take good photos don't enjoy editing them. Professional tools are powerful but expect you to know what “highlights” or “white balance” mean. Photo AI lets you describe the result you want instead.

- **Plain-language edits.** “Light up the sky” becomes concrete adjustments such as exposure and highlights.
- **Nondestructive.** Your original is never touched. Every edit is a set of adjustments you can tweak or undo.
- **Simple sliders** when you want direct control, plus **auto-correct** for a one-click start.
- **Culling.** Blurry shots are flagged so you can clear a shoot fast.

## How it works

The processing pipeline is my own code, with LibRaw handling raw files. A small, Apache-licensed Qwen model runs locally through llama.cpp and acts as a planner: it reads your request and decides which adjustments to make and by how much. It never paints pixels itself. The model is a separate download, and you can swap in another one.
