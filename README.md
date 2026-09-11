# Afterwards website

Static pages for the Afterwards iOS app, kept in their own public repository so the
app's source can stay private — GitHub Pages cannot publish from a private repo on a
free plan. Same arrangement as `chalkline-site`.

| Page | Purpose |
|---|---|
| `index.html` | Marketing homepage. Use as the Marketing URL in App Store Connect. |
| `privacy/index.html` | **Privacy Policy URL** for App Store Connect. Required, and App Review checks it resolves. |
| `support/index.html` | **Support URL** for App Store Connect. Required. |

## The URLs must match the app exactly

`Legal.swift` in the app hardcodes these two:

    https://estateadmin.app/privacy
    https://estateadmin.app/support

That is why privacy and support are **directories with an `index.html`** rather than
`privacy.html` and `support.html`. A directory serves at the extensionless path on every
static host; relying on a host to guess `.html` does not. Do not flatten them.

All internal links are relative, so the site also works unchanged at
`<user>.github.io/afterwards-site/` if the domain is not ready.

## Deploying

1. Create a public repo `afterwards-site` and push this directory to it.
2. Settings → Pages → source `main`, folder `/ (root)`.
3. Register `estateadmin.app` and point DNS at GitHub Pages: four `A` records for the
   apex (`185.199.108–111.153`), or an `ALIAS`/`ANAME` to `<user>.github.io`.
4. Settings → Pages → Custom domain → `estateadmin.app`, then tick **Enforce HTTPS**
   once the certificate is issued. `.app` is HSTS-preloaded, so plain HTTP will not
   work at all — the certificate is not optional.
5. Confirm both URLs load before submitting to App Review.

If you decide against the domain, change the two URLs in `Legal.swift` to the
`github.io` ones instead and delete `CNAME`.

## Contact address

Support and privacy enquiries go to `afterwards.app@icloud.com`. **This alias does not
exist yet** — create it on the Apple Account before the pages go live, or change the
address in `privacy/index.html`, `support/index.html` and `index.html` together. Nothing
in the app itself carries the address; Settings → Contact Support opens the support page.

## Keeping the privacy policy true

The policy describes the app as it is today. It will stop being accurate if any of these
change, and each one needs the policy updated and the effective date bumped **before**
that version ships:

- adding analytics, crash reporting, or any third-party SDK
- asking for a permission the app does not currently use, such as the camera or the
  photo library
- sending anything to the content server beyond a request for a file
- storing any new field about the estate or the person who died

The claim that pairs with this is the App Store Connect privacy label, currently
**Data Not Collected**. The two must agree.
