# King Performance Therapy Website — Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-page responsive website for King Performance Therapy with a scroll-synced video, specialties grid, USC affiliation, and waitlist form.

**Architecture:** Single `index.html` file with a `<style>` block and a `<script>` block. No build tools, no dependencies, no frameworks. Placeholder assets (images, video) used during development — client swaps them before launch. Formspree handles the waitlist form backend.

**Tech Stack:** HTML5, CSS3 (custom properties, grid, media queries), vanilla JavaScript (Intersection Observer, requestAnimationFrame)

**Spec:** `docs/superpowers/specs/2026-03-11-king-performance-therapy-website-design.md`

---

## File Structure

```
index.html          — entire site (HTML + inline CSS + inline JS)
assets/
  headshot.jpg      — hero background (placeholder during dev)
  video.mp4         — scroll-synced lifestyle video (placeholder during dev)
  usc-logo.png      — USC DPT logo (placeholder during dev)
  favicon.ico       — site favicon
```

All CSS lives in a `<style>` block in `<head>`. All JS lives in a `<script>` block before `</body>`. No external files beyond assets.

---

## Chunk 1: Foundation and Hero Section

### Task 1: HTML skeleton with CSS custom properties

**Files:**
- Create: `index.html`

- [ ] **Step 1: Create index.html with boilerplate, meta tags, and CSS custom properties**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="King Performance Therapy — physical therapy services in [City], CA. Specializing in sports rehab, orthopedic, neurological, and general wellness care.">
  <title>King Performance Therapy</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --color-accent: #7C9070;
      --color-accent-dark: #5E7255;
      --color-secondary: #C4785B;
      --color-bg: #FAF8F5;
      --color-bg-warm: #F3EDE7;
      --color-text: #2C2C2C;
      --color-text-light: #6B6B6B;
      --color-white: #FFFFFF;
      --font-heading: 'DM Sans', system-ui, -apple-system, sans-serif;
      --font-body: system-ui, -apple-system, -webkit-system-font, 'Segoe UI', Roboto, sans-serif;
      --radius: 12px;
      --radius-sm: 8px;
      --shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
      --max-width: 1100px;
      --section-padding: 80px 24px;
    }

    *, *::before, *::after {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    html {
      scroll-behavior: smooth;
    }

    body {
      font-family: var(--font-body);
      color: var(--color-text);
      background: var(--color-bg);
      line-height: 1.6;
      -webkit-font-smoothing: antialiased;
    }
  </style>
</head>
<body>
  <header id="hero"></header>
  <section id="video"></section>
  <main>
    <section id="specialties"></section>
    <section id="usc"></section>
    <section id="contact"></section>
  </main>
  <footer></footer>
  <script></script>
</body>
</html>
```

- [ ] **Step 2: Open in browser to verify blank page loads without errors**

Open `index.html` in a browser. Check the developer console — should be zero errors. Page should show a blank off-white background (`#FAF8F5`).

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add HTML skeleton with CSS custom properties"
```

### Task 2: Hero section with full-width background

**Files:**
- Modify: `index.html`
- Create: `assets/` directory (placeholder headshot)

- [ ] **Step 1: Create assets directory and a placeholder headshot**

Create `assets/` directory. Use a solid-color placeholder image or download a free stock headshot from Unsplash for development. Name it `assets/headshot.jpg`.

A simple placeholder can be generated:
```bash
mkdir -p assets
# Create a 1200x800 placeholder (gray rectangle) using ImageMagick if available,
# otherwise download a free stock photo. For now, just create the directory.
# The hero will use a CSS gradient fallback when no image is present.
```

- [ ] **Step 2: Add hero HTML structure inside the `<header id="hero">` tag**

Replace `<header id="hero"></header>` with:

```html
<header id="hero" role="img" aria-label="Portrait of Dr. [Name], physical therapist">
  <div class="hero-overlay"></div>
  <div class="hero-content">
    <p class="hero-label">King Performance Therapy</p>
    <h1 class="hero-name">Dr. [Name], DPT</h1>
    <p class="hero-tagline">Helping you move better, feel stronger, and live without limits.</p>
  </div>
  <a href="#specialties" class="scroll-indicator" aria-label="Scroll to learn more">
    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
      <polyline points="6 9 12 15 18 9"></polyline>
    </svg>
  </a>
</header>
```

- [ ] **Step 3: Add hero CSS inside the `<style>` block**

Add after the `body` rule:

```css
/* -- Hero -- */
#hero {
  position: relative;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  padding: 48px 24px;
  background: url('assets/headshot.jpg') center/cover no-repeat, linear-gradient(135deg, #8a9a7e, #6b7a60);
  color: var(--color-white);
  overflow: hidden;
}

.hero-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(0,0,0,0.55) 0%, rgba(0,0,0,0.1) 50%, transparent 100%);
  pointer-events: none;
}

