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
