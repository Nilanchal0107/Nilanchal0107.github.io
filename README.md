# 🌐 CS50x Week 8 — Homepage

![HTML5](https://img.shields.io/badge/HTML5-Markup-orange)
![CSS3](https://img.shields.io/badge/CSS3-Styling-blue)
![JavaScript](https://img.shields.io/badge/JavaScript-Interactivity-yellow)
![Bootstrap](https://img.shields.io/badge/Bootstrap-4.5.3-purple)

A personal portfolio website built with HTML, CSS, JavaScript, and Bootstrap — featuring 4 pages, responsive design, and multiple interactive JavaScript features.

---

## 📁 Project Structure

```
homepage/
├── index.html          ← Main page (quote + social links + profile)
├── about.html          ← Skills, education, currently section
├── projects.html       ← Project cards with JS filter buttons
├── contact.html        ← Contact form with JS validation
├── styles.css          ← All custom CSS (shared across all pages)
└── specification.txt   ← CS50 requirement checklist
```

---

## 🚀 How to Run

No server needed — just open in a browser:

```bash
# Option 1: open directly
open index.html

# Option 2: use VS Code Live Server extension
# Right-click index.html → Open with Live Server
```

---

## 📄 Pages

### `index.html` — Home
- Hero quote: *"Human by Nature, Developer by Choice."*
- 6 social media icon cards (GitHub, LinkedIn, Gmail, Twitter, Discord, Instagram)
- Profile photo with click-to-expand modal
- CTA section about Smart India Hackathon
- Connect button linking to contact page

### `about.html` — About
- Who I Am section
- Skills displayed as tag pills
- Currently section (CS50x, Scrimba, SIH prep)
- Education (APSIT, 2025–29)

### `projects.html` — Projects
- 3 project cards (FinHabits 2.0, Portfolio, CS50x)
- **JavaScript filter buttons** — filter by All / Full Stack / AI / Frontend
- Each card has tech stack tags and a GitHub link

### `contact.html` — Contact
- Contact form with Name, Email, Message fields
- **JavaScript validation** — checks empty fields and email format
- Direct links to Gmail, LinkedIn, GitHub, Twitter

---

## 🧠 Core Concepts Explained

### 1. HTML Structure — Semantic Tags

HTML tags give meaning to content. This project uses 10+ distinct tags:

```html
<nav>       ← navigation bar
<h1>–<h3>  ← headings (hierarchy matters for SEO)
<p>         ← paragraph text
<a>         ← hyperlinks (href links pages together)
<img>       ← images (profile photo, social icons)
<div>       ← generic container for grouping
<span>      ← inline container (used for icon labels)
<ul><li>    ← unordered lists
<input>     ← form text fields
<textarea>  ← multi-line form input
<button>    ← clickable buttons
<svg>       ← inline vector graphics (LinkedIn icon)
<footer>    ← page footer
<meta>      ← metadata (charset, viewport, author)
<link>      ← links external CSS and fonts
<script>    ← embeds JavaScript
```

**Template inheritance via Bootstrap's navbar** — the same `<nav>` block is copied across all 4 pages so navigation is consistent.

---

### 2. CSS — Styling and Layout

#### CSS Variables
Defined once at the top, used everywhere — change one value to retheme the whole site:

```css
:root {
    --bg-deep:    #000328;
    --cyan:       #00eaff;
    --font-display: 'Montserrat', sans-serif;
}

/* Used anywhere in the file */
color: var(--cyan);
background: var(--bg-deep);
```

#### CSS Selectors (5+ used)
```css
body { }              /* tag selector — targets all <body> elements */
.icon-card { }        /* class selector — targets class="icon-card" */
#imageModal { }       /* ID selector — targets id="imageModal" */
.icon-card:hover { }  /* pseudo-class — applies on mouse hover */
@media (max-width: 768px) { }  /* media query — mobile responsive */
```

#### CSS Properties (5+ used)
```css
background        /* gradient backgrounds */
border-radius     /* rounded corners */
box-shadow        /* glow effects */
transition        /* smooth hover animations */
font-family       /* custom Google Fonts */
grid-template-columns  /* CSS Grid for icon layout */
display: flex     /* Flexbox for profile section alignment */
```

#### CSS Grid vs Flexbox
```css
/* Grid — used for the 3×2 social media icon layout */
.icon-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);  /* 3 equal columns */
    gap: 20px;
}

/* Flexbox — used for profile section (image + text side by side) */
.profile-box {
    display: flex;
    align-items: center;
    gap: 36px;
}
```

#### Responsive Design
```css
/* Stacks layout vertically on small screens */
@media (max-width: 768px) {
    .profile-box {
        flex-direction: column;  /* side-by-side → stacked */
    }
    .icon-grid {
        grid-template-columns: repeat(2, 1fr);  /* 3 cols → 2 cols */
    }
}
```

#### `clamp()` for Fluid Typography
```css
/* Font size scales smoothly between screen sizes */
font-size: clamp(32px, 6vw, 62px);
/* min: 32px | preferred: 6% of viewport width | max: 62px */
```

---

### 3. JavaScript — Interactivity

#### Feature 1: Image Modal (index.html)
Clicking the profile picture opens a full-screen overlay:

```javascript
const profilePic = document.getElementById('profilePic');
const modal = document.getElementById('imageModal');

// Open modal on click
profilePic.addEventListener('click', () => {
    modal.classList.add('active');   // CSS shows it when class = 'active'
});

// Close on X button, background click, or Escape key
closeBtn.addEventListener('click', () => modal.classList.remove('active'));
modal.addEventListener('click', e => {
    if (e.target === modal) modal.classList.remove('active');
});
document.addEventListener('keydown', e => {
    if (e.key === 'Escape') modal.classList.remove('active');
});
```

**Concepts:** `getElementById`, `addEventListener`, `classList.add/remove`, event object `e.target`

---

#### Feature 2: Gmail Mobile Handler (index.html)
On mobile, clicking Gmail tries the native mail app first, then falls back to Gmail web:

```javascript
function isMobile() {
    return /Android|iPhone|iPad/i.test(navigator.userAgent);
}

gmailAnchor.addEventListener('click', function(e) {
    if (!isMobile()) return;  // desktop: use default mailto behavior

    e.preventDefault();       // stop default link behavior
    window.location.href = 'mailto:' + email;  // try native app

    setTimeout(function() {
        window.open(gmailWebURL, '_blank');  // fallback after 800ms
    }, 800);
});
```

**Concepts:** `navigator.userAgent`, regex testing, `e.preventDefault()`, `setTimeout`

---

#### Feature 3: Project Filter Buttons (projects.html)
Clicking filter buttons shows/hides project cards by category:

```javascript
function filterProjects(category, btn) {
    // Update active button styling
    document.querySelectorAll('.filter-btn').forEach(b => {
        b.classList.remove('active');
    });
    btn.classList.add('active');

    // Show/hide cards based on data-category attribute
    document.querySelectorAll('.project-card').forEach(card => {
        if (category === 'all') {
            card.style.display = 'block';
        } else {
            const cats = card.getAttribute('data-category') || '';
            card.style.display = cats.includes(category) ? 'block' : 'none';
        }
    });
}
```

```html
<!-- Cards store their categories in a data attribute -->
<div class="project-card" data-category="fullstack ai">
```

**Concepts:** `querySelectorAll`, `forEach`, `getAttribute`, `style.display`, `data-*` attributes

---

#### Feature 4: Contact Form Validation (contact.html)
Validates form fields before "submitting":

```javascript
function handleSubmit() {
    const name    = document.getElementById('nameInput').value.trim();
    const email   = document.getElementById('emailInput').value.trim();
    const message = document.getElementById('msgInput').value.trim();
    const feedback = document.getElementById('form-feedback');

    // Check empty fields
    if (!name || !email || !message) {
        feedback.textContent = 'Please fill in all fields.';
        feedback.className = 'error';
        return;
    }

    // Basic email format check
    if (!email.includes('@') || !email.includes('.')) {
        feedback.textContent = 'Please enter a valid email address.';
        feedback.className = 'error';
        return;
    }

    // Success
    feedback.textContent = '✓ Message sent!';
    feedback.className = 'success';
}
```

**Concepts:** `.value.trim()`, conditional logic, `textContent`, `className`

---

### 4. Bootstrap — Component Library

Bootstrap is included via CDN in every page's `<head>`:

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css" ...>
<script src="https://code.jquery.com/jquery-3.5.1.slim.min.js" ...></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/js/bootstrap.bundle.min.js" ...></script>
```

Bootstrap features used:

```html
<!-- Responsive collapsible navbar -->
<nav class="navbar navbar-expand-lg navbar-dark">
    <button class="navbar-toggler" data-toggle="collapse" data-target="#navMenu">

<!-- Grid system -->
<div class="container">

<!-- Utility classes -->
class="ml-auto"     ← push nav links to the right
class="mt-3"        ← margin top
class="text-center" ← center text
```

---

### 5. CSS Animations

```css
/* Define the animation */
@keyframes fadeUp {
    from { opacity: 0; transform: translateY(28px); }
    to   { opacity: 1; transform: translateY(0); }
}

/* Apply it to elements */
.animate-in {
    opacity: 0;
    animation: fadeUp 0.7s ease forwards;
}
```

Elements stagger their entrance using `animation-delay`:
```html
<div class="animate-in" style="animation-delay: 0.15s">
<div class="animate-in" style="animation-delay: 0.30s">
<div class="animate-in" style="animation-delay: 0.45s">
```

---

## ✅ CS50 Requirements Met

| Requirement | Status |
|---|---|
| At least 4 HTML pages | ✅ index, about, projects, contact |
| Pages link to each other | ✅ Navbar on every page |
| 10+ distinct HTML tags | ✅ nav, h1–h3, p, a, img, div, span, ul, li, input, textarea, button, svg, footer, meta, link, script |
| Bootstrap integrated | ✅ Navbar, utility classes, CDN included |
| styles.css with 5+ selectors | ✅ body, .icon-card, #imageModal, :hover, @media |
| styles.css with 5+ properties | ✅ background, border-radius, box-shadow, transition, font-family, grid-template-columns |
| JavaScript interactivity | ✅ Modal, Gmail handler, filter buttons, form validation |
| Mobile responsive | ✅ Flexbox, Grid, media queries, clamp() |
| specification.txt | ✅ Included |