.hero-content {
  position: relative;
  z-index: 1;
  max-width: var(--max-width);
}

.hero-label {
  font-family: var(--font-heading);
  font-size: 0.85rem;
  font-weight: 500;
  letter-spacing: 2.5px;
  text-transform: uppercase;
  margin-bottom: 12px;
  opacity: 0.9;
}

.hero-name {
  font-family: var(--font-heading);
  font-size: clamp(2rem, 5vw, 3.5rem);
  font-weight: 700;
  line-height: 1.15;
  margin-bottom: 12px;
}

.hero-tagline {
  font-size: clamp(1rem, 2.5vw, 1.25rem);
  max-width: 500px;
  opacity: 0.9;
  line-height: 1.5;
}

.scroll-indicator {
  position: absolute;
  bottom: 24px;
  left: 50%;
  transform: translateX(-50%);
  color: var(--color-white);
  opacity: 0.7;
  animation: bounce 2s ease-in-out infinite;
  text-decoration: none;
  z-index: 1;
}

@keyframes bounce {
  0%, 100% { transform: translateX(-50%) translateY(0); }
  50% { transform: translateX(-50%) translateY(8px); }
}
```

- [ ] **Step 4: Open in browser and verify**

Open `index.html`. You should see:
- Full-viewport hero section with gradient background (or headshot if provided)
- "KING PERFORMANCE THERAPY" label at bottom-left
- Doctor name in large heading
- Tagline below
- Bouncing down-arrow at bottom center

- [ ] **Step 5: Commit**

```bash
git add -A
git commit -m "feat: add hero section with full-width background and scroll indicator"
```

---

## Chunk 2: Scroll-Synced Video

### Task 3: Video section HTML and CSS

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add a placeholder video file or use a public-domain sample**

```bash
# For development, create a placeholder. The client will provide the real video.
# Option: download a short royalty-free clip, or test with any .mp4 you have.
# Place it at assets/video.mp4
```

- [ ] **Step 2: Replace `<section id="video"></section>` with video markup**

```html
<section id="video" aria-hidden="true">
  <video id="scroll-video" muted playsinline preload="auto" poster="assets/headshot.jpg">
    <source src="assets/video.mp4" type="video/mp4">
  </video>
</section>
```

- [ ] **Step 3: Add video section CSS**

Add to the `<style>` block:

```css
/* -- Scroll Video -- */
#video {
  position: relative;
  height: 70vh;
  overflow: hidden;
  background: var(--color-text);
}

#scroll-video {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}
```

- [ ] **Step 4: Open in browser and verify**

The video section should appear as a dark block (70vh tall) below the hero. If you have a video file, it should display the poster frame.

- [ ] **Step 5: Commit**

```bash
git add -A
git commit -m "feat: add video section HTML and CSS"
```

### Task 4: Scroll-sync JavaScript

**Files:**
- Modify: `index.html` (script block)

- [ ] **Step 1: Add scroll-sync logic inside the `<script>` block**

Replace the empty `<script></script>` with:

```html
<script>
(function() {
  'use strict';

  // -- Scroll-synced video --
  const video = document.getElementById('scroll-video');
  const videoSection = document.getElementById('video');

  if (video && videoSection) {
    const isMobile = () => window.innerWidth < 640;

    // Desktop/tablet: scroll-sync
    function syncVideoToScroll() {
      if (isMobile()) return;

      const rect = videoSection.getBoundingClientRect();
      const sectionTop = rect.top + window.scrollY;
      const sectionHeight = videoSection.offsetHeight;
      const viewportHeight = window.innerHeight;

      // Calculate scroll progress through the video section
      const scrollStart = sectionTop - viewportHeight;
      const scrollEnd = sectionTop + sectionHeight;
      const scrollRange = scrollEnd - scrollStart;
      const scrollPosition = window.scrollY - scrollStart;
      const progress = Math.max(0, Math.min(1, scrollPosition / scrollRange));

      if (video.duration) {
        video.currentTime = progress * video.duration;
      }
    }

    let ticking = false;
    window.addEventListener('scroll', function() {
      if (!ticking) {
        requestAnimationFrame(function() {
          syncVideoToScroll();
          ticking = false;
        });
        ticking = true;
      }
    }, { passive: true });

    // Mobile: auto-play on intersection
    if (isMobile()) {
      const observer = new IntersectionObserver(function(entries) {
        entries.forEach(function(entry) {
          if (entry.isIntersecting) {
            video.play().catch(function() {});
          } else {
            video.pause();
          }
        });
      }, { threshold: 0.3 });
      observer.observe(videoSection);
    }
  }
})();
</script>
```

- [ ] **Step 2: Test scroll-sync in browser**

Open `index.html` on desktop (window > 640px wide). Scroll down slowly past the hero into the video section. If a video file is present:
- Video should progress forward as you scroll down
- Video should rewind as you scroll up
- No jitter or lag (requestAnimationFrame smooths it)

Resize to mobile width (< 640px) and reload:
- Video should auto-play when scrolled into view
- Video should pause when scrolled away

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add scroll-synced video with mobile intersection observer fallback"
```

