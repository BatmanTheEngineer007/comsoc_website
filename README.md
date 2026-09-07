# IEEE ComSoc ENIG — Student Branch Chapter Website

A React + Vite + Tailwind site for the IEEE Communications Society ENIG Student Branch Chapter.

## Project structure

```
src/
  data.js                 ← all editable content: nav links, board, events, activities, projects, news, stats
  lib/theme.js             ← fonts + shared text style objects (heading / body / mono / display)
  components/
    shared.jsx              ← Reveal (scroll animation), NetworkBackground (svg), horizontal-scroll helpers, BlinkingCursor
    Navbar.jsx, Hero.jsx, About.jsx, MissionVision.jsx,
    Board.jsx, Events.jsx, Activities.jsx, Projects.jsx,
    Stats.jsx, News.jsx, IEEEConnection.jsx, CTA.jsx, Footer.jsx,
    IntroSequence.jsx        ← the 5s "connection" loading screen (plays once per browser session)
  App.jsx                  ← assembles all sections
  main.jsx                 ← React entry point
index.html                 ← page title, meta tags, Google Fonts
```

## Updating content

Almost everything you'll want to change day-to-day lives in **`src/data.js`**:

- `NAV_LINKS` — top nav items
- `boardMembers` — name, role, bio per board member (first one in the array is shown as the featured/larger card)
- `events` — id, title, type, date, location, desc, status (`OPEN` / `REGISTER_NOW` / `CLOSED`), interest (0–100)
- `activities` — icon + title (icons come from `lucide-react`, already imported at the top of the file)
- `projects` — title, tags, desc
- `news` — date, category, title, excerpt
- `stats` — value, suffix, label

Just edit the array, save, and both `npm run dev` (locally) and your live deploy (once pushed) will pick it up.

**Things that are currently placeholders and worth swapping in before/soon after launch:**
- Board member photos — currently colored initials circles in `components/Board.jsx` (`BoardCard`). Add an `image` URL field per member in `data.js` and render an `<img>` instead.
- Event/project images — currently icon-based schematic art, not real photos.
- All `href="#"` links (social icons, LinkedIn/email on board cards, GitHub/project links) — search the `components/` folder for `href="#"` and replace with real URLs.
- Footer contact email/address.

## Running locally

```bash
npm install
npm run dev
```

Opens at `http://localhost:5173`.

## Building for production

```bash
npm run build
```

Outputs a static site to `dist/` — this has already been verified to build cleanly.

## Deploying to comsoc.enig.ieee.tn

The simplest, free, zero-maintenance path is **Vercel** or **Netlify** — both auto-build and auto-deploy from a GitHub repo on every push.

### 1. Push this project to GitHub

```bash
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/<your-org>/ieee-comsoc-enig.git
git push -u origin main
```

### 2. Deploy on Vercel

1. Go to vercel.com → "Add New Project" → import the GitHub repo.
2. Framework preset: **Vite** (auto-detected). Build command `npm run build`, output directory `dist` (also auto-detected).
3. Deploy — you'll get a temporary `*.vercel.app` URL to confirm everything looks right.

*(Netlify works the same way: "Add new site" → import from Git → build command `npm run build`, publish directory `dist`.)*

### 3. Point comsoc.enig.ieee.tn at it

`comsoc.enig.ieee.tn` is a subdomain, so whoever controls DNS for `enig.ieee.tn` (or `ieee.tn`) needs to add one record:

- In Vercel: Project → Settings → Domains → add `comsoc.enig.ieee.tn`. Vercel will show you the exact record to create — normally a **CNAME** record:
  ```
  comsoc.enig.ieee.tn   CNAME   cname.vercel-dns.com
  ```
- In Netlify: Site settings → Domain management → add custom domain, then create the CNAME it shows you (typically pointing to `<your-site>.netlify.app`).

Once the DNS record propagates (usually minutes, sometimes a few hours), the platform automatically issues a free HTTPS certificate for the domain — no extra steps needed.

If you don't have DNS access yourself, this is the one step you'll need to hand off to whoever manages the `ieee.tn` / `enig.ieee.tn` domain (likely IEEE Tunisia Section or ENIG's IT admin).

## Handing off maintenance to future boards

Since content is isolated in `src/data.js`, a future board member with no React experience can still update board members/events by editing that one file and opening a pull request — no need to touch styling or layout code. If the chapter wants non-coders to edit content directly (e.g. through a form instead of a code file), the next step would be wiring `data.js` up to a small headless CMS (Sanity, Contentful, or even a Google Sheet via API) — happy to help set that up when it's needed.
