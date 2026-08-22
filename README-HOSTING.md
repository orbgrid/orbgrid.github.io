# Publishing the ORBGRID legal site (free, on GitHub Pages)

This folder is a complete static website. There is no build step, no framework
and no server: the files you see are exactly the files that get served.

**Nothing has been published yet.** Everything below is for you to run when you
are ready. Total cost: nothing. Total time: about 15 minutes.

**The owner values are already filled in** — LUNQO APPS, Tunisia,
`support@lunqolabs.com`, effective 30 July 2026. Nothing in the pages
needs editing before you publish. The only value still outstanding is the AdMob
publisher ID for `app-ads.txt`, which is added later (Step 9).

---

## Step 0 — The one prerequisite

This site is configured as a **GitHub user/organisation site**, which is why the
repository is named `orbgrid.github.io` and why every URL sits at the root.

> **The GitHub account or organisation that owns the repository must literally
> be named `orbgrid`.** That is what produces `https://orbgrid.github.io/`.
> An account named anything else yields a different address, and the URLs
> printed in Step 5 would not match.

Two consequences worth knowing before you start:

- The repository must be **public**. Free-plan GitHub Pages only serves public
  repositories, and Google's crawlers must reach your privacy policy and
  `app-ads.txt` anyway.
- An account gets **one** user/organisation site. Any *other* site you publish
  from this account later becomes a project site at
  `https://orbgrid.github.io/<other-repo>/`. That does not affect this site.

Using an organisation rather than a personal account is worth considering if
LUNQO APPS is a registered entity — ownership then survives any one person
leaving. Either works, both are free, and you can transfer later.

---

## Step 1 — Create the GitHub account (skip if `orbgrid` already exists)

1. Go to <https://github.com/signup>.
2. The **username must be `orbgrid`** — this is the part that becomes your URL.
3. Verify the email address.

*(For an organisation instead: avatar → **Your organizations** → **New
organization** → Free plan → name it `orbgrid`.)*

---

## Step 2 — Create the repository

1. Go to <https://github.com/new>.
2. **Owner:** `orbgrid`
3. **Repository name:** `orbgrid.github.io` — exactly this, including the dot
   and the `.io`. GitHub recognises the name and treats it as the account's
   site.
4. **Public** — required.
5. Leave "Add a README" unticked.
6. **Create repository**.

---

## Step 3 — Upload the files

**The important rule:** upload the *contents* of `legal-site`, **not the folder
itself**. `index.html` must end up at the top level of the repository.

### Option A — Drag and drop (no tools needed)

1. On the empty repository page, click **uploading an existing file**.
2. Open the `legal-site` folder on your computer.
3. Select everything inside it — `index.html`, `app-ads.txt`, `assets`,
   `privacy`, `terms`, `support`, `fr`, `ar` — and drag them into the browser.
4. **`.nojekyll` needs care:** files starting with a dot are hidden by default
   and browsers sometimes skip them. If it did not upload, click
   **Add file → Create new file**, type `.nojekyll` as the name, leave the body
   empty, and commit. It matters: without it GitHub may ignore some paths.
5. Commit message such as `Add legal site`, then **Commit changes**.

The two `.md` files are optional — they are notes for you, not part of the
site. Uploading them is harmless; they are simply never served as pages.

### Option B — Git command line

```bash
cd path/to/orbgrid-mobile/legal-site
git init
git add -A
git commit -m "Add legal site"
git branch -M main
git remote add origin https://github.com/orbgrid/orbgrid.github.io.git
git push -u origin main
```

---

## Step 4 — Turn on GitHub Pages

1. In the repository: **Settings** → **Pages**.
2. Under **Build and deployment**:
   - **Source:** `Deploy from a branch`
   - **Branch:** `main`, folder `/ (root)`
3. **Save**.
4. Wait 1–3 minutes for the first build, then reload the Pages settings screen.
   It will show *"Your site is live at https://orbgrid.github.io/"*.

---

## Step 5 — Check every page

Open each of these and confirm it loads:

| URL | Expected |
|---|---|
| `https://orbgrid.github.io/` | Landing page |
| `https://orbgrid.github.io/privacy/` | Privacy Policy |
| `https://orbgrid.github.io/terms/` | Terms of Service |
| `https://orbgrid.github.io/support/` | Support |
| `https://orbgrid.github.io/app-ads.txt` | Plain text |
| `https://orbgrid.github.io/fr/privacy/` | French |
| `https://orbgrid.github.io/ar/privacy/` | Arabic, right-to-left |

Also click **English / Français / العربية** on a few pages to confirm the
language switch works on the live URLs.

---

## Step 6 — Confirm HTTPS

GitHub Pages issues a certificate automatically for `github.io` addresses.

