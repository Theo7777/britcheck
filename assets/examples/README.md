# The example set

Thirteen images, each isolating one behaviour of the rubric, with the verdict britcheck
gave it in `verdicts.json`.

Generated across two vendors on purpose: **seven with OpenAI `gpt-image-2`, six with
Google `gemini-3.1-flash-image-preview` (Nano Banana 2)**. A gate tuned to one
generator's quirks is not a gate.

| # | File | Model | UK | Artefact | Overall | Shows |
| --- | --- | --- | --- | --- | --- | --- |
| 01 | terrace-clean-pass | gpt-image-2 | 9 | 9 | **9** | Clean pass, exterior |
| 02 | interior-clean-pass | nano-banana-2 | 9 | 9 | **9** | Clean pass, interior |
| 03 | fail-socket | nano-banana-2 | 3 | 8 | **3** | Hard fail, US outlet |
| 04 | fail-pickup | gpt-image-2 | 3 | 9 | **3** | Hard fail, US pickup |
| 05 | fail-screens-texture | nano-banana-2 | 3 | 9 | **3** | Hard fail, screens and wall texture |
| 06 | fail-mailbox | gpt-image-2 | 3 | 9 | **3** | Hard fail, kerbside mailbox |
| 07 | fail-hydrant-lights | nano-banana-2 | 3 | 8 | **3** | Two hard fails, two categories |
| 08 | tell-clapboard | gpt-image-2 | 6 | 9 | **6** | Strong tell, American building fabric |
| 09 | tell-overhead-cables | gpt-image-2 | 8 | 9 | **8** | Soft detractor, then confirmers lift it back |
| 10 | tell-harsh-sun | nano-banana-2 | 8 | 9 | **8** | Soft detractor only. Ships |
| 11 | min-rule-hands | nano-banana-2 | 9 | 4 | **4** | The minimum rule |
| 12 | confirmers-lift | gpt-image-2 | 8 | 9 | **8** | Confirmers lifting a 7 to an 8 |
| 13 | regional-weatherboard | gpt-image-2 | 9 | 9 | **9** | Regional edge case. Ships |

## The three that matter

**11 is the argument for the minimum rule.** A genuinely British scene: sash window with
peeling paint, column radiator below the sill, terraced houses through the glass, mug of
tea, damp overcast light. It scores 9 on Britishness. It also has six digits on the hand
holding the mug and two hands merging at the wrist, so it scores 4 on artefacts. The
minimum gives 4 and a regeneration. An average would have given 6.5 and shipped it.

**12 is the argument for confirmers.** Rendered terraces with no distinctive
architecture, nothing identifying in the built form. On detractors alone it sits at 7 and
fails. Eight confirmers — double yellows, a cast iron manhole cover, four coloured bins,
a gully grate at the downpipe — carry it to 8.

**13 changed the rubric, and is kept as a warning.** It was generated as a failure
fixture for timber cladding. What came back was white weatherboard over a plain clay tile
roof with a brick chimney, which is genuine Kent, Essex, and Sussex vernacular. A rubric
that read timber cladding as automatically American would reject a real British house.
Compare it with 08, which is the actual American form: vinyl siding, asphalt shingle, and
a covered porch with steps, sitting between two brick terraces.

## What the fixtures changed

Building this set corrected the rubric twice, both times because an image was right and
the rubric was wrong.

1. **Blue sky stopped being a strong tell.** Image 10 is a real-looking British summer
   street. Capping it at 6 for having hard shadows would reject legitimate work. What
   actually reads as American is high sun *with bleached, arid ground*, so that is the
   check now, and plain bright light is a soft detractor.

2. **Overhead cables were downgraded twice, and now barely count.** They started as a
   strong tell on the strength of UK stock contributors naming them as their top
   complaint. Image 10 has a pole and is plainly a real British street. Image 09 has
   poles on both sides and a dozen spans, and is *still* plainly a real British street,
   because that is what northern terraced streets look like. The check is now a soft
   detractor that only counts alongside American housing and absent road markings.

   This is the honest result: the rubric's object checks (sockets, hydrants, mailboxes)
   are strong, and its ambient checks (light, cables) are weak. Better to know that than
   to keep a tidy-looking check that rejects real work.

## Regenerating

Prompts are in `PROMPTS.md`. Where britcheck disagrees with the expectation written
there, check the image before the rubric — usually the prompt did not produce what was
asked for. Both corrections above came from doing that and finding the rubric at fault
instead, which is the less common outcome and worth pausing on when it happens.
