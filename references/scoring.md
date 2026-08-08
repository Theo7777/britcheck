# Scoring

How a verdict is produced. The reviewer subagent loads this file alongside
`uk-rubric.md` before scoring anything.

## Two sub-scores, then take the minimum

Score every image on two independent dimensions, 1 to 10.

- **`uk_score`** – does this read as Britain? Driven entirely by `uk-rubric.md`.
- **`artefact_score`** – is it physically believable? Generic AI-defect scoring, no UK
  content at all.

Then:

```
overall = min(uk_score, artefact_score) - critical_penalty
```

**Use the minimum, not the average.** This is the whole design.

An image that is perfectly British with a hand melting mid-frame is not shippable. An
image that is anatomically flawless with an American fire hydrant in it is not
shippable either. Averaging would pass both at 7. The minimum refuses to trade one
dimension off against the other, which is how a person would actually judge it.

## Bands

| `overall` | Band | Ship |
| --- | --- | --- |
| 9-10 | ship | yes |
| 8 | ship, with minor notes | yes |
| 6-7 | weak | no, regenerate |
| 5 | reject | no |
| below 5 | reject, broken | no |

**Ship bar: `overall >= 8`.** Anything at 7 or below does not ship.

The bar sits at 8 rather than 7 deliberately. A 7 is an image with something noticeably
wrong that a viewer will eventually spot. Shipping sevens is how a body of work slowly
stops being believable.

## artefact_score: what to look for

Generic physical plausibility. No UK content here.

- Extra or missing fingers, fused or duplicated limbs
- Melted, fused, or impossible tools and objects
- Warped geometry: bent straight lines, walls meeting at impossible angles
- Impossible joins where two objects merge into one another
- Distorted faces, asymmetric eyes, teeth that do not resolve
- Text that dissolves into pseudo-letterforms
- Shadows falling in inconsistent directions, or objects with no shadow at all
- Reflections that do not match what is in front of the surface

## Severity

- **critical** – a ship-blocker on its own. Any hard fail from the rubric, severely
  broken anatomy, or an object that could not physically exist. A critical failure
  forces `ship: false` and pulls `overall` below 8 regardless of the sub-scores.
- **minor** – noticeable but not blocking. A small artefact in a corner, slightly off
  framing, one soft detractor.

## Confirmer arithmetic

Confirmers only ever help, and only near the boundary.

- Count the confirmers actually visible in the image.
- Three or more can lift a 7 to an 8, once. They cannot lift a 6, and they cannot
  override a hard fail or a critical.
- Absence of confirmers is never a penalty. A tight interior crop shows almost none of
  them, and that is a fact about the framing, not the image's Britishness.

Record the confirmers found in the `confirmers` array, so the lift is auditable rather
than a hidden nudge.

## Score blind

If a filename already carries a score (`-s8`, `-s9`, and so on), that is a previous
human verdict. Read it only to populate the `agreement` column in the report. **Never
let it influence the score.** Form the verdict from the image, then compare.

## Output schema

The reviewer returns exactly one JSON object per file, and nothing else.

```json
{
  "file": "street-01.png",
  "uk_score": 9,
  "artefact_score": 5,
  "overall": 5,
  "band": "reject",
  "ship": false,
  "confirmers": ["double yellow lines", "wheelie bins", "external soil stack"],
  "failing_checks": [
    {
      "check": "hand_anatomy",
      "category": "artefact",
      "severity": "critical",
      "note": "Six fingers on the left hand; the grip does not close around the handle."
    }
  ],
  "fix": "Regenerate with the hands out of frame or holding a simpler object."
}
```

Field rules:

- `uk_score`, `artefact_score`, `overall` – integers, 1 to 10.
- `band` – one of `ship`, `weak`, `reject`.
- `ship` – boolean. True only when `overall >= 8` and no critical failure.
- `confirmers` – plain-language names of the confirmers found. Empty array is fine.
- `failing_checks` – empty array when nothing fails. Each entry names the `check`, its
  `category` (a rubric section name, or `artefact`), its `severity`, and a one-line
  `note` that says what is wrong and where.
- `fix` – one concrete, regeneration-ready instruction. Empty string when the file
  ships clean. Write it as an instruction to the image model, not as a description of
  the problem.

## Honest limits

State these rather than over-claiming.

- **Occlusion is not absence.** An image that shows no sockets is not un-British. Score
  what is visible, and say so in the note when a category could not be assessed.
- **Illegible text is not evidence.** Do not score section 11 on garbled signage.
- **The rubric describes a norm.** Britain has American-style detached houses, rural
  overhead poles, and open-plan extensions. A single strong tell in an otherwise
  coherent scene is worth a note, not a rejection.
- **Confidence belongs in the fix line.** When a call is genuinely borderline, say so
  there rather than inventing a decisive score.
