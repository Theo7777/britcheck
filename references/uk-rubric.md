# The UK rubric

The single source of truth for what makes an image read as British. The reviewer
subagent loads this file before scoring anything.

AI image models train overwhelmingly on American and global-web data, so their default
output is American. This rubric works in two directions: it catches the American
defaults that leak through, and it rewards the details that only appear in Britain.

## How the tiers work

**Detractors** cap `uk_score`.

| Tier | Effect | Meaning |
| --- | --- | --- |
| Hard fail | Caps `uk_score` at 3 | Effectively nonexistent in Britain |
| Strong tell | Caps `uk_score` at 6 | Possible, but wrong-looking |
| Soft detractor | Costs one point | Slightly off |

**Confirmers** lift a borderline score. Three or more present can raise a 7 to an 8.
Absence never penalises: a tight interior crop legitimately shows none of them, and an
image should not be punished for its framing.

Only the worst detractor in a category applies. Two hard fails in the same category do
not cap twice, but a hard fail in two different categories is a strong signal that the
model has produced an American scene wearing a British label, and should be noted in
the fix line.

---

## 1. Electrical and interior fittings

The highest-signal category. British electrical fittings are visually distinct and
regulated into shapes no American model produces by accident.

| Check | Tier | British | American default |
| --- | --- | --- | --- |
| Plug socket | Hard fail | Three rectangular pins, switch on the socket | Two flat vertical pins, unswitched |
| Socket in a bathroom | Hard fail | None. Shaver socket only | Standard outlets by the basin |
| Bathroom light switch | Hard fail | Pull cord, or a switch outside the door | Wall switch inside the bathroom |
| Ceiling lighting | Strong tell | Pendant on a ceiling rose with a shade | Recessed downlights throughout |
| Cable | Strong tell | Flat grey twin-and-earth; brown, blue, green-yellow cores | Romex; black, white, green |
| Air conditioning | Strong tell | Absent. No external condenser unit | Central air, visible vents or condenser |
| Wall radiator | Confirmer (strong) | Panel radiator under the window, TRV on the end | Floor or ceiling air register |
| Consumer unit | Confirmer | White plastic, row of RCD and MCB switches, hallway or under the stairs | Grey metal panel in a basement |
| Boiler | Confirmer | Combi on the kitchen or utility wall | Basement furnace and tank |

## 2. Kitchen, bathroom, and layout

| Check | Tier | British | American default |
| --- | --- | --- | --- |
| Fridge | Strong tell | Slim, under-counter or tall and narrow | Double-door with ice and water dispenser |
| Room layout | Strong tell | Compartmentalised. Separate small rooms with doors | Open-plan great room |
| Washing machine | Confirmer (strong) | Under the kitchen counter | Separate laundry room |
| Bathroom | Confirmer | Shower over the bath with a screen; chrome towel rail | Separate walk-in stall |
| Kettle | Confirmer | On the counter. Near universal | Absent |
| Basin | Soft | Pedestal sink, no counter | Vanity unit with a counter |
| Taps | Soft | Separate hot and cold pillar taps in older homes | Single mixer everywhere |
| Storage | Soft | Freestanding wardrobe | Built-in or walk-in closet |

## 3. Building fabric, roofline, and thresholds

| Check | Tier | British | American default |
| --- | --- | --- | --- |
| Insect screens on windows | Hard fail | None, ever | Screens on every opening |
| Wall texture | Hard fail | Wet-plaster skim, flat and smooth | Orange-peel or knockdown texture |
| Post-mounted kerbside mailbox | Hard fail | None. Letterbox slot in the door leaf | Mailbox on a post at the kerb |
| Exterior walls | Strong tell | Brick (red, London stock, buff), render, pebbledash | Vinyl or timber clapboard siding |
| Roof | Strong tell | Slate or concrete tile, pitched | Asphalt shingle |
| House form | Strong tell | Terraced or semi-detached, shared walls, bay windows | Detached with an open front lawn |
| Basement | Strong tell | Absent in most UK homes | Basement workshop or utility room |
| Chimney stack | Confirmer (strong) | Brick stack with several clay pots | – |
| Front door | Confirmer | Opens onto the pavement. No porch, no steps | Porch with steps and railings |
| Interior joinery | Confirmer | Skirting, architrave, picture rail, chimney breast | – |
| Windows | Soft | White uPVC casement, top-opening lights, trickle vents | Double-hung sash |
| Stairs | Soft | Narrow, steep, often turning. Loft hatch on the landing | Wide, straight, open |
| Floors | Soft | Carpet in bedrooms, on stairs and the landing | Hardwood throughout |

