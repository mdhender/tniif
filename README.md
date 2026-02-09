# TNIIF  
**TribeNet Inferred Input Format**

TNIIF defines the JSON input format expected by the **Render** stage of OttoMap.

It represents a *curated, inferred subset* of data extracted from a parsed TribeNet turn report.  
TNIIF is intentionally simpler and more explicit than the raw report and is designed to be:

- Stable across report variations
- Suitable for manual inspection and editing
- Reusable by tools other than OttoMap
- Independent of the parsing process

The Render command consumes **only** TNIIF.  
The Parser produces TNIIF.

---

## Motivation

Original TribeNet turn reports are:

- Human-oriented
- Inconsistently formatted
- Occasionally contradictory
- Edited by hand by the GM
- Not designed to be machine-stable

TNIIF exists to draw a hard boundary between *interpretation* and *presentation*.

Once data reaches TNIIF:
- Ambiguities have been resolved (or explicitly flagged)
- Identifiers are normalized
- Implicit relationships are made explicit
- Rendering logic no longer depends on report text

---

## Scope

TNIIF is:

✅ A **subset** of the full report data  
✅ Focused on **map-relevant and render-relevant information**  
✅ Designed for **JSON-literate users to edit by hand**  
✅ Versioned and forward-compatible  

TNIIF is **not**:

❌ A complete archival format for turn reports  
❌ A lossless representation of report text  
❌ A parser output dump  
❌ A UI format  

If the Renderer needs it, it belongs here.  
If it only exists to explain *why* something happened, it probably doesn’t.

---

## Pipeline Overview

```text
TribeNet Turn Report
        │
        ▼
     Parser
        │
        ▼
     TNIIF (JSON)  ←─── editable, reusable, tool-facing
        │
        ▼
     Renderer
        │
        ▼
   Map / Output Artifacts
```

The Parser and Renderer are intentionally decoupled by TNIIF.

⸻

Design Principles

1. Inferred, Not Quoted

Values in TNIIF may be derived, inferred, or normalized.
They are not guaranteed to appear verbatim in the source report.

2. Explicit Over Clever

If a relationship or value is important for rendering, it must be explicit.
No hidden rules. No implied defaults.

3. Stable Identifiers

Names may change. IDs must not.
All cross-references use stable identifiers.

4. Human-Editable

A knowledgeable user should be able to:
	•	Fix a typo
	•	Resolve an ambiguity
	•	Correct a known report error
	•	Add missing but obvious data

…using nothing more than a text editor.

5. Renderer Is Dumb (On Purpose)

The Renderer:
	•	Does not parse prose
	•	Does not infer relationships
	•	Does not guess

If the Renderer needs logic, it belongs upstream.

⸻

Versioning

Each TNIIF document declares its schema version.

```json
{
  "tniif_version": "1.0.0",
  ...
}
```

•	Minor versions may add optional fields
	•	Major versions may remove or redefine fields
	•	The Renderer must reject incompatible versions

⸻

Error Handling & Uncertainty

Some report data is ambiguous or contradictory.

TNIIF supports this by:
	•	Allowing null where appropriate
	•	Allowing explicit uncertainty flags
	•	Preferring explicit unknowns over silent guesses

If a value is wrong, it should be obvious.
If a value is missing, it should be intentional.

⸻

Reuse by Other Tools

TNIIF is designed to be consumed by tools beyond OttoMap, such as:
	•	Validators
	•	Visualizers
	•	Analytics tools
	•	Alternate renderers
	•	Debugging and comparison utilities

No OttoMap-internal assumptions should leak into the format.

⸻

Non-Goals
	•	Preserving original report wording
	•	Encoding game rules
	•	Supporting speculative or hypothetical states
	•	Acting as a save file

⸻

Status

This specification is evolving.

Fields and structures will stabilize as:
	•	Additional reports are parsed
	•	Edge cases are encountered
	•	Rendering requirements become clearer

Breaking changes will be versioned explicitly.
