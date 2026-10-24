# theleoref.app

Static site for The LEO Ref, LLC. Three pages and a content file — no build
step, no dependencies, no JavaScript.

    index.html      Support page (App Store "Support URL")
    privacy.html    Privacy policy (App Store "Privacy Policy URL" — required)
    terms.html      Terms of use (points at Apple's Standard EULA)
    style.css       Shared styling
    content/        Signed content patches the app fetches
    CNAME           Custom domain for GitHub Pages

## Hosting it

1. Create a public repo and push this directory to it.
2. Settings → Pages → Source: `main` branch, `/` root.
3. Wait a minute. It is live at `https://<user>.github.io/<repo>/`.

## Custom domain

Edit `CNAME` to your domain, then at your registrar add:

    A      @    185.199.108.153
    A      @    185.199.109.153
    A      @    185.199.110.153
    A      @    185.199.111.153
    CNAME  www  <user>.github.io

Then Settings → Pages → Custom domain, and tick **Enforce HTTPS** once the
certificate is issued (usually within the hour).

With `.app` that tick is not optional. The whole TLD is on the HSTS preload
list, so browsers refuse plain http outright — until the certificate is issued
and HTTPS is enforced, the site will not load at all. That is a feature here:
the app rejects non-https feeds anyway, and this makes it impossible to
accidentally publish over plain http.

Register the domain to the LLC with WHOIS privacy enabled. Registration is
public by default and will otherwise publish the registrant's name, address and
phone — which would undo the care taken to keep that information out of the app
and out of the App Store listing.

## Where these URLs go

| Where | Which URL |
|---|---|
| App Store Connect → Support URL | `https://theleoref.app/` |
| App Store Connect → Privacy Policy URL | `https://theleoref.app/privacy.html` |
| App description → Terms of Use link | `https://theleoref.app/terms.html` |
| `contentPatchUrl` in `src/config/edition.ts` | `https://theleoref.app/content/patch.json` |

That last one is the reason to own a domain rather than use the
`github.io` address: it is compiled into every shipped binary and installed apps
check it forever. A domain can be repointed; a hostname you do not control
cannot.

## Before publishing

- Replace `theleoref@gmail.com` throughout if you set up a domain mailbox.
- Check the effective dates on `privacy.html` and `terms.html`.
- Have an attorney read `terms.html`. It was drafted, not lawyered.
