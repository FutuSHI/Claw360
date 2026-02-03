---
name: card-image-generator
description: Generate cinematic card images for Claw360 in the "Technological Brutalism" and "Cyber-Industrial Aesthetic" style. Use this skill when the user asks to create card images, generate cover images for articles/cards, batch generate images for Claw360 content, or create visuals in the predominantly matte-white cyber-industrial style with minimal green accent lighting.
---

# Card Image Generator for Claw360

Generate cinematic macro-style images for Claw360 cards using the "Technological Brutalism" and "Cyber-Industrial Aesthetic" style.

## Model

Use **Gemini 3 Pro Image Preview** (`gemini-3-pro-image-preview`) for image generation. This model supports multiple reference image inputs, which is required for this skill.

## Reference Images (Must Be Provided to the Model)

When generating card images, always provide these 2 reference images alongside the text prompt:

1. **Style reference** — `ref-card-on-page.png` (located in this skill's directory)
   - Shows how a card image appears on the actual Claw360 website
   - Purpose: Tell the model "match this visual style — dark cinematic look with neon-green accents on a pitch-black background"

2. **Layout / safe-zone guide** — `ref-safe-zone-guide.png` (located in this skill's directory)
   - Shows the full generated image with a red rectangle marking the visible safe zone
   - Purpose: Tell the model "the subject must fit entirely inside this red box area — this is the only part that will be visible on the card"

## Prompt Template

Use this template to generate images. Replace `[SUBJECT]` with the card topic.

**Text prompt:**

```
Generate an image in 8:3 aspect ratio.

Subject: A cinematic shot of [SUBJECT] set against a pitch-black, minimalist background. The style is "Technological Brutalism" mixed with "Cyber-Industrial Aesthetic."

I am providing two reference images:
- Reference image 1 shows how the final image will look on a card in the website. Match this visual style exactly — dark cinematic look with neon-green emissive accents on a pitch-black background.
- Reference image 2 shows a red rectangle on the image. This red rectangle marks the safe zone. The entire subject must fit INSIDE this red rectangle area. Nothing important should extend outside the red box. The red box is roughly the center 60% width and 55% height of the image.

Composition: The subject must be compact and fully contained within the center safe zone of the frame. Keep it small-to-medium sized, horizontally and vertically centered, with generous pitch-black negative space on all sides. The subject should float in the center like a product shot. Do NOT let any part of the subject touch or extend near the edges of the image. Do NOT draw the red rectangle in the output image.

Lighting & Color: High-contrast low-key lighting. The subject is predominantly matte white / light gray. Green light should be VERY minimal — only tiny sparse accent details such as a few small glowing dots, a thin edge highlight, or faint circuit traces. Green should cover no more than 10-15% of the subject. Do NOT surround or engulf the subject in green glow. The overall impression should be a clean white object with just a hint of green. A very faint, subtle green reflection on the polished dark marble floor is acceptable.

Materials: Primarily matte white ceramic as the dominant material. Secondary material is high-gloss onyx black. Sharp edges and clean, engineered lines. The subject should read as a white object first and foremost.

Camera Settings: Shot on 50mm lens, f/4, moderate depth of field, sharp focus on the center object, cinematic wide composition with lots of breathing room, hyper-realistic 8k render, Unreal Engine 5 aesthetic.
```

**Image inputs (attach both):**
1. `ref-card-on-page.png` — style reference
2. `ref-safe-zone-guide.png` — layout / safe-zone guide

## Aspect Ratio

- **Claw360 card images**: Always use 8:3 aspect ratio (matching the card display area of 320x120px)
- This is the only ratio used for Claw360 cards. Do not use other ratios unless the user explicitly requests it.

## Subject Framing Rules (Critical)

The card image area uses `object-fit: cover` with a wide 8:3 aspect ratio (e.g. 320x120px). This means the visible area is a very wide, short horizontal strip. To ensure the subject looks good in the card:

1. **Keep the subject compact and centered** — it must fit entirely within the center 60% width x 55% height of the generated image (the red box area in the safe-zone guide)
2. **Use generous negative space** — the pitch-black background should dominate the image, with the subject floating as a small-to-medium element in the center
3. **Avoid tall subjects** — anything taller than it is wide will get cropped at top and bottom. Prefer wide, horizontal, or squat compositions
4. **No edge bleeding** — no part of the subject should extend to the edges of the image
5. **Think "product photography"** — imagine the subject sitting on a small area in the center of a very wide dark stage

## Subject Mapping Examples

Transform card topics into visual subjects:

| Card Topic | Image Subject |
|------------|---------------|
| AI/Machine Learning | a neural network cube with pulsing data nodes |
| Web Development | a crystalline browser window with floating code fragments |
| Database/Data | geometric data crystals arranged in a grid formation |
| Security/Encryption | an intricate mechanical lock with glowing circuits |
| Cloud Computing | floating server modules connected by light beams |
| API/Integration | interlocking mechanical gears with data streams |
| Mobile Development | a sleek smartphone frame with holographic UI elements |
| Analytics/Metrics | 3D bar charts rising from a reflective surface |
| Automation | robotic arms with precision joints and sensors |
| E-commerce | a stylized shopping cart with digital price tags |

## Batch Generation Workflow

1. **Collect card titles** from Claw360 content
2. **Map each title** to a visual subject using the table above or create custom subjects
3. **Generate prompts** using the template
4. **Attach both reference images** to every generation request
5. **Use Gemini 3 Pro Image Preview** to generate each image

## Example Output Prompt

For a card titled "molt.chess" (an agent chess league):

**Text prompt:**

```
Generate an image in 8:3 aspect ratio.

Subject: A cinematic shot of a chess king piece in matte white ceramic standing next to a knight piece with neon-green glowing wireframe outline, both pieces compact and centered, set against a pitch-black, minimalist background. The style is "Technological Brutalism" mixed with "Cyber-Industrial Aesthetic."

I am providing two reference images:
- Reference image 1 shows how the final image will look on a card in the website. Match this visual style exactly — dark cinematic look with neon-green emissive accents on a pitch-black background.
- Reference image 2 shows a red rectangle on the image. This red rectangle marks the safe zone. The entire subject must fit INSIDE this red rectangle area. Nothing important should extend outside the red box. The red box is roughly the center 60% width and 55% height of the image.

Composition: The subject must be compact and fully contained within the center safe zone of the frame. Keep it small-to-medium sized, horizontally and vertically centered, with generous pitch-black negative space on all sides. The subject should float in the center like a product shot. Do NOT let any part of the subject touch or extend near the edges of the image. Do NOT draw the red rectangle in the output image.

Lighting & Color: High-contrast low-key lighting. The subject is predominantly matte white / light gray. Green light should be VERY minimal — only tiny sparse accent details such as a few small glowing dots, a thin edge highlight, or faint circuit traces. Green should cover no more than 10-15% of the subject. Do NOT surround or engulf the subject in green glow. The overall impression should be a clean white object with just a hint of green. A very faint, subtle green reflection on the polished dark marble floor is acceptable.

Materials: Primarily matte white ceramic as the dominant material. Secondary material is high-gloss onyx black. Sharp edges and clean, engineered lines. The subject should read as a white object first and foremost.

Camera Settings: Shot on 50mm lens, f/4, moderate depth of field, sharp focus on the center object, cinematic wide composition with lots of breathing room, hyper-realistic 8k render, Unreal Engine 5 aesthetic.
```

**Image inputs (attach both):**
1. `ref-card-on-page.png` — style reference
2. `ref-safe-zone-guide.png` — layout / safe-zone guide

## Integration

Use `generate-image` skill or Gemini 3 Pro Image Preview API directly. Always attach both reference images with every generation request.
