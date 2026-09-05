# Fall 2026 Paper Assessment Site

The assessment site is served from:

`https://microstructure.exchange/tme-paper-assessment-2026/`

Reviewers receive private links with a `token` query parameter. The token identifies the reviewer, shows only their assigned papers, gates paper downloads from the R2 bucket, and allows one submission of ratings.

The form collects one 1-5 rating per assigned paper. There is one optional reviewer-level comment box labeled "Any comments for us (none are expected)"; comments are not collected per paper.

## Data Sources

- Reviewer assignments: `generated/long_assignments_cloudflare_ready.csv`
- Mail merge workbook: keep locally or in a private drive; do not commit it to GitHub.
- R2 bucket: `tme-review-papers`
- R2 prefix: `fall2026/`

## Generate Reviewer Links

From the Cloudflare app repo:

```sh
npm run seed:assessment
```

This creates:

- `generated/seed-assessment-fall2026.sql`
- `generated/assessment-reviewer-links-fall2026.csv`

Use `assessment-reviewer-links-fall2026.csv` for the mail merge. If this script is run again, it creates new private tokens, so regenerate and reseed before sending links.

## Apply Database Changes

Local test database:

```sh
npm run db:migrate:local
npx wrangler d1 execute tme-best-paper --local --file generated/seed-assessment-fall2026.sql
```

Production database:

```sh
npm run db:migrate
npx wrangler d1 execute tme-best-paper --remote --file generated/seed-assessment-fall2026.sql
```

## Deploy

```sh
npm run deploy
```

The Worker has routes for both:

- `andreaspark.com/tme-best-paper-vote-2026*`
- `microstructure.exchange/tme-paper-assessment-2026*`

The assessment site also needs the R2 binding named `PAPERS`, pointing to bucket `tme-review-papers`.

## Export Results

Browser/admin endpoint:

`https://microstructure.exchange/tme-paper-assessment-2026/api/admin/assessment-export.csv?key=ADMIN_KEY`

Command line:

```sh
npm run export:assessment
```

For local testing:

```sh
npm run export:assessment:local
```
