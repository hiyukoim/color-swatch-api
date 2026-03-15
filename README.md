# Node Vibrant Api

A lightweight REST API microservice that extracts color palettes from images using [node-vibrant](https://vibrant.dev/).

## Endpoint

### `POST /api/analyze`

Extracts a color palette from a publicly accessible image URL.

**Request**

```http
POST /api/analyze
Content-Type: application/json

{
  "url": "https://example.com/image.jpg"
}
```

**Response**

Returns six named swatches. Each swatch may be `null` if the color could not be extracted from the image.

```json
{
  "Vibrant": {
    "hex": "#e05a2b",
    "rgb": [224, 90, 43],
    "hsl": [0.0472, 0.7295, 0.5235],
    "population": 342,
    "titleTextColor": "#ffffff",
    "bodyTextColor": "#ffffff"
  },
  "Muted": { ... },
  "DarkVibrant": { ... },
  "DarkMuted": { ... },
  "LightVibrant": { ... },
  "LightMuted": { ... }
}
```

**Swatch properties**

| Property | Type | Description |
|---|---|---|
| `hex` | string | Hex color code (e.g. `#e05a2b`) |
| `rgb` | number[3] | RGB values `[r, g, b]` (0–255) |
| `hsl` | number[3] | HSL values `[h, s, l]` (0–1) |
| `population` | number | Number of pixels this swatch represents |
| `titleTextColor` | string | Recommended text color for titles on this background |
| `bodyTextColor` | string | Recommended text color for body text on this background |

**Swatch names**

| Name | Description |
|---|---|
| `Vibrant` | Most vivid and saturated color |
| `Muted` | Muted, desaturated version of the main color |
| `DarkVibrant` | Dark, vibrant shade |
| `DarkMuted` | Dark, muted shade |
| `LightVibrant` | Light, vibrant shade |
| `LightMuted` | Light, muted shade |

**Error responses**

| Status | Body | Reason |
|---|---|---|
| `400` | `{ "error": "Image URL is required." }` | Request body missing `url` |
| `500` | `{ "error": "Internal server error during color extraction." }` | Image could not be fetched or processed |

---

## Example

```bash
curl -X POST https://example.com/api/analyze \
  -H "Content-Type: application/json" \
  -d '{"url": "https://images.unsplash.com/photo-1501854140801-50d01698950b"}'
```

---

## Running locally

```bash
npm install
npm start
```

The server starts on port `3000` by default. Override with the `PORT` environment variable.

## Environment variables

| Variable | Required | Description |
|---|---|---|
| `PORT` | No | Port to listen on (default: `3000`) |
| `ALLOWED_ORIGINS` | No | Comma-separated list of allowed CORS origins (e.g. `https://example.com,https://sub.example.com`). Leave unset to block all cross-origin requests. |

## Tech stack

- [Node.js](https://nodejs.org/) + [Express](https://expressjs.com/)
- [node-vibrant v4](https://vibrant.dev/)
- Deployed via [Coolify](https://coolify.io/) with [Nixpacks](https://nixpacks.com/)