---

## Chunk 3: Specialties and USC Sections

### Task 5: Specialties section

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Replace `<section id="specialties"></section>` with specialties markup**

```html
<section id="specialties">
  <div class="container">
    <h2 class="section-title">Areas of Focus</h2>
    <p class="section-subtitle">Comprehensive physical therapy for every stage of recovery and wellness.</p>
    <div class="specialties-grid">
      <div class="specialty-card">
        <span class="specialty-icon" aria-hidden="true">🏃</span>
        <h3>Sports Rehab</h3>
        <p>Return to sport safely with evidence-based rehabilitation tailored to your activity and goals.</p>
      </div>
      <div class="specialty-card">
        <span class="specialty-icon" aria-hidden="true">🦴</span>
        <h3>Orthopedic / Post-Surgical</h3>
        <p>Recover strength and mobility after surgery or musculoskeletal injury with hands-on care.</p>
      </div>
      <div class="specialty-card">
        <span class="specialty-icon" aria-hidden="true">🧠</span>
        <h3>Neurological Rehab</h3>
        <p>Improve function and independence for neurological conditions through targeted therapy.</p>
      </div>
      <div class="specialty-card">
        <span class="specialty-icon" aria-hidden="true">💪</span>
        <h3>Chronic Pain Management</h3>
        <p>Address persistent pain with a whole-person approach combining movement, education, and manual therapy.</p>
      </div>
      <div class="specialty-card">
        <span class="specialty-icon" aria-hidden="true">🌿</span>
        <h3>General Wellness</h3>
        <p>Prevent injury and maintain your best physical health with proactive wellness programs.</p>
      </div>
      <div class="specialty-card">
        <span class="specialty-icon" aria-hidden="true">⚖️</span>
        <h3>Balance & Fall Prevention</h3>
        <p>Build confidence and stability with assessments and exercises designed to reduce fall risk.</p>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Add specialties CSS**

Add to the `<style>` block:

```css
/* -- Shared section styles -- */
.container {
  max-width: var(--max-width);
  margin: 0 auto;
}

.section-title {
  font-family: var(--font-heading);
  font-size: clamp(1.5rem, 4vw, 2.25rem);
  font-weight: 700;
  text-align: center;
  margin-bottom: 8px;
}

.section-subtitle {
  text-align: center;
  color: var(--color-text-light);
  max-width: 550px;
  margin: 0 auto 48px;
  font-size: 1.05rem;
}

/* -- Specialties -- */
#specialties {
  padding: var(--section-padding);
  background: var(--color-bg);
}

.specialties-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 20px;
}

.specialty-card {
  background: var(--color-white);
  border-radius: var(--radius);
  padding: 28px 24px;
  box-shadow: var(--shadow);
  text-align: center;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.specialty-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
}

.specialty-icon {
  font-size: 2rem;
  display: block;
  margin-bottom: 12px;
}

.specialty-card h3 {
  font-family: var(--font-heading);
  font-size: 1.1rem;
  font-weight: 600;
  margin-bottom: 8px;
  color: var(--color-text);
}

.specialty-card p {
  font-size: 0.95rem;
  color: var(--color-text-light);
  line-height: 1.55;
}

