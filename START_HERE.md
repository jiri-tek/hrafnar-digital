# Hrafnar Digital — project package

Everything to build the site, ready for Claude Code in your terminal.

## Files

- **BRIEF.md** — the full build brief. THIS is what you give Claude Code.
  It contains brand DNA, tone of voice, visual system, site structure,
  copy rules, pricing, and explicit do's/don'ts.
- **index.html** — a working reference build (open it in a browser to
  see the intended direction). Single file, no build step.
- **README.md** — what to replace before going live.
- **assets/mark.svg** — the custom Hrafnar Digital logo (inline SVG).
- **assets/founder.png** — founder photo.
- **assets/hrafnar-guides-REFERENCE.png** — the *Guides* logo, for brand
  family reference only. Do NOT reuse it as-is for Digital.
- **assets/fonts/** — drop licensed Gascogne Serial + Sarvatrik here.

## Use with Claude Code (terminal)

1. Unzip this folder and `cd` into it.
2. Run `claude` (Claude Code) in that directory.
3. Prompt suggestion:

   > Read BRIEF.md. Build the Hrafnar Digital site exactly to that brief.
   > Use index.html as a visual reference for direction, but produce a
   > clean, well-structured implementation. Keep assets/mark.svg as the
   > logo and assets/founder.png in the founder section. Output a static
   > site I can deploy with no backend.

4. Iterate from there ("make the hero darker", "add an FAQ section",
   "add an Icelandic language toggle", etc.).

## Run the reference build right now

Either just double-click `index.html`, or:

```
python3 -m http.server 8000
# open http://localhost:8000
```

## Deploy (when ready)

Static site — drag the folder onto Netlify, or `vercel`, or push to
GitHub Pages. Nothing else needed.
