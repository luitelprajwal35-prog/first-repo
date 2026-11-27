# Class 1 — Making the Page Feel Alive

**Topics:** Pseudo-classes · Transitions · Transform & Box-shadow · Position (relative, absolute, sticky, fixed) · z-index

**Files for this class**

| File | Purpose |
|---|---|
| `class1-practice.html` | The practice page. Students never edit this. |
| `class1-starter.css` | Students fill in the 16 TODOs here. |
| `class1-complete.css` | Your answer key, with the reasoning behind each line. |

**Suggested length:** about 90 minutes. Each concept follows the same rhythm: **teach → live-code → students do the matching TODOs**. Shorten the practice blocks if you run out of time.

---

## 0. Class plan

| Time | Block | What happens |
|---|---|---|
| 0–5 min | Hook | Open the practice page with **no** CSS effects. Ask: *"What feels dead about this page?"* |
| 5–20 min | Part 1: Pseudo-classes | Teach, then students do TODOs 3 and 8–11 |
| 20–40 min | Part 2: Transitions, transform, shadow | Teach, then students do TODOs 2, 4–5 and 12–15 |
| 40–70 min | Part 3: Position | Teach, then students do TODOs 6–7 and 16 |
| 70–85 min | Free play and bonus | Students experiment. Fast finishers do the bonus TODO. |
| 85–90 min | Recap and homework | Quick quiz (Section 6), then set homework |

> **Tip:** the TODO numbers in the starter file follow the page from top to bottom, not the order you teach the concepts. The table above tells you which TODOs go with which concept.

### How to run the practice page
1. Put all three files in one folder.
2. Open `class1-practice.html` in the browser (right-click → *Open with Live Server* if you use VS Code).
3. The HTML links to `class1-starter.css`. To show the finished version, change that one line to `class1-complete.css`.

### The hook (do this first)
Open the page and hover over everything. Nothing happens. Scroll down. The navbar disappears. Click the input box. Nothing tells you it is active. Then say:

> "A page that just *sits there* feels broken. Today we teach the page to **react** to you."

---

## 1. Part 1 — Pseudo-classes

### 1.1 What is a pseudo-class?

A **pseudo-class** lets you style an element **when it is in a certain state or position**, without adding any extra class to your HTML.

**Analogy:** a button is always the same button. But it has *moods*: "someone's mouse is over me", "someone is pressing me", "I'm the first item in a list". A pseudo-class is how CSS talks about those moods.

**Syntax:** a single colon after the selector, with no spaces.

```css
selector:pseudo-class {
  property: value;
}
```

### 1.2 State pseudo-classes (user interaction)

| Pseudo-class | Applies when… | Typical use |
|---|---|---|
| `:hover` | the mouse pointer is over the element | link colours, card lift, button colour |
| `:active` | the element is being pressed (mouse down) | the "pressed in" feel of a button |
| `:focus` | the element is selected via keyboard or click (inputs especially) | highlight the text box you're typing in |
| `:focus-visible` | focused **by keyboard** (Tab key) | a clear ring for keyboard users, without showing it on mouse clicks |

**Live-code demo (5 minutes):**

```css
a:hover {
  color: #e44d26;
}
```
Save, hover a link, and it changes. Then remove `:hover` and show that the colour changes *always*. The pseudo-class is what makes it conditional.

**Show the Tab key.** Click the address bar, then press **Tab** repeatedly. Watch the focus move from link to link to button. Say: *"Some people never use a mouse. Focus styles are how they know where they are."*

### 1.3 Structural pseudo-classes (position in a family)

These select an element by **where it sits among its siblings**.

| Pseudo-class | Selects… |
|---|---|
| `:first-child` | the first child of its parent |
| `:last-child` | the last child of its parent |
| `:nth-child(2)` | exactly the 2nd child |
| `:nth-child(even)` / `(odd)` | every 2nd child (zebra striping) |
| `:nth-child(3n)` | every 3rd child (3, 6, 9…) |

