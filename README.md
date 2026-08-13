# cirrus-site

The public website for **Cirrus**, an iOS health & recovery app. Two static pages, no build step,
no dependencies, no JavaScript.

| File | Serves | Why it exists |
|---|---|---|
| `index.html` | `/` | Landing page, and the link target Washington's MHMD calls a "homepage" if that ever applies |
| `privacy.html` | `/privacy` | The privacy policy. **Required** — App Store Guideline 5.1.1(i) needs a reachable policy URL in App Store Connect *and* an in-app link |
| `_headers` | — | Security headers applied by Cloudflare Pages |

## Why this repo is separate from the app

The app lives in a **private** repo (`Ramy9114/CirrusApp`) that also holds the compliance register,
backend planning and product strategy. Connecting that repo to a hosting provider would grant it
read access to all of it in order to serve two HTML files. Splitting the site out keeps the
deployment surface to exactly what is meant to be public.

**Consequence to respect: this repo is the single source of truth for the policy text.** Do not keep
a second copy in the app repo — a legal document that exists in two places will drift, and the copy
users actually read is this one.

## Deploying

Cloudflare Pages, connected to this repo. Auto-deploys on every push to `main`.

**Build settings** (all defaults except the framework):

| Setting | Value |
|---|---|
| Framework preset | **None** |
| Build command | *(leave empty)* |
| Build output directory | `/` |
| Root directory | `/` |

There is no build step. Cloudflare serves these files as-is.

### Clean URLs

Cloudflare Pages serves `privacy.html` at `/privacy` automatically. `index.html`'s link points at
`/privacy` for that reason — do not "fix" it to `/privacy.html`.

## Before this can go live

Two placeholders in `privacy.html` are marked with `⚠️ TO FILL IN` and render as dashed orange
boxes, so they cannot be missed in a browser:

1. **Controller identity** — who legally publishes Cirrus (an individual, or a registered entity).
2. **Privacy contact address** — a working email for data-subject requests. It will be publicly
   visible and indexed, so a dedicated address is preferable to a personal one.

## Still owed

- **A French version.** Loi Toubon (loi n° 94-665, art. 2) requires French for goods and services
  offered in France, and France is the home market. The English text is the source; the French
  translation is not yet written.
- **The in-app link.** Guideline 5.1.1(i) requires the policy be reachable *inside* the app, not
  only from App Store Connect. That lands in the app repo once this site has a stable URL.

## A note on changing the URL

App Store Connect's privacy policy URL is **version-bound**: changing it after submission requires
shipping a new app build. The content at the URL can be edited freely at any time — the *address*
is what is expensive to move. That is the whole argument for putting a custom domain in front of
this before submitting, rather than relying on the `.pages.dev` address.
