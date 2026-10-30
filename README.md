# Decentro Demo Hub

One launcher for all 16 Decentro product demos. Static HTML — no build step, no dependencies.

Open `index.html` to run it locally, or host the folder as-is.

## Structure

```
index.html          the hub shell (homescreen + viewer)
demos/*.html        the 16 prototypes, each self-contained
```

Each demo runs live in an iframe at `demos/<file>` — `index.html` prefixes the bare
filename in `DEMOS` with `DEMO_DIR`, so the folder must ship alongside it.

The viewer gives Demo fill, Restart, prev/next and the Backend trace. On a desktop it
also carries a **Full / iPad / iPhone** switch: Full is the default and simply lets the
flow fill the window, while iPad (834 x 1112) and iPhone (393 x 852) wrap the same
iframe in a real logical-size frame, scaled to fit and never upscaled. On a real phone
or tablet the switch is hidden — the device is already the frame, and the flow answers
to the actual viewport. The choice is remembered in `localStorage`.

Every flow has its own link, e.g. `…/#lrs-checkout`.

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
 file:'my-flow.html', view:'full'}
```

`group` is one of `identity`, `risk`, `payments`, `crossborder`. `file` is the bare
filename — `DEMO_DIR` adds `demos/`. Optionally add a `TRACES['my-flow']` entry to give
the flow a Backend trace. Nothing else needs wiring.

## Two edits made to the bundled prototypes

- **demos/lrs-checkout.html** — embed/SDK mode used to switch on whenever the file was in
  an iframe, which hid the standalone journey inside the hub. It now keys off `?embed=1`
  only; the viewer carries a Standalone ⇄ Embed toggle for that flow.
- **demos/netbanking-connect.html** — added `<!DOCTYPE html>` and `<meta charset="utf-8">`,
  which it lacked. Without them a browser serves it in quirks mode.

## Fixes made on 29 Oct 2026

- `index.html` loaded demos from the repo root while the files live in `demos/`, so
  every flow 404'd when the folder was opened as-is. Paths now go through `DEMO_DIR`.
- `setApi()` called `$('hud')`, an element that does not exist — opening the Backend
  panel threw before it could wire up the trace. It now shifts `#pwrap` instead.
