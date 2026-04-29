# Code Review Skill

This skill enables AI agents to perform structured, thorough code reviews on pull requests, diffs, or individual files. It checks for code quality, security vulnerabilities, performance issues, and adherence to best practices.

## Overview

| Property | Value |
|----------|-------|
| **Skill ID** | `code-review` |
| **Category** | Engineering |
| **Complexity** | Intermediate |
| **Estimated Time** | 2–5 minutes per review |
| **Requires Tools** | GitHub / GitLab (optional), File I/O |

---

## What This Skill Does

- Analyzes code diffs or full files for quality issues
- Identifies potential bugs, anti-patterns, and code smells
- Flags security vulnerabilities (e.g., injection risks, hardcoded secrets)
- Suggests performance improvements
- Checks for test coverage gaps
- Produces a structured review report with severity levels

---

## Inputs

```yaml
inputs:
  code_diff:
    type: string
    description: A unified diff string or raw file content to review
    required: true
  language:
    type: string
    description: Programming language of the code (e.g., python, javascript, go)
    required: false
    default: auto-detect
  review_focus:
    type: array
    description: Specific areas to focus on during review
    items:
      - security
      - performance
      - readability
      - testing
      - style
    required: false
    default: [security, performance, readability]
  severity_threshold:
    type: string
    description: Minimum severity level to report (info, warning, error, critical)
    required: false
    default: warning
```

---

## Outputs

```yaml
outputs:
  summary:
    type: string
    description: High-level summary of the review findings
  issues:
    type: array
    description: List of identified issues
    items:
      line: integer
      severity: string  # info | warning | error | critical
      category: string  # security | performance | readability | testing | style
      message: string
      suggestion: string
  score:
    type: integer
    description: Overall code quality score from 0 (worst) to 100 (best)
  approved:
    type: boolean
    description: Whether the code passes the review threshold
```

---

## Example Prompt

```
Review the following Python code diff for security vulnerabilities and performance issues.
Focus on critical and error-level findings only. Return a structured JSON report.

<diff>
{code_diff}
</diff>
```

---

## Agent Configuration

See [`agents/openai.yaml`](./agents/openai.yaml) for a ready-to-use OpenAI agent configuration.

---

## Notes

- For best results, provide the programming language explicitly rather than relying on auto-detection.
- When reviewing large files, consider splitting into logical chunks of ≤300 lines.
- Security findings at `critical` severity should always block merges in CI pipelines.
- This skill pairs well with the **canvas-design** skill for visualizing review metrics.
