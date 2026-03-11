# King Performance Therapy

Single-page website for King Performance Therapy, a physical therapy private practice.

## Setup

1. Copy the config template:
   ```bash
   cp config.example.js config.js
   ```

2. Edit `config.js` with your information:
   ```js
   var CONFIG = {
     doctorName: "Dr. Jane Smith, DPT",
     tagline: "Your tagline here.",
     email: "you@example.com",
     address: "123 Main St, Suite 100<br>City, CA 90001",
     formspreeId: "abc1234",  // from formspree.io
   };
   ```

3. Add your assets to the `assets/` folder:
   - `headshot.jpg` — hero background photo
   - `video.mp4` — lifestyle/movement video for the scroll section
   - `usc-logo.png` — USC logo

4. Open `index.html` in a browser.

## Formspree

The waitlist form uses [Formspree](https://formspree.io) to collect submissions.

1. Create a free account at formspree.io
2. Create a new form
3. Copy the form ID (the part after `/f/` in the endpoint URL)
4. Paste it as `formspreeId` in `config.js`

## Hosting

This is a static site — no build step required. Deploy to any static host:

- Drag the project folder into [Netlify](https://netlify.com)
- Push to GitHub and enable [GitHub Pages](https://pages.github.com)
- Upload to any web server

Make sure `config.js` and your asset files are included in the deploy.
