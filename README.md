# Handoff: Andrei Shedav — personal portfolio ("Desert" direction)

## Overview
A personal site for a frontend engineer. It's minimal and cinematic, with a desert look inspired by Dune (no film assets or logos). There are two page types:
1. **Home** (`design/index.html`): Hero → About → Chronology (experience timeline) → Pet projects → Equipment (stack) → Contact.
2. **Project detail** (`design/project.html?p=<slug>`): one template, filled with data for each project.

## About the design files
The files in `design/` are **design references built in HTML**. They show the intended look and behavior. They are not production code. Rebuild them in the target stack. Suggested stack: **Next.js (App Router) + TypeScript**, with each animation as a plain `<canvas>` client component driven by `requestAnimationFrame`. No animation library is needed.
To view the references, open `design/index.html` in a browser through a local static server (`npx serve design`). `support.js` is the runtime for the prototype only. Don't port it.

All animation logic is plain JS. It lives in the `class Component` inside the `<script data-dc-script>` block of each file. Port those functions almost 1:1 into hooks or components:
- `drawHero`: dunes, sun, wind particles, worm-sign effect on click
- `drawBg`: floating sand grains behind the content
- `drawNight`: sunset scene in Contact
- `drawGen`: the 4 generative project previews
- the chronology block in `frame()`: horizontal scroll-pinned timeline

## Fidelity
**High fidelity.** Match the colors, type, spacing and motion exactly. All copy for experience and projects is **placeholder**. Replace it with real data.

## Design tokens
**Colors**
- Sand (page bg): `#EAE0CE`
- Ink: `#1F170F`
- Muted text: `#6F6152`
- Body secondary: `#4A3F33`, `#3A3027`
- Line / dim: `#C9BBA3`, `#CDBFA8`
- Accent "spice": `#B4471B`. Alternatives exposed as a tweak: `#8C2F1C`, `#B7792A`, `#2F5D62`. Implement it as the CSS variable `--accent`.
- Dark panel: `#1F170F`. Contact base: `#3A2418`.
- Hero palette "noon": sky `#EFE6D6`→`#E6D2B2`, sun `#FBF6EC`, dunes `#DFC59E` `#D3AA7A` `#C08A58` `#A86F43`, front dune = page sand.
- Hero palette "dusk": sky `#E6C39C`→`#D58A57`, sun `#F7DDB8`, dunes `#C7845A` `#A9643E` `#86492D` `#5E321F`.
- Contact sunset: sky stops `#24170F` 0 / `#4A2A1A` .42 / `#9A4B26` .72 / `#D27A3E` .9. Sun `#F2A865`. Dunes `#7A3A1E` `#4E2615` `#24150D`, each with a rim-light stroke `rgba(246,170,110,.45/.3/.18)`.

**Type** (Google Fonts)
- Display: **Syncopate** 700/400, uppercase, tracked. Name: `clamp(18px,4.2vw,64px)`, letter-spacing .28em. Section titles: `clamp(30px,4.2vw,60px)` .04em. Years: Syncopate 400 `clamp(56px,7.5vw,112px)`.
- Body: **Hanken Grotesk** 300/400/500. About statement: 300 `clamp(28px,3.6vw,52px)`/1.22. Body: 16–19px/1.55–1.65.
- Labels: **Geist Mono** 11–12px, uppercase, letter-spacing .14em.

**Spacing**
- Horizontal page padding: `clamp(20px,4vw,56px)`
- Max width: 1440px
- Section vertical padding: `clamp(120px,14vw,200px)`
- Border radius: almost none (2px)
- No shadows

## Screens

