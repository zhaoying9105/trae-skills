---
name: mr-commit-change-analyzer
description: Analyze MR/Commit level code changes, provide intuitive and rich logical interpretation, including intent, impact scope, and potential issues.
metadata:
  openclaw:
    requires:
      bins: ["git"]
    permissions:
      filesystem: "read"
      shell: "allow"
    tags: ["code-review", "git", "analysis"]
    author: "openclaw"
    version: "1.0.0"
---

# MR/Commit Change Analyzer

## Description
This skill analyzes code changes at the Merge Request (MR) or individual commit level. It provides deep, intuitive, and rich logical interpretation of modifications, helping developers quickly understand the purpose, impact, and potential risks of code changes without reading through all diff lines manually.

## Usage Scenario
Trigger this skill when:
- The user asks to "analyze this MR" or "explain this commit"
- The user wants to understand what changes were made and why
- Code review process needs a quick summary of change logic
- You need to assess the impact of incoming changes

## Instructions

1. **Input Collection**: Collect diff, issue links, PR description, and commit messages. Identify language and context.
2. **Classification**: Categorize changes (feature, bug, refactor, etc.) and group by module. Separate logical from cosmetic changes.
3. **Deep Analysis**: Identify core intent, map to domains, analyze data flow/API changes, assess impact scope and risks.
4. **Structure Output**: Use the "Output Template" below. Use clear non-technical language where possible. Include critical code snippets.
5. **Validation**: Verify against commit messages. Ensure critical changes are covered and flag ambiguities.

## Change Type Classification

| Type | Description |
|------|-------------|
| **Feature** | New functionality, endpoints, UI, or logic |
| **Bug Fix** | Corrects behavior, crashes, or errors |
| **Refactor** | Structure changes without logic change (cleanup, renaming) |
| **Perf** | Improves performance/latency/resource usage |
| **Docs** | Updates to documentation/comments |
| **Style** | Formatting/whitespace only |
| **Test** | Test cases added/modified |
| **Deps** | Dependency updates |
| **Config** | Config/env/deployment settings |
| **Breaking** | Breaks backward compatibility |

## Risk Assessment Guidelines

- **High Risk**: Breaking API changes, Auth logic, Data persistence/schema, Critical path (payment/core), Security changes, Removal of functionality.
- **Medium Risk**: Performance code, Major dependency updates, Error handling flow, Shared utilities, Production config.
- **Low Risk**: Docs, UI (cosmetic), Tests, Formatting, Logging, Isolated non-critical features.
- **Flags**: Missing tests, Large unfocused commits, Ambiguous messages, Magic numbers, Pattern deviations.

## Output Template

```markdown
# MR/Commit Change Analysis

## Basic Information
- **ID:** {id} | **Author:** {author} | **Date:** {date}
- **Stats:** +{additions} / -{deletions} lines, {files} files
- **Type:** {primary_change_type}

## Core Intent
{concise description of main purpose}

## Key Changes by Module
### {module_name}
- {change_description}

## Impact Analysis
- **Components:** {component_1}, {component_2}
- **Compat:** ✅ Full | ⚠️ Partial | ❌ Breaking ({explanation})
- **Perf:** ✅ None | ⚠️ Improved | ⚠️ Degraded | ❌ Significant ({explanation})

## Risk Assessment
- **High:** {list or "None"}
- **Medium:** {list or "None"}

## Recommendations
- {recommendation_1}

## Critical Code Snippets
```{language}
{code_snippet}
```
*Why: {explanation}*
```
