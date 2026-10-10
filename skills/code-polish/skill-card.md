## Description:

Pre-release code review that runs lint and type checks, coordinates cleanliness, design, efficiency, and side-effect gating review, validates findings, supports GitHub PR review, and applies fixes only after approval.

This skill is ready for commercial/non-commercial use.

## Publisher:

[tenequm](https://clawhub.ai/user/tenequm)

### License/Terms of Use:

Apache 2.0

## Use Case:

Developers and engineers use this skill before committing, pushing, or reviewing a pull request to run project checks, inspect changed code, surface actionable findings, and optionally apply targeted fixes after approval.

### Deployment Geography for Use:

Global

## Known Risks and Mitigations:

Risk: Fix mode can change local code before the advertised approval point.

Mitigation: Use review mode for read-only assessment, work from a clean branch, and require explicit confirmation before fixes are applied.

Risk: The skill may run project check commands and use GitHub CLI flows for PR checkout, cloning, review, and posting.

Mitigation: Review commands before execution, use trusted repositories and credentials, and require confirmation before any GitHub posting.

Risk: Changed code, PR text, and repository files are read as part of review.

Mitigation: Run the skill only in repositories where the agent is allowed to inspect the changed source and related project context.

## Reference(s):

- [ClawHub skill page](https://clawhub.ai/tenequm/skills/code-polish)
- [Skill homepage](https://github.com/tenequm/skills/tree/main/skills/polish)

## Skill Output:

**Output Type(s):** [text, markdown, code, shell commands, configuration, guidance]

**Output Format:** [Markdown review report with file and line references, optional code edits, shell command summaries, and optional GitHub pull request review payloads.]

**Output Parameters:** [1D]

**Other Properties Related to Output:** [May read changed code, run project validation commands, use GitHub CLI workflows, edit local code in fix mode, and post pull request reviews after confirmation.]

## Skill Version(s):

3.1.0 (source: frontmatter, changelog, server release metadata)

## Ethical Considerations:

Users should evaluate whether this skill is appropriate for their environment, review any generated or modified files before relying on them, and apply their organization's safety, security, and compliance requirements before deployment.
