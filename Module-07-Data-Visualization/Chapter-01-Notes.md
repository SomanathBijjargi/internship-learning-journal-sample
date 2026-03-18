# Reveal.js

RevealJS is a **JavaScript-based HTML presentation framework** used to create interactive slides in a web browser.

It converts **simple Markdown or HTML into professional slides**.

---

## 📌 Where is it Used?

RevealJS is useful for:

1. **Data Presentations** → charts, dashboards, live demos
2. **Technical Talks** → code snippets, syntax highlighting
3. **Web Portfolios** → project showcases
4. **Educational Content** → interactive learning slides
5. **Conference Talks** → speaker notes, remote presentations
6. **Documentation** → API & library demos

---

## ⚙️ Installation

### Prerequisite

* Install **Node.js**

### Steps

1. Clone the repository:

```bash
git clone https://github.com/hakimel/reveal.js.git
```

2. Move into the folder:

```bash
cd reveal.js
```

3. Install dependencies:

```bash
npm install
```

4. Start the server:

```bash
npm start
```

👉 Open browser: `http://localhost:8000`

---

## 🧠 Basic Concept

* A presentation = **multiple slides**
* Each slide is written in **Markdown or HTML**

👉 In Markdown:

```markdown
---
```

➡️ Creates a new slide

---

## 📝 Simple Slide Example (Markdown)

```markdown
# My First Slide

- Hello World
- This is RevealJS

---

## Second Slide

- Another slide
```

---

## 🧩 HTML Slide Example

```html
<div class="reveal">
  <div class="slides">
    <section>Slide 1</section>
    <section>Slide 2</section>
  </div>
</div>
```

👉 Each `<section>` = One slide

---

## 💻 Code Highlighting Example

```python
def greet():
    print("Hello RevealJS")
```

👉 Supports multiple languages (Python, JS, C++, etc.)

---

## 🎯 Key Features

* Slide navigation (keyboard support)
* Code syntax highlighting
* Nested (vertical) slides
* Animations & transitions
* Multimedia support (images, videos)
* Export to PDF

---

## 🔽 Vertical Slides (Nested)

```markdown
## Main Topic

---

### Sub Topic 1

---

### Sub Topic 2
```

👉 Used for **subtopics inside a slide**

---

## ⏸️ Pause Animation

```markdown
- First point

. . .

- Second point
```

👉 Used to reveal content step-by-step

---

## 📊 Columns Layout

```markdown
:::: {.columns}

::: {.column width="50%"}
Left Content
```

---

## ✅ Advantages

- Free and open-source  
- Works in browser  
- Easy to customize  
- Supports code & multimedia  

---

## ❌ Disadvantages

- Requires basic coding knowledge  
- Setup needed (Node.js, npm)  

---

---
# Creating Presentations with Marp

Marp allows you to create presentations directly from your text editor using Markdown.

## Getting Started
*   **Installation:** Use Marp as a VS Code extension by searching for and installing "Marp for VS Code" from the Extensions panel.
*   **Initialization:** Start a presentation by adding a YAML frontmatter block at the top of a new file with three dashes, `marp: true`, and closing with three dashes.
*   **Previewing:** Click the "Open Preview" button in VS Code to see your slides.

## Content & Formatting
*   **Basic Syntax:** Marp uses CommonMark Markdown, so you can use standard syntax for headers and bulleted lists.
*   **Lists & Animations:** Using asterisks (`*`) creates a "fragmented list" where items appear one at a time. Using dashes (`-`) makes all bullets appear simultaneously.
*   **Code Blocks:** Create code blocks using triple backticks. Specify the language after the first backticks to apply syntax highlighting via Highlight.js.
*   **Math Typesetting:** Add the `math` directive (set to `KaTeX` or `MathJax`) to write single-line expressions with a single dollar sign (`$`) or multi-line expressions with double dollar signs (`$$`).
*   **HTML & Emojis:** Marp supports emojis and custom HTML, letting you use `<span>` tags to style specific words.

