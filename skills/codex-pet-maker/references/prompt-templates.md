# Prompt Templates

Use these as starting points. Keep the user's requested identity and style above the template when they provide references.

## Pig Hero Mascot

```text
Create a Codex-compatible 8-column by 9-row chibi pig hero spritesheet.
Character: glossy red-suited pig hero mascot, red helmet/hood, yellow eye patches on helmet, yellow wrist bands, yellow pig emblem on belly, pink snout and cheeks, small ears, big friendly eyes.
Canvas: exactly 1536x1872 if possible, each frame fits a 192x208 cell.
Rows:
1 idle standing, blink, breathing;
2 side movement facing one direction, clean loop;
3 side movement facing the opposite direction, clean loop;
4 greeting/waving;
5 happy celebration/jump;
6 error/confused, first two frames holding ERROR sign;
7 progress bars labeled exactly 0%, 20%, 40%, 60%, 80%, 100%, 100%, 100%;
8 front-facing walk/patrol;
9 sitting with small dark laptop, typing/blinking.
Critical quality: one pig per cell, no ghosting, no afterimages, no duplicate silhouettes, no motion trails, no shadows, no cropped limbs, no cell bleeding.
Background: transparent if possible, otherwise flat removable light checker-free background.
```

## Pet Photo Cat

```text
Create a Codex-compatible 8-column by 9-row chibi cat spritesheet from the reference cat photos.
Preserve key traits: coat colors and markings, eye color, face shape, tail type, body softness, and the cat's general expression.
Canvas: exactly 1536x1872 if possible, each frame fits a 192x208 cell.
Rows:
1 seated idle, slow blink, ear twitch, tail curl;
2 side-walk movement to one side, paws alternating;
3 side-walk or trot movement to the opposite side, paws alternating;
4 paw raise / attention / tiny meow;
5 cat-specific play, roll, loaf, stretch, or pounce;
6 error/confused, first two frames holding ERROR sign;
7 progress bars labeled exactly 0%, 20%, 40%, 60%, 80%, 100%, 100%, 100%;
8 front-facing walk toward viewer;
9 keyboard/laptop cat, paw tapping or sleepy blink.
Critical quality: one cat per cell, no ghosting, no afterimages, no duplicate silhouettes, no cropped ears/tail/paws, no cell bleeding.
Background: transparent if possible, otherwise flat removable light checker-free background.
```

## Pet Photo Dog

```text
Create a Codex-compatible 8-column by 9-row soft chibi dog spritesheet from the reference dog photo.
Style: match the existing cat pet spritesheets as the visual anchor: delicate fluffy fur edges, soft watercolor-like semi-3D sticker volume, gentle shading, warm highlights, big expressive eyes, cute rounded proportions. Avoid program-drawn vector art, pixel art, hard icon outlines, flat emoji style, jagged edges, and geometric blocky bodies.
Preserve key breed traits: coat colors and markings, ear shape, tail type, face mask or muzzle color, body proportions, and the dog's general expression.
Canvas: exactly 1536x1872 if possible, each frame fits a 192x208 cell.
Rows:
1 seated idle, blink, breathing;
2 side-run or side-walk movement facing left, paws alternating;
3 side-run or side-walk movement facing right, paws alternating;
4 paw raise / greeting;
5 happy jump or playful bounce;
6 error/confused, first two frames holding ERROR sign;
7 progress bars labeled exactly 0%, 20%, 40%, 60%, 80%, 100%, 100%, 100%;
8 front-facing walk toward viewer;
9 sitting with small dark laptop, typing/blinking.
Critical quality: one dog per cell, no ghosting, no afterimages, no duplicate silhouettes, no motion trails, no cropped ears/tail/paws, no cell bleeding.
Background: transparent if possible, otherwise flat removable light checker-free background.
```

## Repair Prompt

When regenerating only a bad row:

```text
Create exactly 8 separate replacement frames in one row, matching the existing pet style and scale.
Each frame must be a single isolated subject centered in its own cell with generous padding.
No motion trails, no ghosting, no afterimages, no duplicate silhouettes, no cropped body parts, no background.
```
