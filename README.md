# CES LinkedIn Media

Public, stable image URLs for the Clinical Equipment Service LinkedIn page (Buffer channel `clinical-equipment-service`).

## Why this repo exists

Buffer downloads the media from the URL at publish time. If the URL is not publicly reachable, the post fails with:

> Please update the media URL to be publicly accessible before retrying the post.
> (Media URL is not publicly accessible — HTTP 403)

On 2026-10-19, 2026-10-20 and (scheduled) 2026-10-22, three CES posts failed for exactly this reason.

## Rule: never use these as Buffer media URLs

| Source | Why it breaks |
|---|---|
| `scontent.*.fna.fbcdn.net` (Facebook CDN) | The URL carries an expiring token (`oe=` param). It 403s once it expires and the image cannot be recovered. |
| `files.catbox.moe` / any catbox link | Unreliable host — the whole domain became unreachable (connection timeout) on 2026-10-21. |
| Anything behind a login / private bucket | Not publicly reachable from Buffer's servers. |

## What to use instead

GitHub Releases, exactly like this repo. Stable, public, no expiring token.

```
https://github.com/Rgarcial444/ces-linkedin-media/releases/download/v1/<archivo>
```

Upload a new version by creating a new tag (`v2`, `v3`, ...) — **never overwrite an existing tag**, because posts already in Buffer still point to the old URL.

```bash
gh release create v2 <archivos> --title "LinkedIn media v2" --notes "..."
```

Verify before handing the URL to Buffer:

```bash
curl -sL -o /dev/null -w "%{http_code}\n" "https://github.com/.../releases/download/v1/archivo.png"
```

Must return `200`.

## Assets in v1

| File | Use |
|---|---|
| `carousel_slide_1.png` … `carousel_slide_6.png` | Calibration carousel (1920x1080, LinkedIn 16:9) |
| `post-downtime-monitor.jpg` | Post: "Equipment downtime has a ripple effect…" |
| `post-technician.jpg` | Post: "Behind every reliable medical device is a technician…" |

## Notes

- Carousel slides are generated locally by `linkedin/carousel_calibracion/create_calibration_carousel.py` (PDF → PNG). Resize to 1920x1080 before uploading.
- Buffer's MCP tool exposes only `createPost` / `deletePost` / `createIdea`. There is no `updatePost`, so "editing" a post means deleting it and recreating it.