### Home: Hero (100vh, min 600px)
- Full-bleed canvas. Sky gradient, pale sun with radial glow, 4 parallax dune ridges plus a front ridge in page color, so the hero blends into the page.
- Ridge profile: `pow(1-|sin(u)|,2)*.75 + .3*sin(...)`. Wind-driven phase. Mouse parallax per depth (`[.15,.3,.5,.75,1]`).
- On scroll, the back layers sink and the sun sets.
- About 240 wind streaks blow left→right. Fast mouse movement adds gusts.
- **Click** = "worm sign": a Gaussian bulge travels along the front dune for 4.2s, spraying sand.
- Intro: the name animates letter-spacing .7em→.28em with blur-in (2.2s). The sun rises over 2.6s.
- Overlay: centered name and "Frontend Engineer". At the bottom: a live wind readout on the left, "Click the sand · Scroll ↓" on the right.

### About
- A label, then a large statement.
- On scroll, the text fills from dim `#C9BBA3` to ink. Implement it as an inline span with a two-layer `background-clip:text` gradient. Animate `background-size` from 0→100%.
- Attribution line in mono.

### Chronology (scroll-pinned horizontal track)
- Section height = track overflow width + 100vh. A sticky 100vh viewport. The track translates on X with scroll progress.
- Horizon line at 58% height. A spice progress line runs up to a fixed "traveler" marker at 32vw.
- Waypoints are each `min(78vw,560px)` wide:
  - Year above the line. It changes from dim to ink when passed.
  - Dot on the line. It fills with accent when passed.
  - Body below the line: period, role (Syncopate 700), company (accent), description, stack. Opacity .4→1 when passed.
- Counter at the top right, e.g. "02 / 04".
- The track starts with an intro panel and ends with a "20— / Let's talk →" panel.

### Pet projects
- List rows with a top border. Each row: index, name (Syncopate 700, `clamp(20px,2.6vw,38px)`), description, meta.
- On hover, the name shifts 18px. A floating 340×226 preview canvas follows the cursor with lerp .14 and tilts with horizontal velocity.
- Each project has its own generative preview: `flow`, `sphere`, `life`, `wave`.
- On click, an ink curtain slides up (750ms, `cubic-bezier(.7,0,.2,1)`), shows the project name, then navigates to the detail page.

### Equipment
- 4-column auto-fit grid: Languages, Frameworks, Motion & graphics, Tooling.

### Contact (min 100vh)
- Animated sunset canvas: gradient sky, stars in the top 42%, two moons, a setting sun at x 68%, warm wind streaks, 3 rim-lit dune ridges.
- Text: "Say hello", the email (underline in accent, magnetic hover), socials, footer.
- The nav switches to sand color over this section.

### Background (global)
- A fixed canvas behind the content: ~76 small sand grains plus 14 large, faint accent motes.
- They drift and twinkle on their own and fade in after the hero.

### Project detail
- Sticky sand nav: logo, "← All projects".
- Header: "(P/0n) Field experiment", title, summary, meta grid (Year / Role / Stack / Links).
- A large 16:8 live canvas runs the same generator as the home preview. It reacts to the mouse. Click to reseed.
- Overview / Approach in two columns.
- Two screenshot slots at 4:3.
- Dark "Next project →" block, then the footer.
- The curtain wipes in on load and out when you navigate.
- Data lives in the `PROJECTS` array (slug, n, title, gen, year, role, stack, summary, overview, approach). Move it to a content file or MDX.

## Interactions summary
- Custom cursor: 5px accent dot plus a 30px ring with `mix-blend-mode:difference`. The ring grows to 58px over links and hides over project rows. Only on `(pointer:fine)`.
- Reveal on scroll: translateY 40px + fade, 1.2s `cubic-bezier(.2,.7,.1,1)`, via IntersectionObserver.
- Magnetic email button.
- Tweakable in the prototype: hero light (noon/dusk), wind strength (0–3), accent, custom cursor.

## Implementation notes
- Respect `prefers-reduced-motion`: freeze the canvases and remove the curtain and pinning.
- Pause each canvas when it's offscreen. Cap DPR at 2.
- On mobile, hover previews are disabled. The chronology still works with vertical scroll.

## Files
- `design/index.html`: home page
- `design/project.html`: project detail template
- `design/image-slot.js`: placeholder image slots (prototype only)
- `design/support.js`: prototype runtime (don't port)
