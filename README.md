# The Microstructure Exchange

Public website and Cloudflare Worker app for The Microstructure Exchange.

## Contents

- `public/`: static website pages, calendar file, favicon, submission page, and assessment page assets.
- `src/worker.js`: Cloudflare Worker routes for paper submission, reviewer assessment, and exports.
- `migrations/`: D1 database migrations.
- `scripts/`: utility scripts for assessment/submission exports and data generation.
- `docs/`: workflow and design notes.

Submitted papers, reviewer exports, generated token lists, and private review artifacts should not be committed to this repository. Paper files belong in Cloudflare R2 or public author/SSRN/journal links.

## Local Preview

The public website can be previewed by serving the `public/` folder locally.

## Deployment

Cloudflare should deploy from this repository when ready. The repository push alone does not connect or change the `microstructure.exchange` domain.
