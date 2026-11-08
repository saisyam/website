---
title: "Architecture Diagram SLM"
description: "A small language model that turns a plain-English description of a system into a clean architecture diagram."
status: "Planned"
label: "Small language model"
art: 0
weight: 2
stack: ["Small language model", "Fine-tuning", "Diagram-as-code"]
---
## The idea

Architects spend a surprising amount of time drawing boxes and arrows. I want a small, specialized model that takes a description like “events land in Kafka, get processed in Databricks and served from Snowflake to a BI layer” and produces a clean, editable architecture diagram.

## Why a small model

A focused model trained for one job can be fast and cheap, and it can run locally, so it works on confidential system designs that should never leave your machine.

## Status

Planning. I'll share the dataset approach, training notes and early results here as the work moves.
