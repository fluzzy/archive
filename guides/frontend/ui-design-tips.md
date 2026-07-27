# 16 Little UI Design Tips That Make a Big Impact

> **Source**: [Adham Dannaway](https://www.adhamdannaway.com/blog/ui-design/ui-design-tips)
> **Author**: Adham Dannaway
> **Fetched**: 2026-07-27
> **Archived**: 2026-07-27

Sixteen logic-driven UI design guidelines covering layout, colour, accessibility, and typography, presented as a step-by-step case study.

The core argument: after two decades as a product designer, the author found that most visual and interaction design decisions are governed by a system of logical guidelines — not artistic flair or magical intuition. Artistic talent helps, but much of what makes an interface intuitive, accessible, and beautiful can be learned. Without a logical system, you're just using gut feeling to move things around until they look pretty.

**Case study**: the property details page of a short-term property rental app. The original design feels messy, complicated, and hard to use because of many small problematic details. Each tip fixes one of them. Tips are drawn from the author's *Practical UI* design book; the example interface was built with his Figma design system.

---

## Layout & Structure

### 1. Use space to group related elements

Breaking information into smaller groups of related elements structures and organises an interface, making it faster to understand and remember.

Four ways to group elements:

- Place related elements in the **same container**
- **Space** related elements close together
- Make related elements look **similar**
- **Align** related elements in a continuous line

Containers are the strongest grouping cue but add clutter. Prefer the more subtle methods where possible, and combine them to make groupings clearer. Space in particular is simple and very effective.

**In the example**: content is cramped together, making the design look cluttered. Increasing spacing groups content clearly.

### 2. Be consistent

Consistency means similar elements look and work in a similar way — both within your product and compared to other well-established products. This predictable functionality improves usability and reduces errors, because people don't have to keep learning how things work.

**In the example**: icon styles are inconsistent — some filled, some not. This confuses users because **filled icons often indicate a selected state**. All icons are outlined with a **2pt stroke weight and rounded corners** for consistent visual weight. Using a quality free icon set generally avoids this problem entirely.

Text labels are also added to the icons so their meaning is clear, especially for screen reader users (software that describes an interface via speech or braille to someone who can't see it).

### 3. Ensure similar looking elements function similarly

If elements look similar, people expect them to work similarly. Use a consistent visual treatment for elements with the same functionality — and make elements with *different* functionality look different.

**In the example**: the icon containers are styled like the "Book now" button, making them seem interactive when they aren't. Removing the blue colour and button styling from the icons prevents them being mistaken for interactive elements.

### 4. Create a clear visual hierarchy

Not all information has the same importance. Present information in order of importance by making more important elements more prominent. A clear hierarchy helps people scan quickly and focus on areas of interest, and improves aesthetics by creating a sense of order.

Create hierarchy with variations in **size, colour, contrast, spacing, position, and depth**.

**The Squint Test** — squint your eyes at the design (or move further from the screen, or blur it). You should still be able to tell what the most important elements are and what the interface is for.

**In the example**: multiple elements compete for attention with similar prominence, while the primary action in the bottom left doesn't stand out at all. The primary action should generally be the most prominent element — giving it a high-contrast background colour and bold font weight achieves this (and fixes the button's contrast accessibility issue, see #7).

### 5. Remove unnecessary styles

Unnecessary information and visual styles are distracting and increase **cognitive load** (the brain power required to use an interface). Avoid unnecessary lines, colours, backgrounds, and animations.

**In the example**: white space and borders around the image add visual complexity without conveying information or grouping anything, so they're removed.

---

## Colour & Accessibility

### 6. Use colour purposefully

Use colour sparingly and with purpose — avoid colour purely for decoration. **Start with black and white and introduce colour where it conveys meaning.**

A simple, effective approach: apply the brand colour to interactive elements like text links and buttons. This teaches people what's interactive. Avoid the brand colour on non-interactive elements.

Not *all* interactive elements need colour — some already have visual cues signalling interactivity (e.g. a card still feels clickable with or without a blue link inside it).

**In the example**: the blue heading looks nice but makes the text seem interactive. Blue is removed from the heading and from other non-interactive elements like the star rating.

### 7. Ensure interface elements have a 3:1 contrast ratio

Contrast measures the difference in perceived brightness between two colours, expressed as a ratio from **1:1 to 21:1** (black on black = 1:1; black on white = 21:1).

Aim to at least meet **WCAG 2.1 Level AA**: user interface elements — form fields, icons, buttons — need a minimum **3:1** contrast ratio, so people with vision impairments can clearly see interface details.

**Tools**: [WebAIM contrast checker](https://webaim.org/resources/contrastchecker/) (online), Contrast (Figma plugin).

**In the example**:
- The arrow icon sitting on a photo has too little contrast. Making the icon grey and adding a **solid white background** gives it 3:1 contrast regardless of the photo behind it — and reduces interaction cost, since the tap area is now larger and clearly visible.
- The low-contrast primary button risks not being identified as a button at all by low-vision users, who can't see the button shape. Raising it above 3:1 makes it accessible *and* corrects the visual hierarchy.

### 8. Ensure text has a 4.5:1 contrast ratio

Text needs stronger contrast than UI components to meet WCAG 2.1 Level AA:

| Text size | Minimum contrast ratio |
| --- | --- |
| Small text (18px and under) | 4.5:1 |
| Large text (above 18px bold, or above 24px regular) | 3:1 |

**In the example**: the small text in the photo-count element is raised above 4.5:1 by increasing the opacity of its grey container and adding a text shadow. The location text — made harder to read by its thin font weight — is changed to a darker grey.

### 9. Don't rely on colour alone as an indicator

There are many types of colour blindness and they mainly affect men. Commonly people can't distinguish red from green; some can't see any colour at all. You can't rely on colour alone to convey meaning or distinguish elements — add another visual cue.

**In the example**: blue alone marks "reviews" as a link. With colour removed, it looks identical to surrounding text. **Underlining** the link differentiates it in the absence of colour.

---

## Typography

### 10. Use a single sans serif typeface

A **typeface** is a set of related fonts with a similar style (e.g. Helvetica). **Fonts** are variations within a typeface (Helvetica Bold and Helvetica Regular are two fonts in the Helvetica typeface).

A single sans serif typeface is safest for interface design — generally the most legible, neutral, and simple.

**In the example**: the heading uses a detailed serif typeface that's harder to read and carries a personality that may not match the app's photos. It's simplified to the same sans serif used for body text.

### 11. Use a typeface with taller lower case letters

**x-height** is the height of lowercase letters in a typeface. Look for typefaces with a taller x-height and greater letter spacing — they're more legible at small sizes.

**In the example**: League Spartan has a relatively low x-height and is swapped for **Inter**.

### 12. Limit the use of uppercase

Unless you're shouting, there aren't many valid reasons to use uppercase. IT'S LOUD AND DIFFICULT TO READ.

When reading, you recognise the **shape of a word** rather than each letter. Uppercase words all have the same rectangular shape, forcing letter-by-letter reading.

**In the example**: uppercase location text is changed to **sentence case** — only the first word and proper nouns (names of people, places, or things) capitalised.

### 13. Use regular and bold font weights only

Having many weights available doesn't mean you should use them all. Many weights add noise and clutter, and make consistent usage harder. Keep the design system simple with **regular and bold only**. If bold is too heavy, use semi-bold instead.

Usage tips:

- Use **bold** for headings to emphasise them
- Use **regular** for other smaller text
- If you do use very thin or thick weights, reserve them for headings and large text — they're hard to read at small sizes

**In the example**: the location text uses a light weight. Even with contrast above 4.5:1, the thin characters remain hard to read for some — increasing it to regular improves readability and simplifies the design.

### 14. Avoid pure black text

Black has 0% colour brightness, white has 100%. That large difference makes our eyes work harder, so pure black on white causes eye strain and fatigue when reading. Use a **dark grey** instead.

**In the example**: pure black is changed to dark grey throughout. The overly prominent property description text is also set in a **lighter grey** to lower its prominence and correct the visual hierarchy (see #4).

### 15. Left align text

English is read left to right, downwards in an **F-shaped pattern**, so left-aligned text is optimal for readability. For long body text, avoid centre-aligned or justified text — it's harder to read, especially for people with cognitive disabilities.

Centre alignment can work for headings and short text that's read quickly. It hurts longer body text because the starting point of each line constantly changes, forcing your eyes to hunt for it.

**In the example**: the centre-aligned property description is left aligned, which also makes it consistent with the text above it.

### 16. Use at least 1.5 line height for body text

Line height is the vertical distance between two lines of text. The space between lines helps prevent rereading the same line and makes text feel more comfortable to read.

For accessibility and readability — especially for long body text — keep line height at least **1.5 (150%)**. Between **1.5 and 2** generally works well.

**In the example**: body text line height was only 1 (100%) and is raised to 1.5.

---

## Key Takeaway

A handful of simple but powerful guidelines finds and fixes a whole range of problems quickly. Decorative styles can then be customised to match a specific brand vibe — but get the basics right first.

UI design might look like a magical art form, but much of it is made up of logical guidelines like these. Using **objective logic rather than subjective opinion** makes it faster and easier to design intuitive, accessible, and beautiful interfaces. The more you apply them, the more naturally they become part of your design workflow — a solid base to build on as you experiment and stretch your creativity.

---

## References

- [16 Little UI Design Tips That Make a Big Impact — Adham Dannaway](https://www.adhamdannaway.com/blog/ui-design/ui-design-tips) (original)
- [큰 효과를 만드는 작은 UI 디자인 팁들 — GeekNews](https://news.hada.io/topic?id=31746) (Korean summary — condenses the list to 15 items)
- [Practical UI](https://www.practical-ui.com/) — the author's UI design book, source of these tips