## Themes & Slide Styling
*   **Built-in Themes:** Use the `theme` directive to select from `default`, `uncover`, or `gaia`.
*   **Dark Mode:** Add the `class: invert` directive for a dark mode version of a theme.
*   **Slide-Specific Styling:** Use `spot` directives to change the style of a single slide, like setting the background color to black and the font color to red.
*   **Impactful Text:** Use the `fit` directive to stretch a header word to the full extents of the slide.

## Advanced Layouts
*   **Image on One Side, Text on the Other:** Use the `bg` argument with a directional argument (like `left`) when inserting an image, then add your text to place them side-by-side.
*   **Two Columns of Text:** This requires enabling custom CSS in the Marp plugin settings, adding a custom styling snippet at the top of the document, and creating a `<div class="columns">` with two sub-divs.

## Exporting Options
*   **HTML:** The best option, supporting the most features. It creates a single file with a bottom toolbar, full-screen mode, presenter view, and support for fragmented lists.
*   **PDF:** Usable but misses features like fragmented lists, and some styling (like shadow gradients around code blocks) might render as solid colors instead of gradients.
*   **PowerPoint:** Preserves accurate visual styling, but renders slides as non-editable static images. You can still add transition animations or extra content on top in PowerPoint.

---

# Python Notebooks with Marimo

Marimo is an open-source, reactive notebook for Python designed to solve common issues found in traditional notebooks, such as hidden state and reproducibility problems. 

## Core Philosophy & Mechanics
*   **The Hidden State Problem:** Traditional notebooks often suffer from hidden state issues where the code on the page does not match the outputs because cells can be executed out of order. Marimo fixes this by ensuring the variables in memory and outputs on the page depend *only* on the code.
*   **Reactive Execution:** Marimo operates similarly to a spreadsheet (like Excel). If you update a variable in one cell, all other cells that depend on that variable automatically recalculate. 
*   **Directed Acyclic Graph (DAG):** Marimo statically analyzes all cells before they run to form a DAG based on variable definitions and references, understanding exactly how data flows through the notebook.
*   **Design Constraints:** To enable this reactivity, Marimo enforces certain rules: variable reassignment across cells is banned (you cannot declare the same variable in multiple cells), cycles are not allowed, and mutating variables across cells is heavily discouraged.

## Interactivity & UI
*   **Python-Native Markdown:** Instead of standard markdown cells, you use Python strings. By importing Marimo (e.g., `import marimo as mo`) and using the `mo.md()` function, you can dynamically interpolate Python values directly into your markdown and LaTeX equations.
*   **Built-in UI Elements:** Marimo comes with interactive UI elements like sliders. When linked to a variable (e.g., the amplitude of a sine wave), scrubbing the slider will instantly update any connected plots, tables, or markdown text.
*   **Interactive Visualizations:** You can seamlessly integrate libraries like Altair to create interactive charts. Selecting data points on a plot can automatically filter and update downstream elements like tables or image previews.

## File Format & Version Control
*   **Pure Python Files:** Unlike traditional notebooks that save as clunky JSON files with embedded outputs, Marimo saves as pure Python (`.py`) files. 
*   **Git-Friendly:** Because it uses a pure Python format, making a small change to your notebook code guarantees a clean, readable Git diff, making notebooks maintainable like standard software artifacts.

## Deployment & Reusability
*   **Run as a Script:** You can execute a Marimo notebook directly from the command line just like a standard Python script (e.g., `python notebook.py`). It even supports passing command-line arguments to set initial variable values.
*   **Serve as a Web App:** By hiding the code cells and using the command `marimo run`, you can serve your notebook as a read-only, interactive web application where multiple connected clients get their own isolated sessions.
*   **In-Browser Execution:** Marimo can run entirely within the web browser without a backend using the Pyodide library (accessible via `marimo.app`), which is highly useful for educational purposes. 

## Fun Fact
*   **Name Origin:** The project is named after the "marimo" moss ball, a type of algae found in lakes in Japan and Europe that clumps together into adorable spheres.
