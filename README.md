# @genshot/sdk

Official TypeScript / JavaScript client for the [genshot](https://genshot.dev) screenshot generation API.

Create App Store, Google Play, and Chrome Web Store listing images from a store URL or your own screenshots — from Node, edge runtimes, or the browser.

## Install

```bash
npm install @genshot/sdk
```

## Quick start

```ts
import { createGenshotClient } from '@genshot/sdk';

const genshot = createGenshotClient({
  apiKey: process.env.GENSHOT_API_KEY!, // gsk_...
});

const created = await genshot.generations.create({
  version: 2,
  targetStore: 'app_store',
  targetImageType: 'store_screenshot',
  sourceUrl: 'https://apps.apple.com/us/app/example/id123456789',
  prompt: 'Bold headlines, real UI, premium 3D product world',
  imageCount: 4,
});

const finished = await genshot.generations.wait(created.generationId);
for (const image of finished.generatedImages) {
  console.log(image.imageNumber, image.url);
}
```

## Authentication

Create an API key in the [genshot dashboard](https://genshot.dev/signup), then pass it as `apiKey`.

Every generation request sends `Authorization: Bearer gsk_...`.

## API surface

| Method | Description |
|--------|-------------|
| `generations.create(input)` | Queue a generation job |
| `generations.get(id)` | Fetch current status and outputs |
| `generations.list({ limit? })` | List recent jobs |
| `generations.delete(id)` | Soft-delete a job |
| `generations.wait(id, options?)` | Poll until completed / failed |
| `uploads.presign(input)` | Get a signed upload URL for a source screenshot |
| `uploads.put(presign, bytes)` | Upload bytes to the signed URL |

This package talks only to the public REST API (`https://api.genshot.dev`). See the [API docs](https://genshot.dev/api-docs) and [OpenAPI](https://genshot.dev/openapi.json) for the full HTTP contract.

## Configuration

```ts
createGenshotClient({
  apiKey: 'gsk_live_...',
  apiUrl: 'https://api.genshot.dev', // optional override
  fetch: customFetch, // optional (tests / polyfills)
  userAgent: 'my-app/1.0', // optional suffix
});
```

## Errors

Failed responses throw `GenshotApiError` with `status`, `code`, and `message`.

## License

MIT
