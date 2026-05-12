# SCT_PE_3 - Prompting for Task Automation

**Track:** Prompt Engineering
**Internship:** SkillCraft Technology
**Task:** 03 of 04

---

## Objective

Apply prompting to semi-automate a useful real-world task. Design a clear, consistent prompt that can handle multiple inputs and produce reliable output. Submit the prompt, at least 3 input-output examples, and a short reflection on prompt iteration and debugging.

---

## Background: Prompting for Automation

Task automation with LLMs differs from creative prompting in one critical way: reliability matters more than originality. An automation prompt needs to produce the same structure every time, regardless of how the input is formatted. This requires a different design philosophy: explicit schemas, strict output rules, and no-inference constraints.

The goal is to make the model behave like a deterministic parser, not a creative writer.

---

## Chosen Task: Meeting Notes to Structured JSON

Converting raw, unstructured meeting notes into a consistent JSON format is a high-value automation task. It removes manual data entry, enables downstream processing (calendar sync, task trackers, CRM updates), and works reliably across different note styles.

This task was chosen because meeting notes vary enormously in format, from informal bullet points to structured formal minutes, making it a good stress test for prompt robustness.

---

## The Final Prompt

```
You are a meeting notes parser. Your job is to convert raw meeting notes
into a structured JSON object. Always return valid JSON and nothing else.
No explanation, no markdown code fences, no extra text.

Use this exact schema:
{
  "meeting_title": "string",
  "date": "YYYY-MM-DD or null if not mentioned",
  "attendees": ["list of names"],
  "agenda_items": ["list of topics discussed"],
  "decisions": ["list of decisions made"],
  "action_items": [
    {
      "task": "string",
      "owner": "string or null",
      "due_date": "YYYY-MM-DD or null"
    }
  ],
  "next_meeting": "YYYY-MM-DD or null"
}

Rules:
- Extract only what is explicitly stated. Do not infer or fabricate.
- If a field has no data, use null for strings or [] for arrays.
- Normalize all dates to YYYY-MM-DD format.
- If an owner is not assigned to an action item, set owner to null.

Meeting notes to parse:
[INSERT NOTES HERE]
```

---

## Input-Output Examples

### Example 1 - Informal Standup Notes

**Input:**
```
May 5 standup - Priya, Ravi, and Sam attended.
Discussed the login bug fix and the new dashboard design.
Ravi will push the bug fix by end of day. Sam to share design mockups
by May 7. No decisions made. Next standup same time Thursday.
```

**Output:**
```json
{
  "meeting_title": "Standup",
  "date": "2026-05-05",
  "attendees": ["Priya", "Ravi", "Sam"],
  "agenda_items": ["login bug fix", "new dashboard design"],
  "decisions": [],
  "action_items": [
    {
      "task": "Push the bug fix",
      "owner": "Ravi",
      "due_date": null
    },
    {
      "task": "Share design mockups",
      "owner": "Sam",
      "due_date": "2026-05-07"
    }
  ],
  "next_meeting": "2026-05-07"
}
```

---

### Example 2 - Formal Project Review

**Input:**
```
Project Review Meeting - 12th May 2026
Present: Ananya (PM), Kiran (Dev Lead), Meera (QA), Suresh (Client)

Agenda:
1. Sprint 3 progress update
2. Client feedback on UI
3. Release timeline discussion

Decisions:
- Release date pushed to June 1st due to QA backlog
- Client approved the new color scheme

Action Items:
- Kiran to complete API integration by May 20
- Meera to finish regression testing by May 28
- Ananya to send updated timeline to client by May 14

Next meeting: May 26, 2026
```

**Output:**
```json
{
  "meeting_title": "Project Review Meeting",
  "date": "2026-05-12",
  "attendees": ["Ananya", "Kiran", "Meera", "Suresh"],
  "agenda_items": [
    "Sprint 3 progress update",
    "Client feedback on UI",
    "Release timeline discussion"
  ],
  "decisions": [
    "Release date pushed to June 1st due to QA backlog",
    "Client approved the new color scheme"
  ],
  "action_items": [
    {
      "task": "Complete API integration",
      "owner": "Kiran",
      "due_date": "2026-05-20"
    },
    {
      "task": "Finish regression testing",
      "owner": "Meera",
      "due_date": "2026-05-28"
    },
    {
      "task": "Send updated timeline to client",
      "owner": "Ananya",
      "due_date": "2026-05-14"
    }
  ],
  "next_meeting": "2026-05-26"
}
```

---

### Example 3 - Sparse Notes with Missing Fields

**Input:**
```
Quick sync today. Just me and Deepak. Talked about the marketing budget
and social media calendar. No decisions yet. Deepak will draft the Q3
budget proposal. No deadline set. No next meeting scheduled.
```

**Output:**
```json
{
  "meeting_title": "Quick Sync",
  "date": null,
  "attendees": ["Deepak"],
  "agenda_items": ["marketing budget", "social media calendar"],
  "decisions": [],
  "action_items": [
    {
      "task": "Draft the Q3 budget proposal",
      "owner": "Deepak",
      "due_date": null
    }
  ],
  "next_meeting": null
}
```

