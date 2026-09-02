---
name: skill-forge
description: "Create high-quality, production-grade skills for Claude Code. Expert guidance on skill architecture, workflow design, prompt engineering, and packaging. Use when user wants to create a new skill, build a skill, design a skill, write a skill, update an existing skill, improve a skill, refactor a skill, debug a skill, or package a skill. Triggers: 'create skill', 'build skill', 'new skill', 'skill creation', 'write a skill', 'make a skill', 'design a skill', 'improve skill', 'package skill', 'skill development', 'skill template', 'skill best practices', 'write SKILL.md'."
---

# Skill Forge

IRON LAW: Every line in a skill must justify its token cost. If it doesn't make the model's output better, more consistent, or more reliable — cut it.

## What is a Skill

A skill is an "onboarding guide" for Claude — transforming it from a general-purpose agent into a specialized one with procedural knowledge, domain expertise, and bundled tools.

```
skill-name/
├── SKILL.md           # Required: workflow + instructions (<500 lines)
├── scripts/           # Optional: deterministic, repeatable operations
├── references/        # Optional: loaded into context on demand
└── assets/            # Optional: used in output, never loaded into context
```

**Default assumption: Claude is already very smart.** Only add what Claude doesn't already know. Challenge every paragraph: "Does this justify its token cost?"

## Workflow

Copy this checklist and check off items as you complete them:

```
Skill Forge Progress:

- [ ] Step 1: Understand the Skill ⚠️ REQUIRED
  - [ ] 1.1 Clarify purpose and concrete use cases
  - [ ] 1.2 Collect 3+ concrete usage examples
  - [ ] 1.3 Identify trigger scenarios and keywords
- [ ] Step 2: Plan Architecture
  - [ ] 2.1 Identify reusable resources (scripts, references, assets)
  - [ ] 2.2 Design progressive loading strategy
  - [ ] 2.3 Design parameter system (if applicable)
- [ ] Step 3: Initialize ⛔ BLOCKING (skip if skill already exists)
  - [ ] Run init_skill.py
- [ ] Step 4: Write Description
  - [ ] Load references/description-guide.md
  - [ ] Apply keyword bombing technique
- [ ] Step 5: Write SKILL.md Body
  - [ ] 5.1 Set Iron Law
  - [ ] 5.2 Design workflow checklist
  - [ ] 5.3 Add confirmation gates
  - [ ] 5.4 Add parameter system (if applicable)
  - [ ] 5.5 Apply writing techniques
  - [ ] 5.6 Add anti-patterns list
  - [ ] 5.7 Add pre-delivery checklist
- [ ] Step 6: Build Resources
  - [ ] 6.1 Implement and test scripts
  - [ ] 6.2 Write reference files
  - [ ] 6.3 Prepare assets
- [ ] Step 7: Review ⚠️ REQUIRED
  - [ ] Run pre-delivery checklist (Step 9)
  - [ ] Present summary to user for confirmation
- [ ] Step 8: Package
  - [ ] Run package_skill.py
- [ ] Step 9: Iterate based on real usage
```

## Step 1: Understand the Skill ⚠️ REQUIRED

Ask yourself:
- What specific problem does this skill solve that Claude can't do well on its own?
- What would a user literally type to trigger this skill?
- What are 3-5 concrete usage examples with realistic inputs and expected outputs?

If unclear, ask the user (don't ask everything at once — start with the most critical):
- "Can you give me 3 examples of how you'd use this skill?"
- "What would you literally say to trigger it?"
- "What does a good output look like?"

Do NOT proceed until you have at least 3 concrete examples.

## Step 2: Plan Architecture

For each concrete example, ask:
1. What operations are deterministic and repeatable? → `scripts/`
2. What domain knowledge does Claude need at specific steps? → `references/`
3. What files are used in output but not in reasoning? → `assets/`

Key constraints:
- SKILL.md must stay under 500 lines — everything else goes to `references/`
- References organized by domain, one level of nesting only
- Load references/architecture-guide.md for progressive loading patterns and organization strategies

## Step 3: Initialize ⛔ BLOCKING

Skip if working on an existing skill. Otherwise run:

```bash
python3 scripts/init_skill.py <skill-name> --path <output-directory>
```

The script creates a template with Iron Law placeholder, workflow checklist, and proper directory structure.

## Step 4: Write Description

This is the most underestimated part of a skill. The description determines:
1. Whether the skill triggers automatically
2. Whether users find it by search

Load references/description-guide.md for the keyword bombing technique and good/bad examples.

Key rule: NEVER put "When to Use" info in the SKILL.md body. The body loads AFTER triggering — too late.

## Step 5: Write SKILL.md Body

Load reference files as needed for each sub-step:

### 5.1 Set Iron Law

Ask: "What is the ONE mistake the model will most likely make with this skill?"
Write a rule that prevents it. Place it at the top of SKILL.md, right after the frontmatter.

→ Load references/writing-techniques.md for Iron Law patterns and red flag signals.

### 5.2 Design Workflow Checklist

Create a trackable checklist with:
- ⚠️ REQUIRED for steps that must not be skipped
- ⛔ BLOCKING for prerequisites
- Sub-step nesting for complex steps
- (conditional) for steps that depend on earlier choices

→ Load references/workflow-patterns.md for checklist patterns and examples.

### 5.3 Add Confirmation Gates

Force the model to stop and ask the user before:
- Destructive operations (delete, overwrite, modify)
- Generative operations with significant cost
- Applying changes based on analysis

→ Load references/workflow-patterns.md for confirmation gate patterns.

### 5.4 Add Parameter System (if applicable)

If the skill benefits from flags like `--quick`, `--style`, `--regenerate N`:

→ Load references/parameter-system.md for $ARGUMENTS, flags, argument-hint, and partial execution patterns.

### 5.5 Apply Writing Techniques

Three techniques that dramatically improve output quality:

1. **Question-style instructions**: Give questions, not vague directives
2. **Anti-pattern documentation**: List what NOT to do
3. **Iron Law + Red Flags**: Prevent the model from taking shortcuts

→ Load references/writing-techniques.md for all three with examples.

### 5.6 Add Anti-Patterns List

Ask: "What would Claude's lazy default look like for this task?" Then explicitly forbid it.

→ Load references/writing-techniques.md for anti-pattern examples.

### 5.7 Add Pre-Delivery Checklist

Add concrete, verifiable checks. Each item must be specific enough that the model can check it by looking at the output. Not "ensure good quality" but "no placeholder text remaining (TODO, FIXME, xxx)."

→ Load references/output-patterns.md for checklist patterns and priority-based output.

### Writing Principles

- **Concise**: Only add what Claude doesn't already know
- **Imperative form**: "Analyze the input" not "You should analyze the input"
- **Match freedom to fragility**: Narrow bridge → specific guardrails; open field → many routes
  - High freedom (text): multiple valid approaches
  - Medium (pseudocode/params): preferred pattern, some variation OK
  - Low (specific scripts): fragile operations, consistency critical