/* Tablet: 2 columns */
@media (min-width: 640px) {
  .specialties-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* Desktop: 3 columns */
@media (min-width: 1024px) {
  .specialties-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
```

- [ ] **Step 3: Open in browser and verify**

Check at three widths:
- Mobile (< 640px): cards stack in single column
- Tablet (640–1024px): 2-column grid
- Desktop (> 1024px): 3-column grid (6 cards, 2 rows)

Cards should have white backgrounds, rounded corners, soft shadows, and a subtle lift on hover.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add specialties section with responsive icon card grid"
```

### Task 6: USC affiliation section

**Files:**
- Modify: `index.html`
- Create: `assets/usc-logo.png` (placeholder)

- [ ] **Step 1: Create a placeholder USC logo**

```bash
# Use a placeholder image for development. Client provides the real logo.
# A simple text-based SVG placeholder can be inlined if needed.
```

- [ ] **Step 2: Replace `<section id="usc"></section>` with USC markup**

```html
<section id="usc">
  <div class="container usc-content">
    <a href="https://pt.usc.edu/" target="_blank" rel="noopener noreferrer">
      <img src="assets/usc-logo.png" alt="University of Southern California logo" class="usc-logo">
    </a>
    <p class="usc-credential">USC Doctor of Physical Therapy</p>
  </div>
</section>
```

- [ ] **Step 3: Add USC section CSS**

```css
/* -- USC Affiliation -- */
#usc {
  padding: var(--section-padding);
  background: var(--color-bg-warm);
  text-align: center;
}

.usc-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16px;
}

.usc-logo {
  max-width: 180px;
  height: auto;
  opacity: 0.85;
  transition: opacity 0.2s ease;
}

.usc-logo:hover {
  opacity: 1;
}

.usc-credential {
  font-family: var(--font-heading);
  font-size: 1.1rem;
  font-weight: 500;
  color: var(--color-text-light);
}
```

- [ ] **Step 4: Verify in browser**

Section should show centered logo (or broken image placeholder) with the credential text below on a warm off-white background.

- [ ] **Step 5: Commit**

```bash
git add -A
git commit -m "feat: add USC affiliation section with logo and credential"
```

---

## Chunk 4: Contact, Waitlist, and Final Polish

### Task 7: Contact and waitlist section

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Replace `<section id="contact"></section>` with contact markup**

```html
<section id="contact">
  <div class="container">
    <h2 class="section-title">Get in Touch</h2>
    <p class="section-subtitle">We'd love to hear from you.</p>
    <div class="contact-grid">
      <div class="contact-info">
        <div class="contact-item">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,13 2,6"/></svg>
          <a href="mailto:hello@kingpt.com">hello@kingpt.com</a>
        </div>
        <div class="contact-item">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/></svg>
          <span>123 Main Street, Suite 100<br>Los Angeles, CA 90001</span>
        </div>
        <div class="notice">
          <p>Not currently accepting new patients — join our waitlist to be notified when availability opens.</p>
        </div>
      </div>
      <div class="waitlist-form-wrapper">
        <h3 class="form-title">Join the Waitlist</h3>
        <form id="waitlist-form" action="https://formspree.io/f/YOUR_FORM_ID" method="POST">
          <label for="name" class="sr-only">Name</label>
          <input type="text" id="name" name="name" placeholder="Name" required>
          <label for="email" class="sr-only">Email</label>
          <input type="email" id="email" name="email" placeholder="Email" required>
          <label for="message" class="sr-only">Message (optional)</label>
          <textarea id="message" name="message" placeholder="Brief message (optional)" rows="3"></textarea>
          <button type="submit">Join Waitlist</button>
        </form>
        <div id="form-success" class="form-success" hidden>
          <p>Thanks for your interest! We'll be in touch when availability opens.</p>
        </div>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Add contact/waitlist CSS**

```css
/* -- Contact & Waitlist -- */
#contact {
  padding: var(--section-padding);
  background: var(--color-bg);
}

.contact-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 40px;
}

@media (min-width: 640px) {
  .contact-grid {
    grid-template-columns: 1fr 1fr;
    gap: 48px;
    align-items: start;
  }
}

