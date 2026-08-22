# Owner values — applied

All five owner-supplied values have been written into the pages. **No
placeholder tokens remain in any HTML file.** This document is now a record of
what was applied and where, so a future edit can be made consistently.

---

## 1. Values in use

| Value | Applied as | Occurrences |
|---|---|---|
| Legal owner | `LUNQO APPS` | 27 |
| Support email | `support@lunqolabs.com` | 18 |
| Business country | `Tunisia` / `Tunisie` / `تونس` | 6 each |
| Governing law | `Tunisia` / `la Tunisie` / `تونس` | 2 each |
| Effective date | `30 July 2026` / `30 juillet 2026` / `٣٠ يوليو ٢٠٢٦` | 2 each |

### Why country, jurisdiction and date differ per language

The company name and the email address are identical in all three languages —
those are identifiers and are never translated. The other three are rendered in
each page's own language, because a legal document reads as machine output
otherwise, and a Latin-script country name inside Arabic RTL prose is
particularly jarring:

- **Country / jurisdiction** — the approved value was `TUNISIA`. Written in the
  page language and in normal sentence case: *"based in Tunisia"*,
  *"établi en Tunisie"*, *"ومقرها تونس"*. French needs the article for the
  governing-law clause (*"le droit de la Tunisie"*), which is why the French
  jurisdiction value carries `la`.
- **Effective date** — the approved value was `30/07/2026`. Written long-form so
  it cannot be misread as a month/day ordering by a store reviewer:
  *30 July 2026*, *30 juillet 2026*, *٣٠ يوليو ٢٠٢٦*. The Arabic version uses
  Arabic-Indic digits to match the section numbering already used on that page.

If you prefer the literal forms instead, they are single constants — see §4.

### Bidirectional text

On the Arabic pages the company name is emitted as
`<bdi>LUNQO APPS</bdi>`. The `<bdi>` element isolates the Latin run so the
Arabic commas and parentheses beside it stay on the correct side. The email
address is likewise wrapped in `dir="ltr"`. Do not remove either.

---

## 2. Branding, as applied

- **ORBGRID** — the app and primary brand. Plain text, never translated,
  never transliterated into Arabic script.
- **ORBGRIDplus** — the one-time Remove Ads upgrade. Emitted as
  `<span class="ogp">ORBGRID<span>plus</span></span>` so `plus` renders smaller
  and in the accent colour, mirroring the in-game wordmark. On Arabic pages the
  same span additionally carries `dir="ltr"`.
- **LUNQO APPS** — the legal owner. Used *only* in ownership, contact and
  governing-law sentences. It is deliberately never used as the product name.

These three are presented identically across English, French and Arabic.

---

## 3. Still outstanding

| Value | Where | When |
|---|---|---|
| AdMob publisher ID (`pub-…`) | `app-ads.txt` | After creating the AdMob account. The file explains exactly what to do; it is intentionally commented out until then, because a wrong or invented ID is worse than no file. |

Nothing else is pending. The live URLs are already wired into the app — see §5.

---

## 4. Changing a value later

Each value is a single constant per language, so a change is a one-line edit
followed by a regeneration. If you no longer have the generator, a find-and-
replace across `legal-site/` works equally well — the strings are unambiguous.

To confirm nothing was missed afterwards, from inside `legal-site`:

```powershell
Select-String -Path (Get-ChildItem -Recurse -Include *.html) -Pattern '\[[A-Z /]+\]'
```

That must return **no results**.

When changing the Privacy Policy or Terms in a way that affects players, update
the effective date in **all three languages** together.

---

## 5. The site and the app

`orbgrid-mobile/src/screens/RemoveAdsScreen.tsx` now reads:

```ts
export const LEGAL_URLS = {
  terms: 'https://orbgrid.github.io/terms/',
  privacy: 'https://orbgrid.github.io/privacy/',
};
```

These two links are the only place in the entire app that references the legal
site; they are opened from the bottom of the Remove Ads screen. They will
return 404 until the site is actually published, so publish before shipping a
build.

The English pages are canonical. French and Arabic are linked from every page,
so a single URL is enough for the store and for the app — players switch
language themselves.

---

## 6. Before you publish — please read

- These documents were written to match what ORBGRID's code **actually does**:
  no accounts, no servers of ours, local-only save data, and Google, RevenueCat
  and Apple as the only third parties. If the app later gains analytics, cloud
  saves, accounts or another ad network, the Privacy Policy must be updated.
- They are a careful, honest starting point, **not legal advice, and not a
  guarantee of compliance with any particular law**. If LUNQO APPS is a
  registered company, or you are unsure about the Tunisian governing-law
  clause, have a qualified lawyer review them.
- The French and Arabic texts are full translations of the English meaning. A
  native-speaker read-through before launch is worthwhile, especially for the
  Terms. One known regional note: the Arabic pages use `يوليو` for July, the
  pan-Arab standard form, rather than the Tunisian `جويلية`.
