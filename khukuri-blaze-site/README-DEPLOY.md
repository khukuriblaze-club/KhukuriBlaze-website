# Khukuri Blaze Cricket Club — deployment guide

This folder is the complete website. It is a **static site**: HTML, CSS,
JavaScript and images, with no server, no database and nothing to maintain.
That is deliberate — it is why it can absorb any amount of traffic, costs
nothing to host, and cannot be hacked through a login or a plugin.

---

## 1. Put it online (10 minutes, free)

**Cloudflare Pages** (recommended) or **Netlify** — both free, both give you
HTTPS, a global CDN and unlimited bandwidth on the free tier.

### Cloudflare Pages
1. Go to `dash.cloudflare.com` → **Workers & Pages** → **Create** → **Pages**
   → **Upload assets**.
2. Drag this whole folder in. Name the project `khukuri-blaze`.
3. Deploy. You get a live address like `khukuri-blaze.pages.dev`.

### Netlify
1. Go to `app.netlify.com/drop`.
2. Drag this whole folder onto the page.
3. Live immediately. `netlify.toml` and `_redirects` are already set up.

---

## 2. Point your domain at it

Buy the domain first — `khukuriblaze.com.au` or `.club` — from any registrar
(VentraIP, Crazy Domains and Cloudflare all sell `.com.au`; a `.com.au` needs
an ABN or registered business name).

Then in your host: **Custom domains → Add domain**, and follow the DNS
instructions it gives you. HTTPS is issued automatically within minutes.

**After the domain is live, update these three files** — they currently say
`khukuriblaze.com.au` as a placeholder:

| File | What to change |
|---|---|
| `index.html` | every `https://khukuriblaze.com.au` (canonical + social tags) |
| `robots.txt` | the `Sitemap:` line |
| `sitemap.xml` | the `<loc>` line |

---

## 3. The forms — already wired, but test them once

The three forms (registration, sponsorship, contact) are already connected to
Web3Forms with your access key, set in `config.js`:

```js
formEndpoint:  'https://api.web3forms.com/submit',
formAccessKey: '5afa2ae3-f707-46fd-b640-e5f39b3dc2cc',
```

Every submission lands in `khukuriblaze@gmail.com`, with the sender's address
set as reply-to so you can just hit reply.

**Send one test from the live site before you announce it.** Delivery could
not be verified from where this was built, so the live page is the only place
that proves it works. Open the contact form, send a message, and check the
club inbox (and spam) a minute later.

If it ever fails, the form says so plainly and offers an "email it instead"
button that opens the visitor's mail app with everything they typed already
filled in — so an enquiry is never silently lost. If the service itself
refuses a submission, its own reason is shown (for example an expired key),
rather than a generic error.

Spam is already handled: a hidden honeypot field and a minimum fill time.

To change where mail goes later, replace the key in `config.js` and re-upload
that one file. Formspree works the same way; the file has both examples.

---

## 4. Links to the official records

Each competition on the Match Centre carries a button through to the record
kept by the association or the scoring app, so the full scorecards live where
they are maintained rather than being copied here:

| Competition | Goes to |
|---|---|
| CCSCA Red Ball 2025/26 | PlayHQ grade page |
| Prithvi Jayanti T10 | CricHeroes tournament |
| KTM Friendship Cup | CricHeroes tournament |
| Expert Aussie Cup | CricHeroes tournament |
| Squad page | CricHeroes team profile |

These sit in the `COMPETITIONS` list near the bottom of `index.html`. Each
entry takes two optional fields:

```js
official:      'https://...',                    // the link
officialLabel: 'Every scorecard on CricHeroes',  // the button text
```

Add those two lines to any future competition and the button appears on its
own. Leave them out and nothing shows.

**Note on the 2025/26 CCSCA season:** the side was registered as *Buddha
Nepalese CC Blue*, so that is the name on PlayHQ. The Match Centre says so in
one line, and the three games against *Buddha Nepalese CC Whites* were against
that club's other team, not against ourselves.

---

## 5. Changing details later

Everything a committee member might need to change lives in **`config.js`** —
club email, phone, Facebook and Instagram links. Edit that one file and
re-upload it; the whole site updates.

Fixtures, players, stats and sponsors live in the data blocks near the bottom
of `index.html` (`COMPETITIONS`, `PLAYERS`, `STATS`, `SPONSORS`). They are
plain lists — adding next season's results means copying a line and changing
the numbers.

---

## 6. What's in the folder

| File | Purpose |
|---|---|
| `index.html` | the entire site |
| `config.js` | club details and form settings — the file you edit |
| `assets/` | crest, photography, player cards, sponsor logos |
| `404.html` | shown for a bad address, bounces home |
| `robots.txt`, `sitemap.xml` | so Google can index the site |
| `site.webmanifest`, icons | adds to a phone home screen properly |
| `social-card.jpg` | the preview image when the link is shared |
| `_headers` | security headers and caching rules |
| `_redirects`, `netlify.toml` | so deep links work on any page |

---

## 7. Going live checklist

- [ ] Uploaded to Cloudflare Pages or Netlify
- [ ] Domain connected, HTTPS green
- [ ] The three placeholder URLs replaced with the real domain
- [ ] A test message sent from the live contact form and received in the inbox
- [ ] Submit the site to Google: `search.google.com/search-console`
- [ ] Share the link on the club Facebook and Instagram

---

## 8. On traffic

A static site on Cloudflare or Netlify is served from a CDN. Ten visitors or
ten thousand makes no difference to it, and there is nothing that can fall
over under load. Images are cached for a year; the page itself is around
195 KB before images, and the 3D ball only downloads when a visitor scrolls
to it.