## 4. Drainage and rainwater goods

High signal, and rarely faked correctly, because American plumbing runs inside the
building envelope. Almost everything here is a confirmer: British drainage is visible
from the outside in a way American drainage is not.

| Check | Tier | British |
| --- | --- | --- |
| External soil stack | Confirmer (strong) | Black uPVC or cast iron running down the rear or side elevation |
| Gully at the downpipe base | Confirmer (strong) | Square metal grid over a trapped gully |
| Manhole cover | Confirmer (strong) | Square or rectangular cast iron, set into the pavement or front path |
| Guttering | Confirmer | Half-round or ogee uPVC into a downpipe at the corner |
| Road drainage | Confirmer | Kerbside gully grate set into the channel |
| Outside tap | Confirmer | Bibcock on the rear wall |
| Water meter | Soft | Small plastic cover set into the pavement |

## 5. Road markings and crossings

The most distinctive category in the rubric. British road markings have no American
equivalent, so a model cannot arrive at them by accident.

| Check | Tier | British |
| --- | --- | --- |
| Traffic lights hung on wires over the junction | Hard fail | UK lights are post-mounted at the stop line, on a white backing board |
| Double yellow lines | Confirmer (strong) | Continuous pair along the kerb |
| Zebra crossing | Confirmer (strong) | Black and white stripes with orange belisha beacons on striped poles |
| Zig-zag white lines | Confirmer (strong) | Either side of a crossing, or outside a school |
| Box junction | Confirmer | Yellow cross-hatching |
| Give-way markings | Confirmer | Painted triangle plus the inverted-triangle sign |
| Mini roundabout | Confirmer | Painted circle at a minor junction |
| Cats eyes | Soft | Reflective road studs along the centre line |
| Drop kerb, pedestrian refuge, guard railing | Soft | Common at junctions |

## 6. Street furniture and utilities

| Check | Tier | British | American default |
| --- | --- | --- | --- |
| Above-ground fire hydrant | Hard fail | None. Hydrants are underground, marked by a small yellow "H" plate | Red or yellow hydrant on every block |
| Dense web of cables crossing the carriageway | Soft | Weak evidence alone. Northern terraced streets genuinely look like this. Only counts alongside American housing and no road markings | Timber poles and sagging cables everywhere |
| A single pole with a service drop | Neutral | Normal, especially in the north and in older estates. Do not penalise | – |
| Green telecoms cabinet | Confirmer (strong) | Green box on the pavement with a peaked roof | – |
| Keep Left bollard | Confirmer (strong) | Illuminated box on a traffic island | – |
| Pillar box | Confirmer (strong) | Red cylinder with a royal cypher | Blue USPS box |
| TV aerial | Confirmer (strong) | H-shaped Yagi on the chimney stack | – |
| Gas or electric meter box | Confirmer (strong) | Cream or white plastic box on the front elevation | Meter on the side wall, no box |
| Road name plate | Confirmer | White rectangle, black text, mounted low on a wall, often with the postcode district | Green street sign on a pole |
| Burglar alarm box | Confirmer | White or red box high on the front elevation | Rare |
| Satellite dish | Confirmer | Bracketed to the front or side wall | – |
| Speed camera | Confirmer | Yellow box on a post | – |
| Pay-and-display machine | Confirmer | On the pavement beside marked bays | – |
| Street lighting | Soft | Tall column, often curved, reaching over the carriageway | Shorter, mounted on utility poles |
| Bollards, litter bins, grit bins | Soft | Grit bins yellow fibreglass | – |

## 7. Vehicles

| Check | Tier | British | American default |
| --- | --- | --- | --- |
| Steering wheel side | Hard fail | Right | Left |
| Road position | Hard fail | Driving on the left | Driving on the right |
| Work vehicle | Hard fail | White panel van (Transit, Vivaro, Sprinter) | Pickup truck |
| Number plate | Strong tell | White front, yellow rear, black text | Coloured state plate |
| Street parking | Soft | Narrow street, cars part-parked on the pavement | Wide kerbside bays |

