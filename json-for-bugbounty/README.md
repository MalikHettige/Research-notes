# Learn JSON for hunting

A self-built, curated path to actually understanding JSON — from syntax to schema validation to querying real API data. Built while training for bug bounty work (IDOR, auth flaws, business logic bugs), so the focus leans toward reading and manipulating real-world API responses, not just textbook syntax.

## Why this exists

Most "learn JSON" tutorials stop at syntax. This path goes further: real API responses, schema validation, and command-line querying — the actual skills needed to work with JSON at a professional level, including in a security research context.

## Path

### 1. Fundamentals
- [MDN — JSON](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/JSON) — core syntax and concepts
- [MDN — Test Your Skills: JSON](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Test_your_skills/JSON) — self-check exercises

### 2. Real API Data (no extra theory)
- [JSONPlaceholder Guide](https://jsonplaceholder.typicode.com/guide/)
- Read the raw JSON and break it down by hand:
  - [`/users/1`](https://jsonplaceholder.typicode.com/users/1)
  - [`/posts/1`](https://jsonplaceholder.typicode.com/posts/1)
  - [`/posts/1/comments`](https://jsonplaceholder.typicode.com/posts/1/comments)
  
  For each, write down (in your own words): which parts are objects, which are arrays, and which keys are nested.

### 3. Applied Practice
- [freeCodeCamp — JSON APIs and AJAX](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) — tiny hands-on challenges

### 4. JSON Schema (validation)
- [json-schema.org — Learn](https://json-schema.org/learn/)
- [Understanding JSON Schema — Basics](https://json-schema.org/understanding-json-schema/basics)
- [Getting Started Step-by-Step](https://json-schema.org/learn/getting-started-step-by-step)
- [Tour of JSON Schema — Your First Schema](https://tour.json-schema.org/content/01-Getting-Started/01-Your-First-Schema)
- [Miscellaneous Examples](https://json-schema.org/learn/miscellaneous-examples)
- [Understanding JSON Schema (full reference)](https://json-schema.org/understanding-json-schema/)

### 5. Querying JSON
- [JSONPath Viewer Guide](https://jsonpathviewer.com/guide)
- [JSONPath practice site](https://jsonpath.com/)
- [jq — command-line JSON processor](https://jqlang.org/)

## Notes

This repo is a living reference, not a finished product — entries get added as I work through them. Research notes live in a [separate repo](#) *(link here once added)*.

## Status

🟡 In progress

# Created: 10-09-2026
