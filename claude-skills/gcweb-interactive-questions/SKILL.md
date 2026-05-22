---
name: gcweb-interactive-questions
description: >
  Generate and review GCWeb interactive question patterns (decision trees/wizards)
  for Canada.ca and CRA web pages. Use this skill whenever the user asks to create
  or review interactive questions, decision trees, wizards, or field flow components
  that guide people through a sequence of questions to find an answer.
---

# GCWeb Interactive Questions

You are creating or reviewing interactive questions for CRA/Canada.ca web pages. Interactive questions present people with a sequence of simple questions that leads to the specific answer they need.

## When to use

- Help people find an answer to their specific situation or condition
- Compliance requirements that depend on specific situations
- Applicability of rules based on user circumstances

## When to consider alternatives

If most answers can be reached in 1 or 2 questions, consider:

- Expand/hides (for mutually exclusive options)
- Tabs (alternative ways to get to the same answer)
- Eligibility checklists
- Lists or other content restructuring

## Fundamental principles

- Use clear and easy to understand questions and options
- Require as few questions as possible to reach an answer
- Prioritize the most common scenarios instead of covering every situation
- Content must already exist in a text format — interactive questions are the supportive feature

## Creating questions

### Focus on the majority

- Keep interactive questions as simple and short as possible
- Only include answers for the most common situations
- For uncommon situations, refer users to the full text version
- Get people to an answer as quickly as possible
- If most answers are found after the first question, move those situations out before the interactive questions

### Keep options unique

- Each option in a question should lead to different answers or next questions
- Use Yes/No/Unsure for questions where multiple options may have the same answer
- If multiple options lead to the same result, restructure the question

### Introducing on the page

- Heading should state the task (e.g. "Find out if you can renew your passport online")
- Avoid terms like "wizard", "decision tree", or "interactive questions" in the heading
- Provide a brief overview and instructions
- Add an opening statement: "Make a selection and then scroll down to see the next set of options or your result"

## Presenting questions

- Add important information **before** the interactive questions
- Use radio buttons as the default presentation method
- Fewer options + more questions is better than front-loading information
- When there are many options, use a select (dropdown)
- Order commonly chosen options toward the top, or sort alphabetically
- Keep Yes/No/Unsure order consistent
- Avoid introducing new terms within questions — explain them before
- Links within questions should open in a new tab

## Supportive statements

- Short plain text above or below the question: use `aria-describedby` or `aria-labelledby`
- Structured content (lists, links): place inside an expand/hide with a descriptive summary
- Information after the question: add an expand/hide below it

## Presenting answers

- Use alerts to show a conclusion: info (default), success (positive), warning (negative)
- Add a header starting with "The result is" or "Your answer is"
- Wrap answers in a block with `aria-live="polite"` for screen reader announcement
- Alternatively use `tabindex="0"` on the answer header for keyboard navigation

## Full text version

The text version can be displayed:

- In a tab design (interactive on first tab, full text on second)
- On a different page via a link near the interactive questions
- On the same page in a different location

## HTML structure

### Basic interactive questions with radio buttons

```html
<div class="wb-frmvld fieldflow-lg">
  <form>
    <div class="wb-fieldflow gc-font-2019" data-wb-fieldflow='{"noForm": true, "isoptional": true, "renderas":"radio", "gcChckbxrdio":true, "reset": {"action": "addClass", "source": ".alert-result", "class": "hidden"}}'>
      <!--Question 1 -->
      <div class="wb-fieldflow-header">
        <p class="wb-fieldflow-label">[Question 1]</p>
      </div>
      <ul>
        <li>Yes
          <div class="wb-fieldflow-sub gc-font-2019" data-wb-fieldflow='{"renderas": "radio", "gcChckbxrdio":true, "isoptional": true, "reset": {"action": "addClass", "source": ".alert-result", "class": "hidden"}}'>
            <!--Question 2 -->
            <div class="wb-fieldflow-header">
              <p class="wb-fieldflow-label">[Question 2]</p>
            </div>
            <ul>
              <li data-wb-fieldflow='{"action": "removeClass", "source": "#answer-2", "class": "hidden", "live": true}'>Yes</li>
              <li data-wb-fieldflow='{"action": "removeClass", "source": "#answer-3", "class": "hidden", "live": true}'>No</li>
            </ul>
          </div>
        </li>
        <li data-wb-fieldflow='{"action": "removeClass", "source": "#answer-1", "class": "hidden", "live": true}'>No</li>
      </ul>
    </div>
  </form>
</div>
<div id="answers" aria-live="polite">
  <!--Answer 1 -->
  <div id="answer-1" class="hidden alert alert-warning alert-result">
    <h3><small>The result is</small><br>[Answer 1 header]</h3>
    <p>[Answer 1 statement]</p>
  </div>
  <!-- Answer 2 -->
  <div class="hidden alert alert-info alert-result" id="answer-2">
    <h3><small>The result is</small><br>[Answer 2 header]</h3>
    <p>[Answer 2 statement]</p>
  </div>
  <!-- Answer 3 -->
  <div class="hidden alert alert-success alert-result" id="answer-3">
    <h3><small>The result is</small><br>[Answer 3 header]</h3>
    <p>[Answer 3 statement]</p>
  </div>
</div>
```

### With supportive statement after question

```html
<div class="wb-fieldflow-header">
  <p class="wb-fieldflow-label"><span aria-describedby="q1-desc">[Question]</span></p>
  <p id="q1-desc">[Supportive statement]</p>
</div>
```

### With expand/hide for complex information

```html
<div class="wb-fieldflow-header">
  <p class="wb-fieldflow-label">[Question]</p>
  <details>
    <summary>[Summary statement]</summary>
    <p>Content, links, tables, etc. go here.</p>
  </details>
</div>
```

### With lead-in statement before question

```html
<div class="wb-fieldflow-header">
  <p id="q2-desc">[Lead in statement]</p>
  <p class="wb-fieldflow-label"><span id="q2">[Question]</span></p>
</div>
```

## Key CSS classes

| Class | Purpose |
|-------|---------|
| `wb-frmvld` | Form validation wrapper |
| `fieldflow-lg` | Large field flow styling |
| `wb-fieldflow` | Main field flow component |
| `wb-fieldflow-sub` | Nested sub-question |
| `wb-fieldflow-header` | Question header container |
| `wb-fieldflow-label` | Question text label |
| `gc-font-2019` | GC 2019 font styling |
| `gcChckbxrdio` | GC styled checkboxes/radio buttons |
| `alert-result` | Answer alert styling |

## Alert types for answers

| Alert | Usage |
|-------|-------|
| `alert-info` | Default results |
| `alert-success` | Positive results |
| `alert-warning` | Negative results or action required |

## Validation checklist

1. Content exists in a full text format as the authoritative version
2. Questions use clear, plain language
3. Radio buttons used as default input method
4. Each option leads to a different answer or next question
5. Answers use appropriate alert type (info/success/warning)
6. Answer headers start with "The result is" or similar
7. Answers wrapped in `aria-live="polite"` container
8. Heading states the task, not "wizard" or "decision tree"
9. Code comments identify questions and answers for readability