In **Settings → Pages**, tick **Enforce HTTPS** if it is not already ticked and
not greyed out. Every link must use `https://` — Google Play rejects a privacy
policy served over plain `http://`.

---

## Step 7 — The app already points here

`orbgrid-mobile/src/screens/RemoveAdsScreen.tsx` has been updated to:

```ts
export const LEGAL_URLS = {
  terms: 'https://orbgrid.github.io/terms/',
  privacy: 'https://orbgrid.github.io/privacy/',
};
```

Nothing further to change in the app. These two links are what the Remove Ads
screen opens. **They will 404 until Step 4 is done**, so publish the site before
shipping a build.

---

## Step 8 — Google Play Console

In **Play Console → your app**:

1. **Store presence → Store listing**
   - **Website:** `https://orbgrid.github.io/`
   - **Email:** `support@lunqolabs.com`
2. **Policy → App content → Privacy policy**
   - **Privacy policy URL:** `https://orbgrid.github.io/privacy/`
3. Still under **App content**, complete the **Data safety** form. Use the
   Privacy Policy as your source of truth — the two must agree:

   | Question | Answer supported by the code |
   |---|---|
   | Does your app collect or share user data? | **Yes** — via the advertising and purchase SDKs |
   | Data types | *Device or other IDs* (advertising ID); *App activity* / *App info & performance* as declared by Google Mobile Ads; *Purchase history* (via the billing/RevenueCat flow) |
   | Collected by your app or a third party? | By **third parties** (Google, RevenueCat, Apple). Your own code sends nothing to servers of yours. |
   | Purposes | Advertising, and app functionality (purchase verification and restoring purchases) |
   | Encrypted in transit? | **Yes** |
   | Can users request deletion? | Data on the device is deleted by uninstalling; purchase records sit with the stores/RevenueCat |
   | Aimed at children? | **No** — general audience, not child-directed |

   > Declare advertising and purchase data only once those features are actually
   > switched on in the released build. Until production AdMob IDs and the
   > RevenueCat key are configured, a released build sends nothing at all — see
   > the "Current status" notes in the Privacy Policy.

---

## Step 9 — AdMob

In **AdMob → Settings → Account information** (or per-app settings):

- **Privacy policy URL:** `https://orbgrid.github.io/privacy/`
- **Developer website:** `https://orbgrid.github.io/`
- Once you have your publisher ID, complete `app-ads.txt` following the
  instructions inside that file, commit the change, then use
  **Apps → (app) → App settings → app-ads.txt → Check for updates**.
  Google crawls `https://orbgrid.github.io/app-ads.txt`, so the developer
  website in your Play listing must be `https://orbgrid.github.io/`.

Also set up your **UMP consent message** in
**AdMob → Privacy & messaging → European regulations**. The app already
implements the consent flow, but the *message itself* is created in the AdMob
dashboard and will not appear until you publish it there.

---

## Step 10 — A custom domain (optional, later)

If LUNQO APPS buys a domain such as `orbgrid.app`:

1. **Settings → Pages → Custom domain**, enter the domain, **Save**.
2. At your registrar, add DNS records:
   - Apex (`orbgrid.app`): four `A` records → `185.199.108.153`,
     `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - Or a subdomain (`legal.orbgrid.app`): one `CNAME` → `orbgrid.github.io`
3. Wait for DNS to propagate, then re-tick **Enforce HTTPS**.
4. **Then update** `LEGAL_URLS` in the app, and the URLs in Play Console and
   AdMob, to the new domain. The old `orbgrid.github.io` addresses keep
   redirecting, but store entries should point at the canonical address.

---

## Updating the pages later

Edit the files and commit. GitHub Pages redeploys within a minute or two.

When you change the Privacy Policy or Terms in a way that affects players,
update the effective date at the top of **all three language versions** so they
stay consistent. It currently reads *30 July 2026* / *30 juillet 2026* /
*٣٠ يوليو ٢٠٢٦*.

---

## Troubleshooting

| Symptom | Cause and fix |
|---|---|
| Pages load but look unstyled | `assets/styles.css` was not uploaded, or the structure was flattened. `assets` must sit next to `index.html`. |
| 404 on every page | You uploaded the `legal-site` folder itself. `index.html` must be at the repository root. |
| Site is at `orbgrid.github.io/orbgrid.github.io/` | Same cause as above — the folder was uploaded instead of its contents. |
| 404 on `/privacy/` only | The `privacy` folder is missing its `index.html`. |
| Site does not appear at the root at all | The repository is not named exactly `orbgrid.github.io`, or its owner is not the account named `orbgrid`. |
| `app-ads.txt` downloads instead of displaying | Harmless — Google reads it either way. |
| Arabic page renders left-to-right | The `ar/` files were edited; `<html dir="rtl">` must remain on those pages. |