```css
.task-list li:first-child { font-weight: bold; }
.task-list li:nth-child(even) { background: #eef2ff; }
```

**Why it matters:** you can style a list of 100 items with one rule and no extra classes. Ask: *"How would you stripe a list of 100 items without pseudo-classes?"* (Answer: 100 classes. Nobody wants that.)

### 1.3b Bonus: pseudo-classes vs pseudo-elements
You'll see `::before` and `::after` (double colon) later. The rule of thumb:
- **One colon** (`:hover`) = a *state* of an existing element.
- **Two colons** (`::before`) = a *new fake element* that CSS creates.

Just mention this now. Don't teach `::before` today.

### 1.4 Common mistakes

| Mistake | What goes wrong | Fix |
|---|---|---|
| `.card :hover` (space before the colon) | Means "any hovered element *inside* .card", not the card itself | `.card:hover` with no space |
| Writing the hover rule **before** the base rule | Equal specificity means the later rule wins, so the base can cancel your hover | Put `:hover` rules **after** the normal rule |
| `outline: none` on inputs with no replacement | Keyboard users can no longer see where they are | Always add your own visible style, e.g. `border-color` + `box-shadow` |
| Relying on hover for important things | Phones have no hover | Hover is a *bonus* effect. Never hide essential content behind it. |

### 1.5 DevTools superpower (show this, students love it)
Right-click an element → **Inspect** → in the Styles panel click **`:hov`** → tick **:hover** or **:focus**. You can now edit hover styles without holding your mouse still.

### 1.6 Practice: TODOs for this part
- **TODO 3:** `.nav-links a:hover`
- **TODO 8–10:** `:first-child`, `:last-child`, `:nth-child(even)` on the task list
- **TODO 11:** `.goal-input:focus`

---

## 2. Part 2 — Transitions, transform and box-shadow

### 2.1 The problem transitions solve
Without a transition, a hover change is **instant**, like flipping a light switch. With a transition, the browser **animates between** the two states over time, like turning a dimmer.

### 2.2 Anatomy of `transition`

```css
transition: property duration timing-function delay;
```

| Part | Example | Meaning |
|---|---|---|
| property | `transform` | *what* to animate (`all` = everything) |
| duration | `0.3s` | how long it takes (`300ms` also works) |
| timing-function | `ease` | the speed curve |
| delay | `0.1s` | wait before starting (optional) |

**Timing functions to know:**
- `ease` (default): starts fast, ends slow
- `linear`: constant speed
- `ease-in`: slow start
- `ease-out`: slow end (feels natural for things appearing)
- `ease-in-out`: slow at both ends

**Multiple properties:** separate with commas.
```css
transition: transform 0.3s ease, box-shadow 0.3s ease;
```

### 2.3 The #1 rule students get wrong
> **Put `transition` on the normal (base) rule, not on `:hover`.**

```css
/* ✅ Correct: animates in AND out */
.card {
  transition: transform 0.3s ease;
}
.card:hover {
  transform: translateY(-6px);
}

/* ❌ Wrong: animates in, but snaps back instantly when the mouse leaves */
.card:hover {
  transition: transform 0.3s ease;
  transform: translateY(-6px);
}
```
**Demo it live** by writing it the wrong way first. Students will see the snap-back and remember it.

### 2.4 What can be animated?
Colours, sizes, numbers, `transform`, `opacity`, `box-shadow`: **yes**.
`display` (none ↔ block) and `height: auto`: **no**. Explain simply: *"The browser needs a start number and an end number to animate between."*

**Caution about `transition: all`:** it works, but it animates *everything*, including things you didn't intend. Naming the properties is cleaner. Use `all` for quick experiments only.

### 2.5 `transform`
Moves, resizes or rotates an element **visually, without disturbing the layout**. Neighbouring elements do not move.

