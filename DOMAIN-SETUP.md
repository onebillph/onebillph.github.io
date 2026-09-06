# onebillph.com — setup checklist

Do the steps in order. Steps 1–3 take ~15 minutes; then there's a wait for DNS.

**Use the `onebillph` GitHub account for all of this** — that's the account that owns
`onebillph.github.io`. (The `sherpotpot` account has a copy of the privacy repo; ignore it.)

---

## 1. Put the website files on GitHub

1. Sign in to GitHub as **onebillph**. Open **github.com/onebillph/onebillph.github.io**.
2. **Add file → Upload files**. Drag in all three files from the `onebillph-site` folder:
   - `index.html` (the new home page)
   - `privacy.html` (the privacy policy)
   - `CNAME` (one line: `onebillph.com` — this tells GitHub which domain to serve)
   Files must sit at the repo root, not in a subfolder. Commit to `main`.
   If the repo already has an `index.html`, replacing it is correct.
3. Repo **Settings → Pages**:
   - *Build and deployment → Source*: **Deploy from a branch**, branch **main**, folder **/ (root)**. Save.
   - *Custom domain*: type `onebillph.com` → **Save**. GitHub will show "DNS check in progress" until step 2 propagates.

## 2. Point the domain at GitHub (at your registrar)

Open the DNS settings for onebillph.com and add these records. Delete any
existing A/CNAME "parking" records for `@` and `www` first.

| Type  | Host / Name | Value                | TTL  |
|-------|-------------|----------------------|------|
| A     | @           | 185.199.108.153      | Auto |
| A     | @           | 185.199.109.153      | Auto |
| A     | @           | 185.199.110.153      | Auto |
| A     | @           | 185.199.111.153      | Auto |
| CNAME | www         | onebillph.github.io  | Auto |

Notes by registrar:
- **Spaceship**: Domain → *DNS records* → Advanced DNS. "Host" `@` = the bare domain.
- **Cloudflare**: DNS → Records. Set each record's proxy status to **DNS only (grey cloud)**
  until GitHub has issued the HTTPS certificate (step 3), then you may turn the orange cloud on.
- **Porkbun / Namecheap**: Manage → DNS. Same records; Namecheap writes `@` as `@`.

Propagation usually takes 5–30 minutes, occasionally a few hours.

## 3. Turn on HTTPS

Back in **Settings → Pages** on GitHub, once the DNS check shows a green tick,
tick **Enforce HTTPS**. (If the box is greyed out, wait 10–15 minutes and reload —
GitHub is still issuing the certificate.)

Check in a browser:
- https://onebillph.com/ → new home page
- https://onebillph.com/privacy.html → privacy policy
- https://www.onebillph.com/ → redirects to the above

Optional but recommended: GitHub **account** Settings → Pages → *Add a verified domain*
→ `onebillph.com`. It gives you a TXT record to add at the registrar. This stops anyone
else from ever claiming the domain on GitHub Pages.

## 4. Verify the domain in Google Search Console

This is what fixes the "homepage not registered to you" error. With a real domain you
can now use the stronger **Domain** property type.

1. search.google.com/search-console → property switcher → **Add property**.
2. Choose **Domain** (left box, not URL prefix) → enter `onebillph.com` → Continue.
3. Google gives you a **TXT record** (`google-site-verification=...`). Add it at your
   registrar: Type **TXT**, Host `@`, Value = the string Google gave you.
4. Wait a few minutes, click **Verify**. Settings → Ownership verification should show
   "You are a verified owner".
   Sign in to Search Console with the **same Google account** that owns the OneBill PH
   Cloud project.

## 5. Update Google Cloud Console (Google Auth Platform → Branding)

| Field                           | New value                          |
|---------------------------------|------------------------------------|
| Application home page           | `https://onebillph.com/`           |
| Application privacy policy link | `https://onebillph.com/privacy.html` |
| Authorised domain 1             | `onebillph.com` (replace `onebillph.github.io`) |

Save. Then **Verify Branding** (or View issues → "I have fixed the issues" → Proceed).
The automated check normally finishes in minutes and should now pass. Once the status
says **Ready to publish**, click **Publish branding**.

Then **Verification centre** → submit the data-access (Gmail scope) request. The demo
video link and justification are already filled in from earlier.

## 6. Update the other places that point at the old URL

- **Play Console** → Store presence → *App content* → **Privacy policy** →
  `https://onebillph.com/privacy.html`.
- **Play Console** → Store listing → Contact details → Website → `https://onebillph.com/`.
- The **app** now links to `https://onebillph.com/privacy.html` (changed in the source
  for the next build). Nothing to do until you next build.
- Leave the old `onebill-privacy` repo alone — its URL keeps working, so old builds
  and any links you've already shared don't break.

## 7. Home page link to swap later

The "Get it on Google Play" button on the home page points to
`https://play.google.com/store/apps/details?id=ph.onebill.app`. That link goes live the
moment your production rollout is live. If Google's reviewer visits before then, the
link shows Play's "not found" page — harmless, but if you'd rather, temporarily change
the button text to "Coming soon on Google Play" and swap it back after rollout.
