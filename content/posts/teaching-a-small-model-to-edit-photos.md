---
# DRAFT FOR REVIEW: rewrite in your own words and add real screenshots before merging.
title: "Teaching a Small Model to Edit Photos"
description: "Most people love taking photos and hate editing them. So I built an editor you can simply talk to, with a small model that runs on an ordinary laptop."
art: 1
date: 2026-11-08
kicker: "Build Notes"
featured: true
project: photo-ai
# image: /images/photo-ai-cover.jpg   # add a real before/after here
categories: ["Photo AI", "Small Language Models"]
---
Most people who take good photos don't enjoy editing them. Professional editors are powerful, but they assume you already know what “highlights,” “white balance” and “clarity” mean, and which of the forty sliders to reach for. My son loves taking photos and has no patience for any of that. That gap is where this project started.

## The idea: describe the photo you want

Instead of learning the tool, you tell it what you want: *“light up the sky,”* *“make it warmer,”* *“bring out the shadows.”* A small language model reads the request and turns it into concrete edits such as exposure, highlights and temperature, which you can see, tweak or undo.

> The model doesn't paint pixels. It decides which knobs to turn, and by how much.

That distinction matters. Because the model only proposes adjustments, every edit stays **nondestructive**: your original file is never touched, and you're always one slider away from overriding it.

## Why small, and why local

The people I'm building for don't have GPUs. They have laptops. So the whole design starts from one constraint: the assistant must run comfortably on an ordinary machine, with no cloud and no upload.

That ruled out the big models and pushed me toward a small, Apache-licensed Qwen model running through llama.cpp. The model is a separate download, so you can swap in another one if you prefer.

## What's under the hood

- **Raw processing** with LibRaw, plus my own processing pipeline. No borrowed editor code.
- **A model that plans edits** by translating plain language into adjustment values.
- **A few simple sliders** for when you want direct control.
- **Auto-correct** for a one-click starting point.
- **Culling** that flags blurry shots so you can clear a shoot quickly.

## What v1 can't do yet

Version one only sees the image as a whole: brightness, color, contrast. It doesn't know there's a *face* in the frame or a *car* in the foreground. So “soften the face” is beyond it today.

That's the next step: detect what's in the photo, then apply corrections tuned to the kind of photography, starting with portraits, along with looks like autumn, winter, vivid and cinematic.

## Follow along

I'll keep posting build notes here as the project moves, along with the experiments that didn't work. You can see the project page in [the Lab](/projects/photo-ai/).
