# Nebula Content And Image Generation Service

## Summary

Nebula is the YourBazar content/media generation service. It has been added as a submodule at `services/nebula` from `https://github.com/YourBazar/nebula`.

Current repository evidence shows Nebula as a small Flask service that generates initials-based images from text and uploads the generated image to a storage provider.

## Current Implementation

Observed files:

- `services/nebula/app/app.py`
- `services/nebula/app/image_processor.py`
- `services/nebula/app/common/constants.py`
- `services/nebula/requirements.txt`
- `services/nebula/run.sh`
- `services/nebula/README.md`

Observed routes:

```http
GET /
POST /generate-image
```

`POST /generate-image` expects:

```json
{
  "text": "Business Name"
}
```

Observed response:

```json
{
  "image_url": "https://..."
}
```

## Intended Product Boundary

Nebula should own generated media and content assets for YourBazar, including:

- business logo placeholders,
- user/profile initials images,
- provider/service category visuals,
- opportunity card images,
- generated marketing/content snippets if added later,
- and future AI-assisted content/image generation workflows.

Nebula should not own:

- user authentication,
- marketplace role persistence,
- business CRUD,
- payments,
- Cygnus UI rendering,
- or Centaurus core domain records.

## Integration Direction

### Cygnus

Cygnus can eventually call Centaurus for media generation workflows instead of calling Nebula directly from the browser. This keeps provider keys and generation settings out of frontend code.

### Centaurus

Centaurus currently has image generation/upload logic inside `app/scripts/ImageProcessor.py`. That responsibility should move to Nebula over time.

Suggested backend integration:

```text
Centaurus -> Nebula -> storage provider
```

Centaurus should call Nebula for generated image URLs when creating users, businesses, providers, and future marketplace opportunities.

### Dockyard

Dockyard should eventually run Nebula as a local service alongside Centaurus and Cygnus.

### Orion

Orion should eventually include Nebula in combined status, health, and integration checks.

## Environment Needs

Observed storage integrations:

- ImgBB through `IMGBB_API_KEY`
- MinIO through `MINIO_URL`, `MINIO_USERNAME`, `MINIO_PASSWORD`, and `MINIO_BUCKET`

Secrets must not be committed.

## Risks

- Nebula currently prints errors directly instead of using structured logging.
- The route is unauthenticated.
- Generated files are temporary but should be protected from cleanup failures.
- Direct browser use would expose service behavior without Centaurus authorization.
- The image style is initials-based only; real AI image generation is not yet implemented.

## Recommended Next Tasks

1. Add `local.env.example` to Nebula.
2. Add `test.sh`, `build.sh`, `rinstall.sh`, `runinstall.sh`, and `refreshinstall.sh` using the current script conventions.
3. Add unit tests for initials generation and missing-text validation.
4. Add a health endpoint.
5. Add structured logging.
6. Add Dockyard service wiring.
7. Add Orion registry entry.
8. Replace Centaurus direct `ImageProcessor` usage with a Nebula client after Nebula has stable local/dev deployment.
