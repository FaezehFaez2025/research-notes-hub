# System Prompt vs User Prompt

When using LLMs, input is split into different **message types**. The two main ones:

---

## System Prompt

**What it is:** Instructions that define *how* the model should behave.

**Think of it as:** Giving the AI its job description before it starts working.

```
You are a helpful Python tutor. Always include code examples. Be concise.
```

**Key points:**
- Set once at the beginning
- Hidden from end users (in apps)
- Controls tone, role, constraints, output format

---

## User Prompt

**What it is:** The actual question or task from the human.

**Think of it as:** What you ask the AI to do right now.

```
How do I read a CSV file in pandas?
```

**Key points:**
- Changes every turn
- This is what you type into ChatGPT, Claude, etc.

---

## Quick Comparison

| | System Prompt | User Prompt |
|--|---------------|-------------|
| **Sets** | Model behavior | Specific task |
| **Written by** | Developer | End user |
| **Changes** | Rarely | Every turn |

---

## In Code

```python
messages = [
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "What is 2+2?"}
]
```

The system prompt shapes *how* the model answers. The user prompt is *what* it answers.

