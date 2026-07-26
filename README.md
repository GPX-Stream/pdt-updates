# pdt-updates

Public version marker for **PDT Setup** (GPX Stream device-setup tool).

This repository exists for exactly one reason: to serve `latest.json` from a
stable URL, with no credential, and a short cache lifetime.

```
https://raw.githubusercontent.com/GPX-Stream/pdt-updates/main/latest.json
```

Deployed copies of PDT Setup read that file to learn whether a newer release
exists. Release *contents* live in the private `GPX-Stream/pdt-setup-py`
repository and are fetched with a baked read-only token; this marker is the
credential-free signal that a check should happen at all — so it must stay
readable even if that token stops working.

It contains a version number, a date, a one-line summary, and a link. Nothing
else, and nothing sensitive.

## Do not hand-edit

`latest.json` is written and pushed by `scripts/publish_release.py` in the
pdt-setup-py repo, as part of cutting a release. Editing it by hand will be
overwritten by the next release, and a wrong value here tells every deployed
copy the wrong thing.

## Why not a CDN asset

It previously lived on the Shopify media CDN, which serves assets with
`cache-control: max-age=31557600` (one year) and versions re-uploads behind a
`?v=` token — so the URL baked into shipped bundles could never be updated in
place. Every deployed copy kept reading the first value ever published and
reported itself up to date forever. See GPX-Stream/pdt-setup-py#113.

`raw.githubusercontent.com` serves `max-age=300` and purges on push.
