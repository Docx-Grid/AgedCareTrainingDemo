# Aged Care Training – Template Editing Guide

This guide explains how to customise the look and structure of the interactive training module.

The story is built with **SugarCube 2** (Twine). All visual settings are controlled by CSS variables and a few simple HTML patterns, so you can change colours, fonts, images and layout without touching the training content itself.

---

## 1. Files you need

| File | Purpose |
|------|---------|
| `aged-care-training.twee` | Source file – edit this |
| `aged-care-training.html` | Playable version – recompile after changes |

**To recompile after editing:**

```bash
/tmp/tweego -o aged-care-training.html aged-care-training.twee
```

(Or use the Twine 2 editor: Import the `.twee` / HTML, edit, then “Publish to File”.)

---

## 2. Changing colours, fonts and layout (CSS Variables)

Open the passage **`StoryStylesheet`**.

Near the top you will see a block that looks like this:

```css
:root {
	/* ---- COLOURS (change these) ---- */
	--bg-color: #eef5f3;
	--card-bg: #ffffff;
	--card-border: #d0e8e2;
	--accent: #2a9d8f;
	--accent-dark: #1d7a6f;
	--text-main: #2c3e50;
	--text-heading: #1d6a63;
	--correct-bg: #e6f7ef;
	--correct-border: #28a745;
	--wrong-bg: #fdf0f0;
	--wrong-border: #dc3545;
	--tip-bg: #fff8e6;
	--tip-border: #e0a800;
	--choice-bg: #f0faf8;
	--choice-border: #b8e0d8;
	--choice-hover: #2a9d8f;
	--choice-text: #1d6a63;

	/* ---- TYPOGRAPHY ---- */
	--font-main: "Segoe UI", system-ui, -apple-system, sans-serif;
	--font-size-base: 1.05rem;
	--line-height: 1.55;

	/* ---- LAYOUT ---- */
	--card-max-width: 720px;
	--card-radius: 16px;
	--choice-radius: 12px;
}
```

### What each variable does

| Variable | Controls |
|----------|----------|
| `--bg-color` | Page background colour (when no background image is set) |
| `--card-bg` | White (or coloured) centre container |
| `--card-border` | Soft border around the centre card |
| `--accent` / `--accent-dark` | Primary buttons, score pill, hover states |
| `--text-main` | Ordinary body text colour |
| `--text-heading` | All headings (h1, h2, h3) |
| `--correct-bg` / `--correct-border` | Green “Right answer” feedback box |
| `--wrong-bg` / `--wrong-border` | Red “Wrong answer” feedback box |
| `--tip-bg` / `--tip-border` | Yellow tip / note box |
| `--choice-bg` / `--choice-border` / `--choice-text` | Multiple-choice button appearance |
| `--choice-hover` | Colour when the learner hovers / taps a choice |
| `--font-main` | Font family for the whole story |
| `--font-size-base` | Base text size |
| `--card-max-width` | Maximum width of the centre content card (responsive) |
| `--card-radius` / `--choice-radius` | Rounded corners |

**Tip:** Change only the hex values. Keep the rest of the CSS intact.

---

## 3. Adding a full-page background image

In the passage **`StoryInit`** find:

```
<<set $bgImage to "">>
```

Change it to a full URL, for example:

```
<<set $bgImage to "https://your-server.com/images/soft-green-bg.jpg">>
```

Or use a local image (place the image next to the HTML file and use a relative path):

```
<<set $bgImage to "background.jpg">>
```

Leave it as `""` for a solid colour background (`--bg-color`).

---

## 4. Adding a featured image at the top of a passage

Inside any passage, place this **above** the `<div class="card-content">`:

```html
<img class="featured-image" src="https://example.com/photo.jpg" alt="Description of image">
```

Example (Dementia question):

```
:: Dementia_Q1
...
<img class="featured-image" src="images/mrs-chen.jpg" alt="Care worker speaking with older woman">

<div class="card-content">
	...
</div>
```

The image will stretch full-width across the top of the white card and be automatically cropped to a sensible height on mobile.

---

## 5. Passage templates (copy-and-paste)

### A. Standard question passage

```
:: Your_Question_Name

<div class="card-content">
	<div class="scenario-label">Scenario X · Short title</div>
	<h1>Question heading</h1>

	<p>Scenario text goes here…</p>

	<p><strong>What do you do?</strong></p>

	<div class="choices">
		[[Wrong option text|Your_Wrong1]]
		[[Correct option text|Your_Right1]]
		[[Another option|Your_Tip1]]
	</div>
</div>
```

### B. Wrong-answer feedback passage

```
:: Your_Wrong1

<div class="card-content">
	<div class="feedback wrong">
		<strong>Not recommended</strong>
		Explanation of why this choice is not best practice.
	</div>

	<div class="choices">
		[[← Go back and choose again|Your_Question_Name]]
	</div>
</div>
```

### C. Right-answer feedback passage

```
:: Your_Right1
<<set $score to $score + 1>>

<div class="card-content">
	<div class="feedback correct">
		<strong>Good choice</strong>
		Positive explanation.
	</div>

	<p>Next part of the scenario…</p>

	<div class="choices">
		[[Next question or continue|Next_Passage]]
	</div>
</div>
```

### D. Tip / neutral feedback

```
:: Your_Tip1

<div class="card-content">
	<div class="feedback tip">
		<strong>Possible, but weaker</strong>
		Explanation.
	</div>

	<div class="choices">
		[[See the preferred response|Your_Right1]]
	</div>
</div>
```

### E. Welcome / End style (centred)

```
:: Welcome

<div class="card-content">
	<div class="center">
		<div class="score-pill">Staff Training Demo</div>
		<h1>Welcome</h1>
		<p>Introduction text…</p>
	</div>

	<div class="center">
		[[Start Training →|Menu]]
	</div>
</div>
```

---

## 6. How the layout works on different devices

- **Desktop / tablet landscape** – Centre card is limited to 720 px wide and sits in the middle of the screen.
- **Tablet portrait / large phones** – Card expands almost full width with comfortable padding.
- **Small phones** – Font size, padding and button size automatically reduce via the media queries at the bottom of the stylesheet. Buttons remain large enough for easy tapping.

The sidebar (UI bar) is completely removed so the experience is full-width and distraction-free on every device.

---

## 7. Common customisations

### Change the main brand colour

1. Set `--accent` and `--accent-dark` to your organisation’s colour.
2. Also update `--choice-hover` and `--text-heading` if you want them to match.

### Make the centre card wider or narrower

Change `--card-max-width` (e.g. `640px` or `800px`).

### Use a different font

```css
--font-main: "Open Sans", "Helvetica Neue", Arial, sans-serif;
```

(Make sure the font is available on the devices your staff use, or load it via a `<link>` in a custom head passage.)

### Make choice buttons more colourful

Increase the contrast of `--choice-bg` / `--choice-border` or give them a stronger background.

---

## 8. Score system (optional)

The demo tracks a simple score:

- `$score` – points earned
- `$maxScore` – total possible points
- `$completed` – array of finished scenario IDs

You can ignore the score system completely if you prefer pure branching without points. Just delete the `<<set $score …>>` and `<<set $maxScore …>>` lines.

---

## 9. Quick checklist for a new scenario

1. Create a question passage using the template in section 5A.
2. Create one Wrong / Right / Tip passage for each choice.
3. Add the scenario to the **Menu** passage.
4. (Optional) Add a featured image.
5. Recompile and test on both a computer and a phone.

---

**That’s everything you need to keep the visual design consistent while adding or changing content.**
