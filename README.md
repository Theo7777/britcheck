# britcheck

A Claude Code skill that scores AI-generated images on whether they actually look
British, and refuses to ship the ones that do not.

Ask an image model for a British house and you will usually get an American one. The
training data is overwhelmingly American, so the defaults leak through: clapboard
siding, an asphalt shingle roof, a mailbox on a post, a red fire hydrant on the kerb,
hard Californian sun on a Manchester street. Researchers who prompted Stable Diffusion
XL for houses in different countries got back
[idealised American houses with trim lawns and porches](https://www.washingtonpost.com/technology/interactive/2023/ai-generated-images-bias-racism-sexism-stereotypes/)
regardless of which country they asked for.

Most of these images look fine until a British viewer glances at them, and then they
look wrong in a way that is hard to name. britcheck names it.

## What it does

Point it at an image or a folder. It returns a verdict.

```
=== BRITCHECK SUMMARY ===
Folder:      ~/Downloads/uk-street-batch
Reviewed:    12
Ship (8+):   7
Weak (6-7):  3
Reject (<6): 2

Most common failure: US plug socket (4 images)
```

Plus a per-image report with what failed and how to fix it in the next generation.

## How it scores

Two independent scores out of 10, then **take the worse of the two**.

- **`uk_score`** – does this read as Britain?
- **`artefact_score`** – is it physically believable? Fingers, geometry, shadows.

```
overall = min(uk_score, artefact_score)
```

Ship bar is 8.

The minimum is the whole design. A perfectly British street with a hand melting
mid-frame scores 9 on one dimension and 4 on the other. Averaging gives you 6.5 and a
shrug. The minimum gives you 4 and a regeneration. It refuses to let one dimension pay
for the other, which is how a person actually judges an image.

## The rubric

Eleven categories, in `references/uk-rubric.md`. Checks are tiered:

| Tier | Effect |
| --- | --- |
| **Hard fail** | Caps `uk_score` at 3. Effectively nonexistent in Britain |
| **Strong tell** | Caps `uk_score` at 6. Possible, but wrong-looking |
| **Soft detractor** | Costs a point |
| **Confirmer** | Lifts a borderline score. Never penalises when absent |

A few of the hard fails, to give you the flavour:

- **An above-ground fire hydrant.** Britain's hydrants are underground, marked by a
  small yellow "H" plate. A red hydrant on a residential street is the single most
  reliable American tell there is, and models produce them constantly.
- **A two-pin unswitched plug socket.** British sockets have three rectangular pins and
  a switch on the faceplate.
- **Traffic lights hung on wires over a junction.** UK lights are post-mounted at the
  stop line.
- **Insect screens on the windows.** British houses do not have them.
- **A mailbox on a post at the kerb.** Post comes through a slot in the door.
- **Orange-peel or knockdown wall texture.** British walls are wet-plaster skimmed flat.

And some of the confirmers, which are the details that only exist here:

- An external soil stack running down the back of the house
- A square cast iron manhole cover set into the front path
- Double yellow lines, zig-zags, a zebra crossing with belisha beacons
- A green telecoms cabinet on the pavement
- An H-shaped TV aerial on a chimney stack with clay pots
- Wheelie bins in three colours, out on the pavement
- A washing machine under the kitchen counter

Confirmers only ever help, and only near the boundary. Three or more can lift a 7 to an
8. Their absence is never a penalty, because a tight interior crop legitimately shows
none of them.

## What it does not check

**Nothing about the appearance, ethnicity, or physical features of people.**

Judging Britishness from how somebody looks is both wrong and unreliable. Britain looks
like everyone. This rubric scores the built environment, objects, clothing, light, and
text, and nothing else. Human figures are assessed only for AI artefacts – finger
count, morphing, warped anatomy – which is a question about physics, not nationality.

This is a deliberate design decision, not an oversight.

## Worked examples

**[See all 13 scored examples →](assets/examples/)** – every image with the prompt that
produced it and the verdict it got. Six ship, one weak, six reject.

Generated across two vendors on purpose: seven with OpenAI `gpt-image-2`, six with
Google Nano Banana 2. A gate tuned to one generator's quirks is not a gate.

### Why the minimum, and not the average

![The minimum rule](assets/examples/11-min-rule-hands.jpg)

**UK 9 · Artefact 4 → 4. Reject.**

Everything about this is British: the timber sash window with peeling paint, the column
radiator below the sill, the terrace through the glass, the mug of tea, the damp flat
light. It scores 9 on Britishness.

It also has six digits on the hand holding the mug, and the two hands merge at the
wrist. That is 4 on artefacts.

The minimum gives 4 and sends it back. An average would have given 6.5 and shipped it.

### Why confirmers exist

![Confirmers lifting a borderline](assets/examples/12-confirmers-lift.jpg)

**UK 8 · Artefact 9 → 8. Ships.**

Rendered terraces, no distinctive architecture, nothing identifying in the built form.
On detractors alone it sits at 7 and fails.

Then look at the ground: double yellow lines, a square cast iron manhole cover, three
wheelie bins in separate colours, and a gully grate at the foot of a black downpipe.
Eight confirmers carry it over the bar. None of them is the building.

### What the fixtures changed

Building the set corrected the rubric twice, both times because the image was right and
the rubric was wrong. Blue sky stopped being a strong tell, because Britain has summer.
Overhead cables were downgraded twice and now barely count, because northern terraced
streets genuinely look like that.

The finding underneath is worth more than either check was: **the object checks are
strong and the ambient checks are weak.** A socket, a hydrant, a mailbox is binary.
Light and cable density are not.

## Install

Clone into your skills directory.

```bash
# Personal, available in every project
git clone https://github.com/Theo7777/britcheck ~/.claude/skills/britcheck

# Or project-local
git clone https://github.com/Theo7777/britcheck .claude/skills/britcheck
```

Then copy the reviewer subagent alongside your other agents:

```bash
cp ~/.claude/skills/britcheck/agents/britcheck-reviewer.md ~/.claude/agents/
```

## Use

Paste an image into Claude Code and ask:

> does this look British?

Or point it at a folder:

> britcheck ~/Downloads/uk-street-batch

Single images are reviewed inline. Folders fan out one reviewer per image in parallel,
then collect the verdicts into a dated report.

## It never touches your files

britcheck reads images and writes a report. It does not rename, move, delete, or
regenerate anything. If you want files re-stamped with their new scores, it hands you
the `mv` commands to run yourself.

## Regional scope

The rubric describes the British norm, weighted towards England, where most housing
stock and most commissioned imagery sits. Scotland, Wales, and Northern Ireland differ
in building fabric, and rural Britain legitimately has overhead poles and no double
yellow lines. The reviewer is told to apply the confirmers that fit the scene and
ignore the ones that do not, rather than docking a Highland farmhouse for lacking a
green telecoms cabinet.

## Why it exists

I run AI content pipelines daily, and the thing that separates a working pipeline from
a demo is the gate at the end. Generation is commodity. Knowing when the output is not
good enough, and saying so consistently, is not.

This is that gate, for one specific failure mode I kept hitting.

## Licence

MIT. Do what you like with it.