| Function | Effect |
|---|---|
| `translateY(-6px)` | move up 6px (negative = up) |
| `translateX(10px)` | move right 10px |
| `scale(1.05)` | grow to 105% |
| `scale(0.96)` | shrink to 96% (great for a "pressed" button) |
| `rotate(5deg)` | tilt 5 degrees |

Combine by separating with a space: `transform: translateY(-4px) scale(1.02);`

**Why not just `margin-top: -6px`?** Changing margin *pushes the other cards around* and the whole layout jumps. `transform` only moves the element's paint. Layout stays still, and the browser can animate it smoothly.

### 2.6 `box-shadow`

```css
box-shadow: offset-x offset-y blur spread color;
```

```css
box-shadow: 0 12px 24px rgba(15, 23, 41, 0.15);
/*          x  y     blur   colour (last number = transparency) */
```

- **x / y offset:** where the shadow falls. `0 12px` means straight down.
- **blur:** how soft. Larger means softer.
- **spread (optional):** grows or shrinks the shadow.
- **color:** use `rgba()` so the shadow is see-through. Solid black shadows look harsh.

**The "lift" trick:** raising an element (`translateY`) **plus** a bigger shadow below it makes the brain read it as *floating up*. Neither alone is as convincing.

**Focus ring trick** (used in TODO 11): a shadow with **0 blur and a spread** makes a ring.
```css
box-shadow: 0 0 0 4px rgba(41, 101, 241, 0.2);
```

### 2.7 Small helpers to mention
- `cursor: pointer;` shows the hand icon on anything clickable. Buttons don't always get it by default.
- `opacity: 0.8;` transparency from 0 to 1. Also animates well.

### 2.8 Good taste: don't overdo it
- Keep durations between **0.2s and 0.4s**. Longer feels sluggish.
- Animate the things that **respond to the user** (hover, focus, press). Don't make every element move all the time.
- A little goes a long way. If students want everything spinning, gently steer them.

### 2.9 Practice: TODOs for this part
- **TODO 2:** transition on the nav links
- **TODO 4–5:** card transition and card hover lift
- **TODO 11:** the input's focus ring (uses the shadow trick)
- **TODO 12–15:** button transition, hover, active and focus-visible

---

## 3. Part 3 — Position

### 3.1 Where does normal layout come from?
By default every element is `position: static`. It sits in the **normal flow**, one after another, and `top / left / right / bottom` do **nothing**. The `position` property is how you *break out* of normal flow on purpose.

### 3.2 The five values

| Value | In normal flow? | Positioned relative to… | Main use |
|---|---|---|---|
| `static` | yes | — (default) | everything, by default |
| `relative` | **yes** (keeps its space) | its own original spot | small nudges, and being an **anchor** for absolute children |
| `absolute` | **no** (removed) | nearest **positioned ancestor** | badges, icons in corners, tooltips |
| `sticky` | yes | scroll position | headers that stick as you scroll |
| `fixed` | **no** (removed) | the **browser window** | back-to-top buttons, chat bubbles |

**Analogies for the classroom:**
- `relative` = a sticky note lying on your desk. You can slide it a bit, but its original spot stays reserved.
- `absolute` = a pin stuck on a corkboard. You choose the exact corner of the board (the parent).
- `sticky` = a bookmark that scrolls along with the page until it reaches the top, then stays there.
- `fixed` = a sticker on your **screen glass**. It doesn't move however you scroll.

### 3.3 `top / right / bottom / left`
These are the **offsets**. They only work when `position` is **not** `static`.
```css
position: absolute;
top: -10px;   /* 10px ABOVE the top edge of the anchor */
right: -10px; /* 10px OUTSIDE the right edge */
```
Negative values push the element *outside* the edge. That is how the "New" badge hangs over the corner of the card.

### 3.4 `relative` + `absolute` together (the most important pair)
> **An absolute element positions itself relative to the nearest ancestor that has `position` set to anything other than `static`.**
> If no ancestor is positioned, it flies off to the corner of the page.

