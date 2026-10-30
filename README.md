# Decentro Demo Hub

One launcher for all 14 Decentro product demos. Static HTML — no build step, no dependencies.

Open `index.html` to run it locally, or host the folder as-is.

## Structure

```
index.html          the hub shell (homescreen + viewer)
demos/*.html        the 14 prototypes, each self-contained
```

Each demo runs live in an iframe. The viewer gives Web / Tablet / Phone frames,
Restart, prev/next, and open-in-new-tab. Every flow has its own link, e.g.
`…/#lrs-checkout`.

Keyboard: `/` search · `Esc` back to the homescreen · `←` `→` between flows.

## Hosting on GitHub Pages

1. Create a repository and push this folder to it (`index.html` must sit at the repo root).
2. Repo → **Settings** → **Pages** → Source: **Deploy from a branch** → Branch: `main`, folder: `/ (root)` → **Save**.
3. The site goes live at `https://<user-or-org>.github.io/<repo>/` in a minute or two.

`.nojekyll` is included so GitHub Pages serves every file untouched.

> These are internal product demos. A public repo makes them public — use a private
> repo if that matters (Pages on private repos needs a paid GitHub plan).

## Adding a demo

Drop the file in `demos/`, then add one entry to the `DEMOS` array in `index.html`:

```js
{slug:'my-flow', group:'payments', tag:'Product', surface:'Web',
 name:'My Flow', meta:'4 screens · detail',
 desc:'One line on what it shows.',
 file:'demos/my-flow.html', view:'full'}
```

`group` is one of `identity`, `risk`, `payments`, `crossborder`. `view` is the default
frame: `full` or `phone`. Nothing else needs wiring.

## Two edits made to the bundled prototypes

- **demos/lrs-checkout.html** — embed/SDK mode used to switch on whenever the file was in
  an iframe, which hid the standalone journey inside the hub. It now keys off `?embed=1`
  only; the viewer carries a Standalone ⇄ Embed toggle for that flow.
- **demos/netbanking-connect.html** — added `<!DOCTYPE html>` and `<meta charset="utf-8">`,
  which it lacked. Without them a browser serves it in quirks mode.
