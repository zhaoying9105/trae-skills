---
name: mr-commit-change-analyzer
description: Analyze MR/Commit level code changes, provide intuitive and rich logical interpretation, including intent, impact scope, and potential issues.
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

1. **Input Collection**
   * Get the MR/commit diff content or access to the git repository
   * If available, collect related issue links, PR description, and commit messages
   * Identify the programming language and project context

2. **Change Classification**
   * Categorize changes by type: feature addition, bug fix, refactoring, documentation, etc.
   * Group related changes by module/functionality
   * Separate logical changes from cosmetic/formatting changes

3. **Deep Analysis**
   * Identify the core intent of the changes
   * Map modified files to business domains or technical components
   * Analyze data flow changes: input/output modifications, API changes
   * Assess impact scope: which parts of the system will be affected
   * Check for potential issues: breaking changes, security risks, performance implications

4. **Structure Output**
   * Follow the template in `templates/analysis-output.md`
   * Use clear, non-technical language where possible for business stakeholders
   * Include code snippets for critical changes when helpful
   * Highlight important notes and recommendations

5. **Validation**
   * Cross-verify the analysis with commit messages and PR descriptions
   * Ensure no critical changes are missed in the summary
   * Flag ambiguous changes that need further clarification

## Resources
- Refer to `resources/change-types.md` for standard change classification
- Refer to `resources/risk-assessment.md` for potential issue identification guidelines
