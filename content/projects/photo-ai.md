---
title: "Photo AI"
description: "A nondestructive photo editor you talk to. Ask it to “light up the sky” and a small local model turns that into real adjustments."
status: "v1 built"
label: "Desktop app"
weight: 1
stack: ["LibRaw", "llama.cpp", "Qwen (Apache 2.0)", "Runs on a laptop"]
---
## Why I built it

Most people who take good photos don't enjoy editing them. Professional tools are powerful but expect you to know what “highlights” or “white balance” mean. I built Photo AI first for my son, who loves taking photos but not editing them.

## What v1 does

- **Plain-language edits.** Type “light up the sky” and the assistant turns it into concrete adjustments such as exposure and highlights.
- **Nondestructive.** Your original file is never touched. Every edit is a set of adjustments you can tweak or undo.
- **A few simple sliders** for manual correction when you want control.
- **Auto-correct** for a quick one-click fix.
- **Culling.** Blurry shots are flagged so you can clear them out fast.

## How it's built

The processing pipeline is my own code, with LibRaw handling raw files. The assistant is a small, Apache-licensed Qwen model loaded through llama.cpp, so everything runs locally on a laptop with no GPU and no cloud. The model is a separate download, and you can swap in another model if you prefer.

## What's next

The next version will understand what's *in* the photo, such as faces, sky and cars, so you can say “soften the face” and get corrections tuned to the kind of photography (portrait first, then others), plus looks like autumn, winter, vivid and cinematic.

*Download and early access are coming soon.*