---

### Example 4 - Multi-Team Planning Session

**Input:**
```
Cross-team planning - May 15, 2026
Attendees: Rohit, Fatima, James, Preethi, Arun

Topics covered: Q2 OKR review, hiring plan for engineering,
infrastructure cost reduction, and onboarding process redesign.

Decisions made:
- Approved 3 new engineering headcount for Q3
- Agreed to migrate to spot instances to cut infra costs by 30%

Tasks:
- Rohit to post job descriptions by May 19
- Fatima to coordinate with DevOps on migration plan, due May 22
- Preethi to revamp onboarding doc, no deadline
- James to present cost analysis at next meeting

Next session: June 2, 2026
```

**Output:**
```json
{
  "meeting_title": "Cross-team Planning",
  "date": "2026-05-15",
  "attendees": ["Rohit", "Fatima", "James", "Preethi", "Arun"],
  "agenda_items": [
    "Q2 OKR review",
    "hiring plan for engineering",
    "infrastructure cost reduction",
    "onboarding process redesign"
  ],
  "decisions": [
    "Approved 3 new engineering headcount for Q3",
    "Agreed to migrate to spot instances to cut infra costs by 30%"
  ],
  "action_items": [
    {
      "task": "Post job descriptions",
      "owner": "Rohit",
      "due_date": "2026-05-19"
    },
    {
      "task": "Coordinate with DevOps on migration plan",
      "owner": "Fatima",
      "due_date": "2026-05-22"
    },
    {
      "task": "Revamp onboarding doc",
      "owner": "Preethi",
      "due_date": null
    },
    {
      "task": "Present cost analysis at next meeting",
      "owner": "James",
      "due_date": null
    }
  ],
  "next_meeting": "2026-06-02"
}
```

---

## Prompt Iteration and Debugging Reflection

### Iteration 1 - The First Draft

The first version of the prompt was simply:

```
Convert these meeting notes to JSON: [notes]
```

The model produced JSON but with inconsistent key names across runs. Sometimes `actions` instead of `action_items`, sometimes `participants` instead of `attendees`. The schema was unpredictable, making downstream parsing impossible.

**Lesson:** Without a fixed schema, the model invents its own structure every time. For automation, schema consistency is non-negotiable.

---

### Iteration 2 - Adding the Schema

Adding the explicit JSON schema with field names and types solved the key naming problem. However, the model was still wrapping the output in markdown code fences and sometimes adding a sentence like "Here is the parsed JSON:" before the output.

This would break any code trying to parse the raw response with `JSON.parse()`.

**Lesson:** The model defaults to being helpful and conversational. You have to explicitly suppress that behavior for machine-readable output.

---

### Iteration 3 - Suppressing Prose

Adding "Always return valid JSON and nothing else. No explanation, no markdown code fences, no extra text." fixed the wrapping issue in most cases. However, on sparse inputs, the model was sometimes inferring data that was not in the notes, such as guessing a meeting title from context or filling in a due date based on "end of week."

**Lesson:** LLMs try to be helpful by filling gaps. For structured data extraction, fabrication is worse than a null value. A null is honest; a fabricated date is a silent bug.

---

### Iteration 4 - Adding the No-Inference Rule

Adding "Extract only what is explicitly stated. Do not infer or fabricate. If a field has no data, use null for strings or [] for arrays." resolved the hallucination issue. The model now correctly returns null for missing dates and empty arrays for missing lists.

**Lesson:** Explicit permission to return null is necessary. Without it, the model interprets empty fields as a problem to solve rather than a valid state to report.

---

### Final Prompt Stability

After 4 iterations, the prompt produces consistent, parseable JSON across all tested input styles: informal notes, formal minutes, sparse notes, and multi-team sessions. The three design decisions that made it reliable were the fixed schema, the no-prose instruction, and the explicit no-inference rule.

---

## Automation Use Cases

This prompt pattern can be extended to other structured extraction tasks:

| Input | Output Schema |
|-------|--------------|
| Job descriptions | Skills, requirements, salary, location |
| Customer support tickets | Category, priority, sentiment, action needed |
| Research paper abstracts | Problem, method, results, limitations |
| Invoice text | Vendor, amount, line items, due date |
| Bug reports | Component, severity, steps to reproduce, expected vs actual |

The same design principles apply: fixed schema, no-prose rule, no-inference rule.

---

## References and Further Reading

- Wei et al. (2022). Emergent Abilities of Large Language Models. https://arxiv.org/abs/2206.07682
- Mishra et al. (2022). Cross-Task Generalization via Natural Language Crowdsourcing Instructions. https://arxiv.org/abs/2104.08773
- OpenAI JSON Mode Documentation. https://platform.openai.com/docs/guides/structured-outputs

---

## Acknowledgements

SkillCraft Technology for the internship program and task design.

The open-source community for tooling and documentation around structured output generation with LLMs.

---

Back to main repository: [Skill_Craft_Tasks](../README.md)
