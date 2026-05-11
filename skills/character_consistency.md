# Skill: Character Consistency

## Purpose
Maintain stable identity of a character across:
- multiple scenes
- different camera angles
- different lighting conditions
- different generations (video/image/audio)

This skill is designed for AI video/image generation pipelines and must be used together with scene specs and references.

---

## Core Principle

The character is a **persistent identity object**, not a prompt description.

The AI must treat all character attributes as:
- locked (identity-critical)
- semi-flexible (style-adaptive)
- environment-dependent (lighting, mood only)

---

## 1. Identity Lock Rules (HARD CONSTRAINTS)

These attributes MUST NOT change between scenes:

- face structure (bone structure, jawline, nose shape)
- eye shape and spacing
- age range (±2 years max variation)
- ethnicity appearance consistency
- signature facial marks (scars, freckles, tattoos)
- body type (height, proportions)

If a generation deviates:
→ regenerate or correct, do not “accept drift”

---

## 2. Semi-Fixed Attributes (SOFT CONSTRAINTS)

These may adapt slightly but must remain recognizable:

- hairstyle (can change style, not identity shape)
- clothing (can vary by scene, but keep character identity coherent)
- expression style (emotion changes allowed)
- posture tendencies

---

## 3. Scene Adaptation Rules

Character must adapt to:
- lighting (neon, natural, dark, cinematic)
- weather (rain, fog, snow)
- camera angle (close-up, wide shot, motion)

BUT must remain visually identifiable in all conditions.

---

## 4. Cross-Scene Consistency Strategy

When generating multiple scenes:

1. Always reference a "canonical character base"
2. Reuse same face embedding / seed / reference image if available
3. Anchor description to fixed identity tokens

Example:
CHARACTER_ID: main_hacker_v1
REFERENCE: references/characters/main_hacker/


---

## 5. Prompt Injection Rule

When used in prompts for AI models:

Always prepend:

"Maintain strict character consistency based on CHARACTER_ID."

And include:

- reference image (if available)
- character spec summary
- identity lock section

---

## 6. Multi-Model Compatibility Layer

### For text-to-video models (e.g. Kling, Runway, Veo)
- prioritize visual descriptors
- repeat identity anchors per scene
- avoid re-describing character differently each time

### For LLM agents (e.g. Grok, GPT, Claude)
- treat this skill as a reasoning constraint
- enforce consistency in prompt generation
- validate scene outputs against character spec

### For image models (e.g. SDXL, Midjourney, Flux)
- always include fixed face descriptors
- reuse seed / reference images when possible

---

## 7. Anti-Drift Rules

Reject outputs if:
- face changes significantly between scenes
- identity becomes "generic person"
- character loses recognizability
- clothing becomes identity-defining incorrectly

Preferred action:
→ regenerate scene prompt with stronger constraints

---

## 8. Character Definition Dependency

This skill requires a separate file:

/specs/characters.md

Must include:
- canonical description
- reference images
- personality traits
- physical constraints

---

## 9. Example Usage in Workflow

workflow:
  - load character spec
  - apply character_consistency skill
  - attach reference images
  - generate scene prompt
  - validate identity stability

---

## Output Goal

All generated scenes must satisfy:

"Same character, different situation."
NOT
"Similar character each time."