---
name: superpowers/using-omo-superpowers
description: Bootstrap profile for OpenCode + OMO that enables a selective Superpowers subset and blocks automatic use of plan/execution workflow skills.
---

# Using OMO Superpowers

## Profile Purpose

Use this profile when working in OpenCode + OMO and you want selective Superpowers usage without mandatory plan/execution workflow chaining.

## Enabled Skills

- brainstorming
- systematic-debugging
- verification-before-completion
- test-driven-development
- using-git-worktrees
- finishing-a-development-branch
- writing-skills

## Disabled Skills (Do Not Auto-Use)

- using-superpowers
- writing-plans
- executing-plans
- subagent-driven-development
- dispatching-parallel-agents
- requesting-code-review
- receiving-code-review

## Rules

1. Do not auto-use any disabled skill.
2. If a user directly requests a disabled skill by name, ask one concise confirmation question before using it.
3. If the user confirms, use only that explicitly requested disabled skill for that request.
4. Otherwise, continue using enabled skills and native OMO/OpenCode planning and delegation behavior.

## Confirmation Prompt Template

Use this exact style when a disabled skill is explicitly requested:

"You requested `<skill-name>`, which is disabled in this OMO profile. Do you want me to use it anyway for this request?"
