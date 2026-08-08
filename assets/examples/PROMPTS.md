# Example set – generation prompts

Twelve images, each isolating one behaviour of the rubric, so the README can show the
gate working rather than describing it.

Generate each prompt, save with the filename given, then run britcheck over the folder
and save the output as `verdicts.json`. Ship the images and their verdicts together.

For the failure cases the American element is named explicitly in the prompt. That is
deliberate – the point is a reliable fixture, not a fair test of the model.

---

## 1. Clean pass, exterior

`01-terrace-clean-pass.png` – expect `uk 9 / artefact 9 / overall 9 / ship`

> A row of Victorian red brick terraced houses on a British street, overcast grey sky,
> flat diffuse light, wet tarmac. Slate pitched roofs with brick chimney stacks and
> clay pots. H-shaped TV aerials. Black uPVC downpipes into square drain gullies.
> Double yellow lines along the kerb. Green and black wheelie bins on the pavement. A
> green telecoms cabinet. Low brick garden walls. White uPVC windows. Documentary
> photograph, 35mm, natural light.

## 2. Clean pass, interior

`02-interior-clean-pass.png` – expect `uk 9 / artefact 9 / overall 9 / ship`

> A small British kitchen in a terraced house. A washing machine built in under the
> worktop. A white kettle on the counter. A white three-pin switched socket on the
> wall above the worktop. A panel radiator under the window. A combi boiler on the
> wall. Grey overcast daylight through the window. Documentary photograph, natural
> light, slightly worn and lived in.

## 3. Hard fail – plug socket

`03-fail-socket.png` – expect `uk 3 / artefact 9 / overall 3 / reject`

> A small British kitchen in a terraced house, kettle on the counter, panel radiator
> under the window. On the wall above the worktop, an American two-pin unswitched
> electrical outlet, clearly visible in the foreground. Documentary photograph,
> natural light.

## 4. Hard fail – vehicle

`04-fail-pickup.png` – expect `uk 3 / artefact 9 / overall 3 / reject`

> A British semi-detached brick house with a slate roof, overcast sky. A large
> American pickup truck parked on the driveway, left-hand drive, US licence plate.
> Documentary photograph, 35mm.

## 5. Hard fail – screens and wall texture

`05-fail-screens-texture.png` – expect `uk 3 / artefact 9 / overall 3 / reject`

> An interior room with insect screens fitted to the windows and heavily textured
> orange-peel plaster walls. A panel radiator under the window. Overcast daylight.
> Documentary photograph.

## 6. Hard fail – kerbside mailbox

`06-fail-mailbox.png` – expect `uk 3 / artefact 9 / overall 3 / reject`

> The front of a British red brick terraced house with a low garden wall, overcast
> sky. An American mailbox on a wooden post at the kerb with the flag raised.
> Documentary photograph, 35mm.

## 7. Hard fail – hydrant and overhead traffic lights

`07-fail-hydrant-lights.png` – expect `uk 3 / artefact 9 / overall 3 / reject`

> A British high street with terraced shopfronts and grey overcast sky. A red American
> fire hydrant on the pavement in the foreground. Traffic lights suspended on wires
> across the junction. Documentary photograph, 35mm.

## 8. Strong tell – building fabric

`08-tell-clapboard.png` – expect `uk 6 / artefact 9 / overall 6 / weak`

> An American suburban house clad in pale grey vinyl siding with a dark grey asphalt
> shingle roof and a covered front porch with steps and railings, standing on a British
> street under a grey overcast sky. Wheelie bins on the pavement, double yellow lines
> along the kerb, red brick terraced houses visible either side. Documentary photograph, 35mm.

## 9. Strong tell – overhead utilities

`09-tell-overhead-cables.png` – expect `uk 6 / artefact 9 / overall 6 / weak`

> A British red brick terraced street under grey overcast sky, wheelie bins and double
> yellow lines. Timber utility poles down both sides of the road with heavy overhead
> cables crossing above the street. Documentary photograph, 35mm.

## 10. Strong tell – light

`10-tell-harsh-sun.png` – expect `uk 8 / artefact 9 / overall 8 / ship`

> A British red brick terraced street with slate roofs, chimney pots, wheelie bins and
> double yellow lines, under a cloudless deep blue sky with harsh direct overhead
> sunlight casting hard black shadows. Documentary photograph, 35mm.

## 11. The min() rule

`11-min-rule-hands.png` – expect `uk 9 / artefact 4 / overall 4 / reject`

> Close up of a person's hands holding a mug of tea on a windowsill in a British
> terraced house, panel radiator below, overcast grey light through the window. The
> left hand has six fingers, clearly visible, and the grip does not close around the
> mug. Documentary photograph, natural light.

**This is the example that sells the repo.** High `uk_score`, low `artefact_score`,
`overall` follows the minimum. An average would have shipped it at 6.

## 12. Confirmers lifting a borderline

`12-confirmers-lift.png` – expect `uk 8 / artefact 9 / overall 8 / ship`

> A plain, featureless residential street under flat grey overcast light, with no
> distinctive architecture. Double yellow lines along the kerb, a square cast iron
> manhole cover in the pavement, three coloured wheelie bins out for collection, and a
> square drain gully at the base of a black downpipe. Documentary photograph, 35mm.

An otherwise unremarkable scene that reaches the ship bar on confirmers alone. Run it
once with the confirmers cropped out to show it landing on 7, and once whole to show
it reaching 8.

---

## After generating

```bash
# from the britcheck repo root
britcheck assets/examples
```

Save the resulting JSON as `assets/examples/verdicts.json`, and check each verdict
against the expected values above. Where the tool disagrees with the expectation, the
tool is usually right and the prompt did not produce what was asked for. Regenerate the
image rather than adjusting the rubric to fit it.
