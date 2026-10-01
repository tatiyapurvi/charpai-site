# Deploying charpai.co.in

Host: GitHub Pages · Repo: `tatiyapurvi/charpai-site` · Branch: `main` (root)

`CNAME` holds the custom domain and `.nojekyll` stops Jekyll processing.
Both must stay in the repo root or the domain unbinds on the next deploy.

## Publish an update

    git add -A && git commit -m "..." && git push

Live in 30-60 seconds.

## /sales (Charpai Sales app)

`sales/index.html` is **built, not hand-edited**. Source and build steps live in
`~/Documents/Charpai ERP/measurement-sheet-backup/curtain-calculator/` (read its HANDOVER.md §2a):
`python3 build.py && python3 app/build_app.py` writes `sales/index.html` here; then commit just that file.
Logins and data are in Firebase (project `charpai-sales`); the security is in its rules, so this public repo
holds no secrets. Never copy `app/export/` or `charpai-import.json` here: they contain costs.

## DNS at GoDaddy (one time)

My Products > charpai.co.in > DNS > Manage Zones.

Delete GoDaddy's parked `A @` record and the `CNAME www` that points to
their parking page, then add:

| Type  | Name | Value                  | TTL     |
|-------|------|------------------------|---------|
| A     | @    | 185.199.108.153        | 600 sec |
| A     | @    | 185.199.109.153        | 600 sec |
| A     | @    | 185.199.110.153        | 600 sec |
| A     | @    | 185.199.111.153        | 600 sec |
| CNAME | www  | tatiyapurvi.github.io  | 600 sec |

All four A records are required — they are GitHub's anycast edge, not
alternatives. Verify them against
https://docs.github.com/pages/configuring-a-custom-domain-for-your-github-pages-site
before pasting, in case GitHub rotates the set.

Then: repo Settings > Pages > wait for the domain check to pass >
tick **Enforce HTTPS** (certificate issues within ~15 min of DNS resolving).

## Check propagation

    dig +short charpai.co.in
    curl -sI https://charpai.co.in | head -1