```css
.subject-card { position: relative; }   /* the anchor  */
.badge { position: absolute; top: -10px; right: -10px; }
```

**Run this live experiment:**
1. Do the badge with `position: absolute`, but **without** `position: relative` on the card. The badge flies to the page corner.
2. Add `position: relative` to the card. The badge snaps into the card corner.

Ask: *"Why did that fix it?"* Let them say the rule in their own words. This is the concept students most often get wrong.

**Also point out:** `.subject-card` uses `position: relative` **without moving anything**. It's there purely to give the badge something to attach to.

### 3.5 `sticky`
```css
.navbar {
  position: sticky;
  top: 0;        /* required: "stick when you're 0px from the top" */
  z-index: 100;
}
```
- Behaves like normal until you scroll to the `top` value, then it sticks.
- **It needs a `top` value.** Without one it does nothing.
- It only sticks **within its parent**. If the parent is short, the sticky element leaves with it.
- If any ancestor has `overflow: hidden/auto/scroll`, sticky often silently breaks. It's the classic "why doesn't my sticky work?" bug.

### 3.6 `fixed`
```css
.back-to-top {
  position: fixed;
  bottom: 20px;
  right: 20px;
}
```
Fixed elements are attached to the **viewport**. Scroll as much as you like and they stay put. Because they leave the flow, other content behaves as if they aren't there.

**Sticky vs fixed in one line:** *sticky* is part of the page layout until it needs to stick. *fixed* is always glued to the window.

### 3.7 `z-index` (who is on top?)
When positioned elements overlap, `z-index` decides the stacking order. A bigger number sits on top.

- `z-index` only works on **positioned** elements (anything but `static`), plus flex or grid children.
- Live demo: remove `z-index: 100` from `.navbar`, scroll, and watch the cards and badge slide **over** the navbar. Put it back and it's fixed.
- Don't use silly numbers like `999999`. Use small, sensible numbers: `1` for cards, `100` for headers.

### 3.8 Common mistakes

| Symptom | Likely cause | Fix |
|---|---|---|
| Badge appears in the page corner, not the card | Parent has no `position: relative` | Add `position: relative` to the parent |
| `top: 10px` does nothing | Element is still `static` | Set `position` first |
| Sticky navbar doesn't stick | Missing `top: 0`, or an ancestor has `overflow` | Add `top: 0`, check ancestors |
| Cards slide over the sticky navbar | No `z-index` on the navbar | `z-index: 100` |
| Layout collapses after `position: absolute` | Absolute removes the element from the flow | Expected. Give the parent a size or use another approach |

### 3.9 Practice: TODOs for this part
- **TODO 1:** sticky navbar
- **TODO 6:** `position: relative` on the card (the anchor)
- **TODO 7:** `position: absolute` on the badge
- **TODO 16:** fixed back-to-top button

---

## 4. Walkthrough: what each TODO teaches (answer-key summary)

| # | Where | What to write | Concept |
|---|---|---|---|
| 1 | `.navbar` | `position: sticky; top: 0; z-index: 100;` | sticky + z-index |
| 2 | `.nav-links a` | `transition: color 0.3s ease, border-color 0.3s ease;` | transition on the base rule |
| 3 | `.nav-links a:hover` | colour and underline change | `:hover` |
| 4 | `.subject-card` | `transition: transform 0.3s ease, box-shadow 0.3s ease;` | transition on the base rule |
| 5 | `.subject-card:hover` | `transform: translateY(-6px)` + bigger shadow | transform + box-shadow |
| 6 | `.subject-card` | `position: relative;` | anchor for the badge |
| 7 | `.badge` | `position: absolute; top: -10px; right: -10px;` | absolute |
| 8 | `.task-list li:first-child` | left border + bold | `:first-child` |
| 9 | `.task-list li:last-child` | `border-bottom: none;` | `:last-child` |
| 10 | `.task-list li:nth-child(even)` | light background | `:nth-child` |
| 11 | `.goal-input:focus` | border colour + ring shadow | `:focus` |
| 12 | `.btn` | `cursor: pointer;` + transition | cursor + transition |
| 13 | `.btn:hover` | darker background | `:hover` |
| 14 | `.btn:active` | `transform: scale(0.96);` | `:active` |
| 15 | `.btn:focus-visible` | visible outline | `:focus-visible` (accessibility) |
| 16 | `.back-to-top` | `position: fixed; bottom: 20px; right: 20px;` | fixed |
| Bonus | `.social-links a` | transition + hover colour | everything together |

