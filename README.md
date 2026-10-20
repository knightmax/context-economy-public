# Context Economy — References

This page collects the public references used to prepare the talk **Context
Economy: Fewer Wasted Tokens, Reliable Results**.

## Searching and narrowing the input

### [`fd`](https://github.com/sharkdp/fd)

`fd` is a fast, user-friendly alternative to `find`. It is used in the talk as
an example of narrowing a repository search before opening files or sending
context to a model.

Example from the deck:

```bash
fd -e java . src
```

The reference supports the “search first, read second” part of the talk. It is
not presented as an AI-specific tool or as a guarantee of token savings;
the benefit depends on how the resulting paths are used.

### [`ripgrep`](https://github.com/BurntSushi/ripgrep)

`ripgrep` is a fast recursive text search tool. The talk uses it to locate a
symbol, event, or configuration value and then inspect only the relevant
matches.

Example from the deck:

```bash
rg "ORDER_CREATED" src
```

The important connection with token consumption is the reduction of irrelevant
file content before it becomes model input. Search output still needs to be
bounded and checked for sufficient surrounding context.

## Filtering command output

### [`snip`](https://github.com/edouard-claude/snip)

`snip` is the command-output filtering tool demonstrated in the talk. It is
used to discuss how an agent can receive a concise test or build summary
instead of an entire raw log.

Example shown in the slides:

```bash
snip mvn test
```

The talk treats this as an output-filtering pattern, not as a measured
benchmark. A useful filter should keep the exit status, the test summary, and
the diagnostics needed to investigate a failure. The raw log should remain
available when the summary is insufficient.

### [`RTK`](https://github.com/rtk-ai/rtk)

RTK is presented as another project to evaluate for reducing unnecessary
terminal output in agent workflows. It is included as an alternative to
compare with `snip`, rather than as a claim that one tool is universally
better.

For a meaningful comparison, use the same command, repository state, model,
tokenizer, and acceptance criteria before and after filtering. A shorter log
is not automatically a better diagnostic.

## Prompt caching and context reuse

### [OpenAI — Prompt caching](https://developers.openai.com/api/docs/guides/prompt-caching)

This is the primary public reference for the caching section of the talk. It
documents prefix-based reuse, cache eligibility, cache reads and writes,
retention behavior, and the way cached input is represented in usage and
pricing.

It supports the distinction between:

- sending context in a request;
- reusing an eligible processed prefix;
- the provider's billing counters; and
- a product's separate quota or usage units.

The conditions described by OpenAI should not be generalized to every model
provider.

### [Anthropic — Prompt caching](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)

This reference is included to compare another provider's caching model,
including explicit cache control and retention behavior.

It helps support the talk's warning that cache thresholds, invalidation rules,
TTL, and pricing are provider-specific. The page should be consulted for the
current API contract rather than treated as a universal description of KV
caching.

### [Google Gemini API — Context caching](https://ai.google.dev/gemini-api/docs/caching)

Google's documentation provides a third provider perspective on context
caching, including explicit cached content and lifecycle management.

It is useful for showing why a cache percentage or a caching workflow from one
provider cannot be copied directly to another provider's accounting model.

## Harnesses, specifications, and agent workflows

### [Naresh Jain — “Are Harnesses Worth the Cost?”](https://martinfowler.com/articles/are-harnesses-worth-the-cost.html)

This Martin Fowler case study is the source behind the talk's discussion of
behavioral harnesses and feedback loops. It compares different development
approaches using a Java service and external acceptance checks, and makes the
cost of verification part of the analysis.

The article is useful because it does not reduce the comparison to model
tokens alone: elapsed time, reliability, feedback, and the cost of the
harness also matter. Its results are an experiment in a specific setting, not
a universal benchmark for every agent workflow.

### [Microsoft Agentic SDLC Starter](https://github.com/microsoft/agentic-sdlc-starter)

This repository is a reference architecture for structuring agent-assisted
software development with specialized prompts, workflow steps, and reviewable
artifacts.

It supports the talk's spec-first and harness discussion: make the requested
work explicit, separate responsibilities, and keep outputs reviewable. The
repository is an architectural reference, not evidence of a particular token
reduction.

### [VS Code 1.140 release notes](https://code.visualstudio.com/updates/v1_140)

These release notes are cited for the version-specific Copilot harness and
telemetry changes relevant to the talk's preparation.

They are a dated product reference. Availability and behavior should be
checked again when the talk or its demo is delivered.

## Local models and inference

### [`llama.cpp`](https://github.com/ggml-org/llama.cpp)

`llama.cpp` is the local inference runtime referenced in the talk's
experimentation section. It illustrates the path from a locally available
model to an OpenAI-compatible endpoint that can be used by a development
client.

The reference is relevant to the discussion of routing a bounded task to local
inference. Local execution still has hardware, electricity, latency,
maintenance, security, model-provenance, and output-quality costs.

### [Hugging Face Hub documentation](https://huggingface.co/docs/hub)

The Hugging Face Hub documentation is relevant to model discovery,
distribution, repositories, metadata, and the practical handling of model
artifacts.

It is included to make clear that selecting a local model is part of the
workflow design. A model repository or downloadable artifact is not, by
itself, a security approval or a quality guarantee; provenance, license,
runtime exposure, and evaluation still need to be checked.

## Observability and measurement

### [VS Code — Monitoring Agent Usage with OpenTelemetry](https://github.com/microsoft/vscode/blob/main/extensions/copilot/docs/monitoring/agent_monitoring.md)

This is the specific VS Code reference used for the agent-monitoring part of
the talk. It documents the available configuration and telemetry integration
for monitoring agent usage with OpenTelemetry.

The exact fields and configuration depend on the VS Code version and enabled
features. Verify the version used for a demonstration, and review data-policy
requirements before exporting prompts, source content, identities, or other
sensitive attributes.
