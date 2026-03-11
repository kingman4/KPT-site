# King Performance Therapy — Website Design Spec

## Overview

A single-page, responsive website for King Performance Therapy, a part-time physical therapy private practice. The site establishes the practice's warm, approachable identity, showcases specialties, highlights USC credentials, and provides contact information with a waitlist signup (no new patients currently accepted).

## Technical Approach

- **Static HTML/CSS/JS** — single `index.html` with inline styles and a small script for scroll-synced video
- **No build tools or frameworks** — vanilla implementation, zero dependencies
- **Hosting** — any static host (GitHub Pages, Netlify, Vercel, etc.)
- **Responsive** — mobile-first CSS with media queries for tablet and desktop breakpoints

## Page Structure

Five sections in order, forming a narrative: who → vibe → what → credibility → get in touch.

### 1. Hero / Landing

Full-viewport section with the practitioner's headshot as a full-width background image.

- Practice name "KING PERFORMANCE THERAPY" displayed prominently
- Doctor's name and "DPT" credential
- Warm, approachable tagline (content TBD)
- Text positioned at bottom-left over a subtle gradient overlay for readability against the photo
- Scroll indicator (subtle down arrow or animated cue) to hint at more content below
- On mobile: same full-width photo, text repositioned/resized for small screens

### 2. Scroll-Synced Video

Inline block section (60–80vh tall) containing a lifestyle/movement brand video.

- Video playback position is tied to the user's scroll position within the element's visible range
- As the user scrolls down, the video plays forward; scrolling up rewinds
- Implementation: `requestAnimationFrame` loop reading `scrollY` relative to the video element's offset, setting `video.currentTime` accordingly
- The video file will be provided by the client (lifestyle/wellness/movement content)
- On mobile: same scroll-sync behavior with reduced section height; fallback to auto-play on intersection if scroll-sync performs poorly on the device

### 3. Specialties

Section heading (e.g., "What We Treat" or "Areas of Focus") followed by an icon card grid.

- **Layout:** 3 columns on desktop, 2 on tablet, 1 on mobile
- **Each card:** icon + specialty name + brief description (1–2 sentences)
- **Specialties** (general PT practice, exact list TBD):
  - Sports Rehab / Return to Sport
  - Orthopedic / Post-Surgical
  - Neurological Rehab
  - Chronic Pain Management
  - General Wellness / Injury Prevention
  - (Possibly 1–2 more based on client input)
- Icons: simple line icons or emoji — consistent style across all cards

### 4. USC Affiliation

Minimal section establishing academic credentials.

- USC logo displayed centered
- Brief credential line (e.g., "USC Doctor of Physical Therapy")
- Clean design — generous whitespace, logo does the heavy lifting
- Logo asset will need to be provided or sourced with appropriate usage rights

### 5. Contact & Waitlist

Side-by-side layout that stacks vertically on mobile.

**Left column — Contact Info:**
- Email address
- Physical office address
- "Not currently accepting new patients" notice — styled warmly but clearly (e.g., soft background with a warm-toned border, not aggressive red)

**Right column — Waitlist Form:**
- Fields: Name, Email, Brief message (optional)
- Submit button: "Join Waitlist"
- **Form backend:** Formspree, Netlify Forms, or similar no-code service (decision deferred to implementation — no custom backend)
- Success state: inline confirmation message replacing the form after submission

## Visual Design

### Tone
Warm and approachable — the site should feel like a welcoming small practice, not a corporate healthcare chain.

### Color Palette
- Warm neutrals as base (off-whites, soft tans, warm grays)
- One accent color for buttons, icons, and highlights (warm tone — e.g., terracotta, sage, or golden)
- Dark text on light backgrounds for readability
- Exact palette to be determined during implementation

### Typography
- One Google Font (or system font stack) for headings — warm, modern sans-serif
- System font stack for body text
- Generous line height and readable font sizes

### Spacing & Shape
- Rounded corners on cards, buttons, and form inputs
- Generous whitespace between sections
- Soft shadows on cards (subtle depth, not harsh)

## Responsive Breakpoints

| Breakpoint | Target |
|---|---|
| < 640px | Mobile (single column, stacked layout) |
| 640–1024px | Tablet (2-column specialties grid, side-by-side contact) |
| > 1024px | Desktop (3-column specialties grid, full layout) |

## Assets Required from Client

- High-quality headshot photo (hero background)
- Lifestyle/movement video file (scroll-synced section)
- USC logo (or confirmation of usage rights)
- Final copy: tagline, specialty descriptions, contact details
- Preferred accent color (or approval of designer's choice)

## Out of Scope

- Multi-page navigation
- Blog or content management
- Online booking/scheduling
- Payment processing
- Patient portal
- Analytics (can be added trivially later with a script tag)
- SEO optimization beyond basic meta tags
