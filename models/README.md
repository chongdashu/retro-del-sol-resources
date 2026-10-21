# Models

These are the Retro Diffusion 3D models that Retro del Sol ships, in the form the
game loads them. Most were tidied in Blender after download: real-world scale in
metres, one shared facing and a pivot at the bottom centre. The notes column says
what changed for each. `vehicle/postal-trike-provider.glb` is the trike exactly as
Retro Diffusion returned it, for comparison with the fitted `postal-trike.glb`.

Each model was generated from the prompt shown, with the style and size settings
in the table. Size sets how many texture pixels a model gets across its body, so
it decides how chunky the pixels look; it does not set the triangle count. Where
a reference image was sent with the prompt, it is in `references/`. Those images
were made with an image generator, not by Retro Diffusion.

| Model | File | Style | Size | Prompt | Reference | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| Yellow mailbox · 32 | [`mailbox/mailbox.glb`](mailbox/mailbox.glb) | Detailed | 32 | A sunny yellow coastal postal mailbox on a short cream stone plinth, rounded rectangular body, envelope emblem, front slot and small door. Low-poly pixel textures, teal trim. One isolated object, no ground plane. | — | Text-only generation. Normalized to 1.25 m in Blender; original geometry and pixel atlas preserved. |
| Courier · 64 · four clips | [`character/courier.glb`](character/courier.glb) | Detailed | 64 | One adult coastal courier, cream cap, teal vest over cream shirt, terracotta trousers, brown shoes, warm brown skin. Neutral A pose, clear separate limbs, simple low-poly pixel-painted forms. Match the four views of this same character. | [courier-turnaround-v1.webp](references/courier-turnaround-v1.webp) | RD3D rigid rig and four clips. Blender knee, ground-contact and rider-fit repairs. Sit-drive uses trike attachment space; shown unmounted here. |
| Postal trike — fitted rigid rig | [`vehicle/postal-trike.glb`](vehicle/postal-trike.glb) | Detailed | 64 | Sea-green electric postal TRICYCLE, exactly one front steering wheel and two rear wheels. Open cream seat, handlebars, footboard, black parcel rack with two tan parcels. Separate wheels from fenders. Match references. No rider. | [trike-turnaround.webp](references/trike-turnaround.webp) | Retro Diffusion geometry with its pieces moved in Blender to fit the rider: lower seat, handlebars brought back, footrests and a seat post added. No vehicle physics. |
| Postal trike — untouched RD export | [`vehicle/postal-trike-provider.glb`](vehicle/postal-trike-provider.glb) | Detailed | 64 | Sea-green electric postal TRICYCLE, exactly one front steering wheel and two rear wheels. Open cream seat, handlebars, footboard, black parcel rack with two tan parcels. Separate wheels from fenders. Match references. No rider. | [trike-turnaround.webp](references/trike-turnaround.webp) | Untouched source export; nominal native units and small ground offset, no rig. Same generation as the fitted trike. |
| Coral house · 128 | [`environment/coral-house.glb`](environment/coral-house.glb) | Detailed | 128 | One compact coral coastal cottage matching the reference: cream trim, terracotta gable roof, teal shutters, centered front door, low stone base. Entire freestanding house, no terrain, no scenery. Coarse pixel-painted textures. | [coral-house.webp](references/coral-house.webp) | Reference-guided building. Scale, ground pivot and opaque nearest-filtered materials normalized in Blender. |
| Harbor lamp · 64 | [`environment/harbor-lamp.glb`](environment/harbor-lamp.glb) | Detailed | 64 | Only the tall navy harbor lamppost from reference: square post, square foot, one right-angle arm and hanging rectangular warm-yellow lantern. Single freestanding lamp, no other objects, no ground. Coarse pixel textures. | [harbor-props.webp](references/harbor-props.webp) | Reference-guided street furniture. Normalized to 3.4 m height. |
| Dock bollard · 32 | [`environment/dock-bollard.glb`](environment/dock-bollard.glb) | Detailed | 32 | Only the short dock bollard from reference: stout wooden post, white cap, thick tan rope loop around it, square timber foot. Single freestanding object, no dock, no ground or other props. Coarse pixel-painted texture. | [harbor-props.webp](references/harbor-props.webp) | Reference-guided small prop. Normalized to 0.85 m height. |
| Harbor arrangement · Scene | [`environment/coastal-study.glb`](environment/coastal-study.glb) | Scene Detailed | kit | — | — | Arranged in Blender from the generated kit, props, mailbox and trike. |
| Generated layout · Scene | [`environment/coastal-lane-kit.glb`](environment/coastal-lane-kit.glb) | Scene Detailed | kit | Sunny coastal lane kit: broad cobbled road, curved pale-stone seawall, low quay, wooden dock modules, stairs, coral cottage walls, terracotta roofs, teal shutters, crates and planters. Clear driveable paths. Pixel textures, no characters or vehicles. | [harbor-lane.webp](references/harbor-lane.webp) | The layout the kit came with, normalized to metres. Mostly road tiles and one cottage; the generated sea and seawall pieces were not used here. |
| Post office · 128 | [`town/post-office.glb`](town/post-office.glb) | Detailed | 128 | Freestanding small coastal post office: cream stucco walls, teal window trim and door, terracotta tile roof, sunny yellow postal sign with envelope emblem above a parcel counter window. Whole building, no terrain. Coarse pixel textures. | — | Text-only landmark building. Normalized to 6.0 m height; opaque, emission off, provider light removed. |
| Cafe shop · 128 | [`town/cafe-shop.glb`](town/cafe-shop.glb) | Detailed | 128 | Freestanding two-storey coastal cafe shop: coral and cream stucco walls, striped teal-and-cream awning over the ground-floor window, small upper balcony with navy railing, terracotta roof. No terrain. Coarse pixel textures. | — | Text-only two-storey shop with striped awning. Normalized to 7.5 m height; awning valance cut-out texels filled opaque. |
| Townhouse · 128 | [`town/townhouse.glb`](town/townhouse.glb) | Detailed | 128 | Freestanding narrow two-storey cream townhouse: teal shutters on every window, teal front door, terracotta pitched roof with small chimney, navy door step. Tall and slim. No terrain or scenery. Coarse pixel textures. | — | Text-only narrow townhouse. Normalized to 7.5 m height. Grey painted roof patches read as missing tiles from a distance. |
| Lighthouse · 128 | [`town/lighthouse.glb`](town/lighthouse.glb) | Detailed | 128 | Freestanding cream harbor lighthouse: tall slender round tower on a stone base, navy and coral painted bands, small lantern room with navy cap and gallery railing, one door. No terrain, no sea. Coarse pixel textures. | — | Text-only harbor landmark. Normalized to 14 m height; lantern emission off. Coarsest texel density in the town (~8 texels/m). |
| Bench · 32 | [`town/bench.glb`](town/bench.glb) | Detailed | 32 | Wooden park bench matching the reference: three seat slats, two back slats, chunky dark navy iron legs with square feet. Single freestanding bench, no ground or other objects. Coarse pixel textures. | [bench.webp](references/bench.webp) | Reference crop of the harbor-props sheet. Size 32 instead of 64 to match building texel density. Normalized to 0.9 m height (1.25 m long). |
| Fence section · 32 | [`town/fence.glb`](town/fence.glb) | Detailed | 32 | Wooden railing fence section matching the reference: two square brown posts with cream caps, two horizontal rails. Single freestanding module, no ground or other objects. Coarse pixel textures. | [fence-section.webp](references/fence-section.webp) | Reference crop of the harbor-props sheet. Normalized to 1.1 m height, giving a 1.68 m module. |
| Potted lemon tree · 32 | [`town/lemon-tree.glb`](town/lemon-tree.glb) | Detailed | 32 | Potted lemon tree matching the reference: square terracotta pot, short brown trunk, chunky geometric green leaf clumps, three yellow lemons. Single freestanding plant, no ground. Coarse pixel textures. | [lemon-tree.webp](references/lemon-tree.webp) | Reference crop of the harbor-props sheet. Size 32 instead of 64 for texel density. Normalized to 1.6 m height. |
| Crate stack · 32 | [`town/crates.glb`](town/crates.glb) | Detailed | 32 | Stack of two weathered timber fish crates matching the reference: X-braced slat sides, dark corner studs, coiled tan rope on top. Single freestanding stack, no ground. Coarse pixel textures. | [crate-stack.webp](references/crate-stack.webp) | Reference crop of the harbor-props sheet. Normalized to 1.2 m height. |
| Palm tree · 64 | [`town/palm.glb`](town/palm.glb) | Detailed | 64 | One tall coastal palm tree: slender gently curved ringed tan trunk, crown of about eight long drooping green fronds, a few brown coconuts. Single freestanding tree, no ground, no sand. Coarse pixel textures. | — | Text-only. Normalized to 6.0 m. Provider frond cut-outs (alpha MASK) filled opaque for this runtime, so fronds read as solid blades. |
| Flower bush · 32 | [`town/flower-bush.glb`](town/flower-bush.glb) | Detailed | 32 | One low rounded flowering shrub: dense green leaf clumps covered in coral-pink and sunny-yellow blossoms, short woody base. Single freestanding plant, no pot, no ground. Coarse pixel textures. | — | Text-only. Normalized to 0.8 m height. |
| Shore rocks · 32 | [`town/rocks.glb`](town/rocks.glb) | Detailed | 32 | One shoreline rock cluster: three or four chunky weathered grey-tan boulders of different sizes huddled together, a few sea-green moss patches. Single freestanding group, no water, no sand. Coarse pixel textures. | — | Text-only. Normalized to 1.2 m height; masked texels filled opaque. |
| Fishing boat · 64 | [`town/boat.glb`](town/boat.glb) | Detailed | 64 | One small wooden fishing rowboat: teal hull with cream upper plank and coral stripe, two wooden bench seats, a pair of oars resting inside, coiled rope. Whole boat, no water, no dock. Coarse pixel textures. | — | Text-only. Normalized to 4.5 m long; bow toward +Z. |

The 16 files under `environment/tiles/` and `town/tiles/` are the tiles
of the Scene Detailed kit and its extension (road, sea, sea walls, quay steps,
beach, garden, plaza, pier, terrace stairs and walls, and a cottage). They snap
together on a grid and share one texture. `town/` also holds the buildings and
props placed on top of them.

## Viewing them

Any glTF viewer opens these files, for example Blender (File, Import, glTF 2.0) or
a browser viewer such as [gltf-viewer.donmccurdy.com](https://gltf-viewer.donmccurdy.com/).
The textures are pixel art and ask for nearest-neighbour filtering. A viewer that
smooths them will show a blurred model; that is the viewer, not the file.

## Using them

See [LICENSE](../LICENSE). Retro Diffusion's
[terms](https://www.retrodiffusion.ai/terms) do not allow its outputs to be used
to train, fine-tune or evaluate AI models, or collected into datasets for that, and
the same applies to these files.
