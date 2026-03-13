# Coder Debugger

A runtime-grounded evaluation and self-correction pipeline for LLM code generation across Python, Java, and C++.

## Motivation

Large language models can generate plausible code that still fails at compile time, breaks under tests, or contains deeper logical errors. For code agents to be reliable, they need more than one-shot generation. They need a closed-loop process that can execute outputs, diagnose failures, and iteratively repair buggy solutions using runtime feedback.

This repository studies that reliability problem directly. It implements a modular generate → execute → debug pipeline and evaluates whether runtime-grounded repair improves end-to-end code generation performance.

## What this project does

The system takes a programming task plus tests, generates candidate code with an LLM, executes the code, captures compiler traces and runtime failures, and routes the failure context to a debugger module that proposes repairs.

The current pipeline supports:
- code generation
- runtime execution and validation
- failure capture from compiler errors, test failures, and runtime exceptions
- iterative self-debugging
- batch evaluation on benchmark tasks
- PEFT fine-tuning with LoRA / QLoRA for an offline debugger model

## Why this matters

This project is aimed at reliable AI behavior rather than raw generation quality alone. It focuses on:
- detecting failure modes instead of trusting surface-level plausibility
- using runtime evidence as a grounding signal
- measuring recovery on concrete bug classes
- improving the dependability of code-generating systems

That makes it relevant to work on model evaluation, safeguards, runtime validation, and reliable agentic systems.

## Pipeline overview

The system is organized as four main components:

1. **Generator**  
   Produces candidate code for a task using an LLM.

2. **Executor**  
   Runs the generated code against tests and captures compile errors, runtime errors, and failing outputs.

3. **Debugger**  
   Uses the failure context to propose repairs, either through prompting or a fine-tuned offline debugger.

4. **Coordinator**  
   Orchestrates the full loop from generation to execution to repair.

## Repository structure

```text
coder_debugger/
│
├── Code Generation and Debugger/
│   ├── generator/            # Code generation logic
│   │   └── generator.py
│   ├── executor/             # Runtime execution module
│   │   └── executor.py
│   ├── debugger/             # Self-debugging and repair logic
│   │   └── debugger.py
│   ├── coordinator/          # End-to-end orchestration
│   │   └── pipeline.py
│
├── scripts/
│   ├── run_pipeline.py       # Single-example execution
│   ├── batch_eval.py         # Batch evaluation on benchmarks
│   └── train_lora.py         # LoRA / QLoRA fine-tuning
│
├── benchmarks/
│   ├── results/              # Output from model runs
│   └── metrics/              # pass@1, accuracy, and related metrics
