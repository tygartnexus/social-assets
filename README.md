# Tygart Nexus — Social Assets (canonical host)

Public, approved social media assets used by the Tygart Nexus social publishing
pipeline. This **public** repo is the single standard location for every social asset
(reels, videos, images, posters) across all brands and channels (Instagram, Facebook,
and any future socials). Files here are served over HTTPS so Instagram/Facebook can
ingest them.

> Only publish assets here after creative approval. Do **not** store secrets, drafts,
> or private client materials in this repository.

## Why a public host

Instagram's content-publishing API cannot read a file off a local disk — it needs a
public HTTPS URL, and Facebook video ingestion is the same. This repo provides that, at
zero cost, via the jsDelivr CDN.

## Path convention (going forward)

```
<brand>/<type>/<campaign>/<filename>
```

- **brand** — `tygart-nexus` · `afterhours-command` · `pirouette` · `maison-voyageur` · `_shared`
- **type** — `reels` (9:16 video) · `videos` (other video) · `images` (static posts/feed) · `posters` (video thumbnails)
- **campaign** — kebab-case slug, e.g. `2026-06-motion-reels`

> Earlier assets (pre-2026-06-13) used `<brand>/<campaign>/<file>` without the `type`
> layer; those URLs still work and are left in place. New assets use the `type` layer.

## Public URL forms

| Use | URL |
|-----|-----|
| **jsDelivr CDN (preferred — video + images)** | `https://cdn.jsdelivr.net/gh/tygartnexus/social-assets@main/<relpath>` |
| raw.githubusercontent (fallback) | `https://raw.githubusercontent.com/tygartnexus/social-assets/main/<relpath>` |

Use **jsDelivr for video/reels** — it serves correct content-types through a real CDN,
which Meta's media fetcher needs. raw.githubusercontent is fine for images.

## How to add assets

From the main app repo:

```bash
python scripts/social_asset_host.py add \
  --brand afterhours-command --type reels \
  --campaign 2026-06-motion-reels --file path/to/reel.mp4 --push
```

This copies the file to the standard path, prints the public URLs, and (with `--push`)
commits and pushes so the URL goes live within ~minutes (jsDelivr cache warm-up).

## Notes / limits

- Keep individual files well under GitHub's 100 MB limit (our reels are ~2–3 MB).
- This repo is **public** — only put assets cleared for public hosting here.
- For very large or high-volume video later, migrate to object storage (e.g. Supabase
  Storage / Cloudflare R2) and keep this same path convention.