## 8. Boundaries, gardens, and waste

| Check | Tier | British |
| --- | --- | --- |
| Wheelie bins | Confirmer (strong) | Several colours by recycling stream, out on the pavement |
| Fence panels | Confirmer (strong) | Larchlap or waney-edge panels between concrete posts |
| Estate agent board | Confirmer | For Sale or To Let on a post outside a house |
| Skip | Confirmer | Yellow or orange, on the road, with permit lights |
| Front boundary | Confirmer | Low brick wall or privet hedge, small garden |
| Rear garden | Confirmer | Small, patio, timber shed with an apex roof, rotary airer |
| Food waste caddy | Confirmer | Small kerbside caddy |
| Conservatory | Confirmer | Glazed rear extension |
| Lawn | Soft | Less manicured, mossy, no sprinkler system |

## 9. Clothing and PPE

| Check | Tier | British | American default |
| --- | --- | --- | --- |
| Hi-vis | Strong tell | Yellow or orange vest, common even on domestic jobs | Absent |
| Layers | Confirmer | Dressed for cold and wet | Short sleeves, sun |
| Work trousers | Soft | Knee-pad pockets | Plain denim |
| Tool carry | Soft | Tool bag or bucket, hi-vis over layers | Leather belt with braces |
| Footwear | Soft | Rigger boots or safety trainers | Tan work boots |

## 10. Light, weather, and planting

**Blue sky is not a failure.** Britain has bright summer days with hard shadows. An
early version of this rubric capped a perfectly good sunlit terraced street at 6 for
having one, which was wrong. What reads as American is the combination below, not the
sunshine on its own.

| Check | Tier | British | American default |
| --- | --- | --- | --- |
| High sun with bleached, arid ground | Strong tell | Even in summer the sun sits lower and the ground stays green | Overhead sun, dry bleached surfaces |
| Light quality | Soft | Overcast and diffuse is the safest default, not the only valid one | — |
| Ground | Confirmer | Damp tarmac, puddles, moss on north-facing brick and paving | Dry, bleached |
| Vegetation | Soft | Dense green and lush, deciduous | Arid, palms, xeriscape |
| Sky | Soft | Grey and flat, no visible sun disc | – |
| Windows | Soft | Condensation in winter | – |

## 11. Text, signage, and the high street

Score this category only when the text is legible. AI renders most signage as
gibberish, and unreadable text is not evidence either way.

| Check | Tier | British |
| --- | --- | --- |
| Currency | Hard fail if wrong | £, not $ |
| Spelling | Strong tell | colour, tyre, licence, kerb, metre |
| Speed signs | Strong tell | White circle, red ring, mph |
| Route signs | Confirmer | Blue motorway, green primary, white local. A roads black on white. Transport typeface |
| Shopfront | Confirmer | Narrow terraced unit, fascia sign, roller shutter, A-board on the pavement |
| Pub | Confirmer | Hanging sign, painted frontage, hanging baskets |
| Trade accreditation | Confirmer | Gas Safe, NICEIC, TrustMark, Checkatrade |
| Phone format | Soft | 07xxx mobile, not (555) |
| Payment | Soft | Contactless terminal, no tipping screen |
| Hot drink | Soft | Mug of tea, not an oversized to-go cup |

## What this rubric deliberately does not check

**Nothing about the appearance, ethnicity, or physical features of people.**

Judging Britishness from how someone looks is both wrong and unreliable. Britain looks
like everyone. This rubric scores the built environment, objects, clothing, light, and
text, and nothing else.

Human figures are assessed only under `artefact_score`, for finger count, morphing, and
warped anatomy. That is a question about physics, not nationality.

## Regional note

The rubric describes the British norm, weighted towards England, where most housing
stock and most commissioned imagery sits. Scotland, Wales, and Northern Ireland vary in
building fabric (harling and render in Scotland, slate and stone in Wales), and rural
Britain legitimately has overhead poles and no double yellow lines.

When an image is clearly rural or clearly Scottish, apply the confirmers that fit and
ignore the ones that do not. Do not dock a Highland farmhouse for lacking a green
telecoms cabinet.