.contact-info {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.contact-item {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  color: var(--color-text);
  line-height: 1.5;
}

.contact-item svg {
  flex-shrink: 0;
  color: var(--color-accent);
  margin-top: 2px;
}

.contact-item a {
  color: var(--color-text);
  text-decoration: none;
  border-bottom: 1px solid transparent;
  transition: border-color 0.2s ease;
}

.contact-item a:hover {
  border-bottom-color: var(--color-accent);
}

.notice {
  background: #FDF5F0;
  border: 1px solid #E8D5CA;
  border-left: 3px solid var(--color-secondary);
  border-radius: var(--radius-sm);
  padding: 16px;
}

.notice p {
  font-size: 0.92rem;
  color: #7A5A45;
  line-height: 1.5;
}

/* -- Waitlist Form -- */
.waitlist-form-wrapper {
  background: var(--color-white);
  border-radius: var(--radius);
  padding: 32px 28px;
  box-shadow: var(--shadow);
}

.form-title {
  font-family: var(--font-heading);
  font-size: 1.15rem;
  font-weight: 600;
  margin-bottom: 20px;
}

#waitlist-form {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

#waitlist-form input,
#waitlist-form textarea {
  font-family: var(--font-body);
  font-size: 0.95rem;
  padding: 12px 16px;
  border: 1px solid #DDD;
  border-radius: var(--radius-sm);
  background: var(--color-bg);
  color: var(--color-text);
  transition: border-color 0.2s ease;
}

#waitlist-form input:focus,
#waitlist-form textarea:focus {
  outline: none;
  border-color: var(--color-accent);
  box-shadow: 0 0 0 3px rgba(124, 144, 112, 0.15);
}

#waitlist-form textarea {
  resize: vertical;
  min-height: 80px;
}

#waitlist-form button {
  font-family: var(--font-heading);
  font-size: 1rem;
  font-weight: 600;
  padding: 14px 24px;
  background: var(--color-accent);
  color: var(--color-white);
  border: none;
  border-radius: var(--radius-sm);
  cursor: pointer;
  transition: background 0.2s ease;
}

#waitlist-form button:hover {
  background: var(--color-accent-dark);
}

.form-success p {
  color: var(--color-accent-dark);
  font-weight: 500;
  text-align: center;
  padding: 20px 0;
}

/* Screen reader only */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

- [ ] **Step 3: Add form submission JavaScript**

Add inside the `(function() { ... })()` script block, after the video code:

```javascript
// -- Waitlist form --
const form = document.getElementById('waitlist-form');
const formSuccess = document.getElementById('form-success');

if (form && formSuccess) {
  form.addEventListener('submit', function(e) {
    e.preventDefault();
    const data = new FormData(form);

    fetch(form.action, {
      method: 'POST',
      body: data,
      headers: { 'Accept': 'application/json' }
    })
    .then(function(response) {
      if (response.ok) {
        form.hidden = true;
        formSuccess.hidden = false;
      } else {
        alert('Something went wrong. Please try again or email us directly.');
      }
    })
    .catch(function() {
      alert('Something went wrong. Please try again or email us directly.');
    });
  });
}
```

- [ ] **Step 4: Verify in browser**

Check at mobile and desktop widths:
- Mobile: stacked — contact info on top, form below
- Desktop: side-by-side — contact info left, form right
- "Not accepting patients" notice should have a warm beige/terracotta left border
- Form fields should have focus states (green outline)
- Submit button should be sage green with darker hover

Note: Form submission won't work until a real Formspree form ID is configured. The JS handles the success/error states correctly.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add contact section with waitlist form and Formspree integration"
```

### Task 8: Footer and final polish

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Replace `<footer></footer>` with footer markup**

```html
<footer>
  <p>&copy; 2026 King Performance Therapy. All rights reserved.</p>
</footer>
```

- [ ] **Step 2: Add footer CSS**

```css
/* -- Footer -- */
footer {
  text-align: center;
  padding: 24px;
  background: var(--color-bg-warm);
  color: var(--color-text-light);
  font-size: 0.85rem;
}
```

- [ ] **Step 3: Verify complete page flow**

Open `index.html` and scroll through the entire page. Verify:
1. Hero fills viewport with gradient background, text at bottom-left, bouncing arrow
2. Video section appears as 70vh dark block
3. Specialties grid shows 6 cards (responsive columns)
4. USC section centered on warm background
5. Contact section with side-by-side layout (stacks on mobile)
6. Footer at bottom
7. No console errors
8. Smooth scroll when clicking the down arrow

- [ ] **Step 4: Test responsive breakpoints**

Using browser dev tools, check at:
- 375px wide (iPhone SE): all sections single-column, text readable
- 768px wide (tablet): specialties 2-col, contact side-by-side
- 1200px wide (desktop): specialties 3-col, full layout

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add footer and verify complete page layout"
```

### Task 9: Add .gitignore and clean up

**Files:**
- Create: `.gitignore`

- [ ] **Step 1: Create .gitignore**

```
.DS_Store
.superpowers/
```

- [ ] **Step 2: Commit**

```bash
git add .gitignore
git commit -m "chore: add .gitignore"
```