The full solutions with commentary are in `class1-complete.css`.

---

## 5. Quick review questions for the last 5 minutes

Ask these out loud or put them on the board.

1. What is the difference between `.card:hover` and `.card :hover`?
   **Answer:** the first styles the card when it's hovered. The second styles any hovered element *inside* the card.
2. Where should `transition` go, on the normal rule or on `:hover`? Why?
   **Answer:** on the normal rule, so it animates in *and* out.
3. Why use `transform: translateY()` instead of `margin-top` for a hover lift?
   **Answer:** transform doesn't disturb the layout, so neighbours don't jump.
4. An absolute badge is showing in the page corner, not the card. What is missing?
   **Answer:** `position: relative` on the card (the parent).
5. Your sticky navbar isn't sticking. Name two possible reasons.
   **Answer:** missing `top: 0`, or an ancestor with `overflow` set.
6. What's the difference between `sticky` and `fixed`?
   **Answer:** sticky sticks only after you scroll to it and stays inside its parent. Fixed is always glued to the viewport.
7. Why is `outline: none` dangerous, and what do you do instead?
   **Answer:** keyboard users lose sight of the focused element. Replace it with your own visible focus style.
8. Which one shows a "ring" without a border: `box-shadow: 0 0 0 4px …` or `box-shadow: 0 4px 0 …`?
   **Answer:** the first one. Zero offset and zero blur with a spread makes a ring.

---

## 6. Homework

**Basic (everyone):** take the personal profile page or the StudyHub dashboard and add
- a hover effect with a transition on every link and every card
- a `:focus` style on any input or button
- a sticky header

**Stretch:**
- Add a "Popular" badge to one card using relative + absolute.
- Use `:nth-child(odd)` or `:nth-child(3n)` to give some cards a different border colour.
- Add a `rotate(-2deg)` tilt on card hover, and adjust the timing until it feels right.

**Reflection question** (one line in their notebook): *"When would I use `sticky` and when `fixed`?"*

---

## 7. Cheat sheet (share with students)

```css
/* STATES */
a:hover { }            /* mouse over */
button:active { }      /* being pressed */
input:focus { }        /* selected / typing */
button:focus-visible { } /* keyboard focus */

/* STRUCTURE */
li:first-child { }
li:last-child { }
li:nth-child(even) { }
li:nth-child(3n) { }

/* TRANSITION (goes on the BASE rule) */
transition: property duration timing-function delay;
transition: transform 0.3s ease, box-shadow 0.3s ease;

/* TRANSFORM */
transform: translateY(-6px);
transform: scale(1.05);
transform: rotate(3deg);

/* SHADOW */
box-shadow: x y blur spread color;
box-shadow: 0 12px 24px rgba(0, 0, 0, 0.15);

/* POSITION */
position: relative;   /* anchor */
position: absolute;   /* free-floating inside anchor */
position: sticky;     /* + top: 0 */
position: fixed;      /* glued to the window */
top / right / bottom / left: ...;
z-index: 100;
```

---

## 8. Bridge to Class 2

End with a question: **"Everything we built today works great on a laptop. What happens when you open this on a phone?"**

Let them try it (or use DevTools device mode). Three problems appear:
1. The 3-column grid is squashed.
2. There is no hover on a touchscreen.
3. The navbar is crowded.

Then say: *"Next class we make the page adapt to every screen: media queries, flexible layouts and CSS variables."*
