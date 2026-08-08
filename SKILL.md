---
name: britcheck
description: Reviews AI-generated images for British authenticity and physical realism. Scores each image 1-10 on two dimensions with a ship bar of 8, and reports which images ship and which to regenerate. Works on a single pasted image inline, or on a whole folder. Trigger when the user says "britcheck", "does this look British", "would this pass in the UK", "QA these images", "review this batch", "check these for UK realism", or shares a folder of AI-generated images set in Britain.
user-invocable: true
allowed-tools: Read, Write, Bash, Glob, Grep, Task
---

# britcheck

A quality gate for AI-generated images set in Britain.

Image models train mostly on American data, so British scenes come out subtly American:
clapboard siding, a mailbox on a post, a fire hydrant on the kerb, hard Californian sun
on a Manchester street. This skill catches that before the image ships.

The bar is **`overall >= 8`**.

## Load the rules first

Two reference files hold everything. Read them before scoring:

- `references/uk-rubric.md` – the eleven categories of British tell, tiered into hard
  fails, strong tells, soft detractors, and confirmers.
- `references/scoring.md` – the two sub-scores, the `min()` rule, bands, severity,
  confirmer arithmetic, and the output schema.

Do not restate the rules here or in the reviewer. One source of truth, so they cannot
drift apart.

## This is report-only

britcheck never renames, moves, deletes, or regenerates anything. It reads images and
writes a report. If the user wants files re-stamped with their new scores, hand them
paste-ready `mv` commands to run themselves.

## Two entry shapes

### A) A single image already in the conversation

Review it yourself in this session. One image is cheap and there is nothing to
parallelise. Read it, apply the rubric, and reply in plain language: `uk_score`,
`artefact_score`, `overall`, the band, ship yes or no, what failed, and the one-line
fix. Do not write a report file.

### B) A folder of images

1. **Enumerate.** Glob the folder for `*.png *.jpg *.jpeg *.webp`.

2. **Dispatch one reviewer per image, in parallel.** Launch the `britcheck-reviewer`
   subagent via the Task tool with the absolute file path. Send the batch of Task calls
   in a single message so they run concurrently.
   - Eight images or fewer: one image per reviewer.
   - More than eight: batch three images per reviewer, so the cost of loading the
     rubric is amortised across them. The reviewer returns an array in that case.

3. **Collect the verdicts.** Parse each reviewer's JSON. Extract it defensively – take
   the last `{ ... }` or `[ ... ]` block in the response, in case a reviewer adds a
   stray line of prose. Keep the main thread clean: you never read the images yourself,
   only the compact verdicts.

4. **Write the report.** Two files in the working directory, or wherever the user asks:
   - `<YYYY-MM-DD>-<folder-name>-britcheck.md`
   - `<YYYY-MM-DD>-<folder-name>-britcheck.json`

   Nothing is written into the reviewed folder itself.

5. **Print the summary block** to stdout.

## Output

### The JSON file

An array of the per-image objects exactly as the reviewers returned them. Schema is in
`references/scoring.md`.

### The markdown file

A table sorted by `overall` descending, then a detail block for every image below the
ship bar.

```markdown
# britcheck – uk-street-batch (2026-08-08)

**Source:** /Users/you/Downloads/uk-street-batch
**Reviewed:** 12  |  **Ship (8+):** 7  |  **Weak (6-7):** 3  |  **Reject (<6):** 2

| File | UK | Artefact | Overall | Band | Ship | Confirmers | Top issue |
|------|----|----------|---------|------|------|------------|-----------|
| terrace-01.png | 9 | 9 | 9 | ship | yes | 5 | – |
| kitchen-03.png | 3 | 9 | 3 | reject | no | 1 | US two-pin socket above the worktop |
| street-07.png | 9 | 5 | 5 | reject | no | 6 | Six fingers on the left hand |

## Below the ship bar

### kitchen-03.png – overall 3 (reject)
- `plug_socket` (critical): Two-pin unswitched outlet above the worktop, American pattern.
- **Fix:** Regenerate with a UK three-pin switched socket, white plastic, above the worktop.

### street-07.png – overall 5 (reject)
- `hand_anatomy` (critical): Six fingers on the left hand; the grip does not close.
- **Fix:** Regenerate with the hands out of frame or holding a simpler object.
```

When a filename already carries a score suffix (`-s8`), add an **agreement** column
comparing the computed band to it. Score blind: read that suffix only to compare, never
to influence the verdict.

### Summary block

```
=== BRITCHECK SUMMARY ===
Folder:      /Users/you/Downloads/uk-street-batch
Reviewed:    12
Ship (8+):   7
Weak (6-7):  3
Reject (<6): 2

Most common failure: US plug socket (4 images)
```

The "most common failure" line matters more than it looks. When one check fails across
a batch, the fix belongs in the generation prompt, not in twelve separate regenerations.

## Notes

- **Occlusion is not absence.** An image showing no sockets is not un-British. Score
  what is visible and say what could not be assessed.
- **Confirmers only ever help.** Three or more can lift a 7 to an 8. They never rescue
  a hard fail, and their absence is never a penalty.
- **No checks on how people look.** The rubric scores the built environment, objects,
  clothing, light, and text. Human figures are assessed only for AI artefacts. See the
  closing section of `references/uk-rubric.md`.
