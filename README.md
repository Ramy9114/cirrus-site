# cirrus-site

The public website for **Cirrus**, an iOS health & recovery app. Two static pages, no build step,
no dependencies, no JavaScript.

| File | Serves | Why it exists |
|---|---|---|
| `public/index.html` | `/` | Landing page, and the link target Washington's MHMD calls a "homepage" if that ever applies |
| `public/privacy.html` | `/privacy` | The privacy policy. **Required** — App Store Guideline 5.1.1(i) needs a reachable policy URL in App Store Connect *and* an in-app link |
| `public/_headers` | — | Security headers applied at the edge |

## Why this repo is separate from the app

The app lives in a **private** repo (`Ramy9114/CirrusApp`) that also holds the compliance register,
backend planning and product strategy. Connecting that repo to a hosting provider would grant it
read access to all of it in order to serve two HTML files. Splitting the site out keeps the
deployment surface to exactly what is meant to be public.

**Consequence to respect: this repo is the single source of truth for the policy text.** Do not keep
a second copy in the app repo — a legal document that exists in two places will drift, and the copy
users actually read is this one.

## Deploying

Deployed to **Cloudflare Pages**, connected to this repo, auto-deploying on every push to `main`.

**`wrangler.jsonc` is the whole configuration** — no build step, no server-side code. Only
`public/` is published, so adding a file to the repo root can never accidentally put it on the
internet.

**Why Pages and not Workers:** a Workers URL is `<worker>.<account-subdomain>.workers.dev`, and the
account subdomain is derived from the account's email local-part. Serving the privacy policy from
that address leaked the owner's email in the URL of the document promising not to leak things.
Pages URLs are a single label with no account identifier.

| Setting | Value |
|---|---|
| Framework preset | **None** |
| Build command | *(none)* |
| Build output directory | `public` |

### Clean URLs

Cloudflare Pages serves `public/privacy.html` at `/privacy` automatically. `index.html`'s link
points at `/privacy` for that reason — do not "fix" it to `/privacy.html`.

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
