# Lessons from making Retro del Sol

Retro del Sol is a browser game set in a sunny harbour town. You play a courier who walks, rides a postal trike and sails between sixteen islands to deliver parcels. Play it at [retrodelsol.com](https://retrodelsol.com) or on [Wavedash](https://wavedash.com/games/retro-del-sol).

Every 3D model in the game came from Retro Diffusion 3D (RD3D), and coding agents wrote and tested the game from plain-language prompts. These are the lessons that kept coming up, or that cost the most rework when we got them wrong. [PROMPTS.md](PROMPTS.md) shows the requests themselves, and [models/](models/README.md) has the models with the prompt behind each one.

## Working with Retro Diffusion 3D

### Pick one size for each kind of object

RD3D's size setting controls how many texture pixels a model gets across its body, which decides how chunky its pixels look. It has no say over the triangle count. The size menu in the app works as a guide: 16 for a small item, 32 for a prop, 64 for a character or vehicle, 128 for a building, 256 for a landmark. Keep one size per kind of thing and one style across the game, and the pieces will look like they belong together.

We had planned the bench and lemon tree at 64. That would have given them about twice as many pixels per metre as the houses beside them, so we dropped them to 32 like the other small props. Shape detail is a separate matter: a size-32 mooring post came back with 1,028 triangles because its rope is modelled, and a size-128 cottage has 424. Every model used the Detailed style, and the ground used its kit version, Scene Detailed.

### Show it one object per picture, and say what to leave out

You can send up to four reference images with a request, and when we checked, prompts were capped at 250 characters. A picture that holds one object works best. Props that aren't in any picture are safer described in words alone, because the tool may copy whatever an unrelated image shows. End the prompt with what you don't want, so the object comes back on its own.

We made one sheet of six props with an image generator and cut it into single-object crops. The first crops for the fence and lemon tree caught a leaf and a bollard foot from their neighbours and had to be tightened. Every prompt ended the same way:

> Wooden park bench matching the reference: three seat slats, two back slats, chunky dark navy iron legs with square feet. Single freestanding bench, no ground or other objects. Coarse pixel textures.

Results follow the picture loosely. The harbour lamp came back with an arm that also sticks out behind the post, and we kept it. We didn't upload a palette, because a palette constrains every pixel of a model, the plants included. We never tried the palette tools, so we can't say how they compare.

### Take one small model all the way into the game first

Before you generate a whole set, push one small prop from prompt to download to Blender to the running game. That one trip turns up problems that would otherwise hit every model after it. Check the texture filtering in particular. RD3D files ask for nearest-neighbour filtering, which keeps each texture pixel a crisp square, and your engine has to honour that.

The mailbox went first. It looked sharp in Blender and smeared in the game, because the engine's model loader ignored the file's filter setting. Fixing the loader once, for both WebGPU and WebGL 2, fixed every model that came after. Watch re-exports too: Blender wrote the mailbox back out with mipmapped nearest filtering where the original file asked for plain nearest.

### Run every model through one clean-up script, and keep the download

Each model arrives with its own scale, facing and pivot. One Blender script that sets real metres, a shared front direction and a pivot at the bottom centre lets every model stand on whatever it's placed on. Keep the original download untouched and make every fix a script that runs on it. Then you can rebuild the game's copies at any time without generating anything again.

The mailbox came in 1.9375 Blender units tall and was scaled to 1.25 m. The palm fronds and rocks used cut-out transparency the engine can't draw, so the script filled those pixels from their solid neighbours. It fills unused texture space the same way, which keeps black from bleeding into the smaller mipmap levels. It also strips the models' built-in lights and glow, so the game decides when the lamps come on.

### Fix a model by moving its named pieces

An RD3D model arrives as a set of named parts. The trike's seat, fork and wheels are separate, and so are the courier's vest and hair. Most fixes mean moving one part while the paint stays as it was, and the same parts give you moving pieces and attachment points without any remodelling.

As generated, the trike's seat was a metre off the ground, too high for the courier's legs. We lowered the seat, backrest and rack by 20 cm, pulled the handlebars back, added footrests and a seat post, and placed five invisible pins (seat, two grips, two footrests) for the rider code to read. A check of every pair of touching parts across 16 models found 701 overlaps. Almost all were meant to be there. Seven were mistakes, like a lemon sunk into its pot and a lighthouse window buried in its wall, and each was fixed by sliding the part back.

### Treat a scene kit as a box of tiles, and draw your own map

The Scene Detailed style returns ground tiles that snap together on a grid, painted from one shared texture so the edges match, with rules for which tiles may touch and a sample map. You give it three sizes: the width of a tile, the height of a person and the height of one level. List what's missing before you build on the tiles, because the kit only contains what you asked for. If you need more tiles, ask for an extension of the same kit.

Our first sample map was 62 squares of road and one cottage, with no sea, so we kept the tiles and wrote a small program to draw our own map. That one map file feeds the game, the Blender scene and the collision walls. We asked for stairs between levels and got stairs with no ramp, so the trike can't reach the raised terrace. An extension added six tiles and left the first ten the same shape, so nothing already built had to move.

From high up, the sea showed dark blotches. The water tile is a sheet with a top and an underside in the same place, and the underside showed through. Turning off shadows, lowering the water and disabling mipmaps didn't help; dropping the downward-facing triangles did.

### Expect a jointed mannequin, and plan to animate it yourself

RD3D characters are rigid pieces that turn at joints, like a wooden artist's mannequin, with no skin that bends. A clip with the right name doesn't prove the rig can perform it, so check the joints you need before you rely on them. Because the parts are rigid, you can write a walk as a script that places the feet and turns the joints, then check it with measurements.

The courier came with no knees. A rerig added knee joints, but each trouser leg was still one solid piece, so we cut the legs at the knee in Blender and covered the joint, which took him from 712 triangles to 888. The generated walk sank his shoes 6.5 cm into the floor, so the walk and run were rewritten as a Blender script with the heel landing first.

### Recolour by swapping each part's shades

Each part of an RD3D model is painted with two to four shades in its own patch of a small texture. To recolour a part, rebuild those few shades around a new colour and keep the steps between them, and the painted shading survives. Work per part, because separate parts can share a colour.

The courier's whole texture is 128 by 128 pixels with 27 colours, and his vest is three teals. Rebuilding shades like that gave five differently coloured riders and four townhouse schemes with no new generation. A multiply tint couldn't do it, since multiplying only darkens and a teal vest can't become yellow. His cap and shirt use the same cream, so swapping that colour across the whole image would have changed both.

## Directing the coding agent

### Ask to see the game before you choose it

Ask for mockups of what the player sees during play, then pick a direction from them. Box art and concept art tell you less about the game you'd be building. Label mockups as mockups, so nobody later mistakes them for screenshots.

The first request asked for four game ideas, each shown as an in-game view:

> we want these to not look like box art or concept art, but rather, reflect what in-game visuals would look like.

The seaside delivery idea won. Its little streets, warm colours and delivery vehicle set the direction for everything after.

### Give the agent a plain place to try one thing

A finished scene hides problems behind its scenery. Ask for a small, empty test scene for one interaction, such as getting on a vehicle or docking a boat, and have it open straight into control with no countdown or camera intro. Try new systems there before they go into the real world.

The movement gym was a flat pad with a few test stations, where the courier could walk, jump on the trike and drive through obstacles. Recolouring got its own test scene too, before it went anywhere near the playable world.

### Name what looks wrong, and the feeling you want

Point at the body part, moment or object that looks off, and add a few words about the feel you're after. Specific words give the agent something it can measure and change.

> his walk feels like his legs can't bend at his knees
> his elbows too, are not bending much
> his up/down motion is very jittery
>
> i think we should focus on exaggerated, whimsical, clearly visible movements

The walk and run were rebuilt with clear knee and elbow bends and a smoother bounce. A later note in the same style, "his hands should attach to the steering wheel and turn left and right too", led to hands that follow the grips as the bars turn.

### Write the action in the order it happens

For any movement, describe what happens first, what happens next and how nearby objects react. Once one move feels right, "the same fix" is enough to carry it to the next.

> He should stand up, with the boat rocking a bit, then jump out with the boat rocking more.

That became a staged exit: stand, pause, crouch, jump, land. The boat rocks gently as he rises and harder as he pushes off. A one-line follow-up, "his jump out of the car also needs the same fix", gave the trike the same sequence.

### Ask for dials when you can't say what better looks like

When something looks wrong and you don't know the fix, ask for sliders and switches so you can compare options yourself, including one switch that turns everything off. Asking for ways to improve something also tends to bring back a list of ideas to choose from.

The town looked flat, so the request was to see what lighting, fog, bloom and other effects could do. The agent added controls for time of day, fog, haze, bloom, shadows and outlines, plus a flat comparison and a reset. The off switch made each effect easy to judge, and some first tries were plainly wrong: haze washed out the view from above, fog covered the whole island and the clouds stayed bright at night.

## Checking and shipping

### Play it, and report what you couldn't do

Play the game yourself and describe what you tried and couldn't do, in plain words, without guessing at the cause. Tests only cover the cases someone wrote down. A plain report lets the agent go looking, and the cause may be somewhere you wouldn't expect.

> i can't seem to find a path from the my island to the next.

That report turned up four islands with no boat at all, two of them islands where new players could start. Shared boats were only created where a decorative boat happened to sit near a pier, and the world generator left decorative boats out at random. Now every pier has a boat. A later report that the piers were hard to see, a level below the street, led to signs above each pier and a pointer at the screen edge.

### Measure what the eye misses, and look at what the tests miss

Automated checks and visual review catch different problems, so use both. Measure what a picture can't show precisely, such as whether a loop ends where it started. Look at real screenshots at small and short screen sizes, because a passing functional test can't tell you whether the interface covers the action.

Reloading the courier's exported file and comparing each loop's first and last frames found a run whose final moment had been dropped on export, which the review videos had missed. Going the other way, the hop onto the trike passed its tests, but on a short landscape screen his head went behind the toolbar, so the camera now rises during the jump. Screenshots can mislead as well: capture delay made a menu slide-in look finished, and only a frame-by-frame trace showed the real timing. The agent can't hear, so the sound was checked with loudness readings, spectrograms and a frequency analysis, which found an 8,369 Hz whine in the trike motor that a filter then removed. Judging whether it sounds good still needs a person listening.

### Ship from a clean copy of a committed revision

When several agent sessions share one working folder, the dev server and the files on disk can hold someone else's unfinished changes. Build releases and record footage from a clean export of a committed revision, and check that a fresh checkout can rebuild what it needs. Keep test scenes and debug tools out of the public build with a separate entry point.

Partway through one test run, the shared dev server started serving another session's new release entry, so the checks moved to a private copy on its own port. A clean checkout once failed its tests because a generated file wasn't in the repository, and the build step that makes it had to run first. Releases now build from the main branch in a separate Git worktree, so uncommitted work can't slip into the package.
