# Paper Submission Site

The submission form is served from:

`https://microstructure.exchange/tme-paper-submission/`

The form collects:

- paper title
- submitting author name and email
- optional co-author names and emails
- uploaded paper file

Uploads are limited to 10MB. PDF, DOC, and DOCX files are accepted.

## Storage

Submission metadata is stored in D1 table `paper_submissions`.

Uploaded files are stored in R2 bucket `tme-review-papers` under:

`submissions/fall2026/`

The Worker assigns the next submission number and stores files as:

`TME_fall2026_submission_<number>.<ext>`

## Export

From the Cloudflare app repo:

```sh
npm run export:submissions -- --output generated/paper-submissions.csv
```

For local testing:

```sh
npm run export:submissions:local
```

The CSV includes `author_names` and `author_emails` as semicolon-separated fields for conflict checks, plus `coauthors_json` for structured follow-up work.

## Admin Endpoint

The protected CSV endpoint is:

`https://microstructure.exchange/tme-paper-submission/api/admin/submissions-export.csv?key=ADMIN_KEY`

The endpoint uses the same `ADMIN_KEY` secret as the existing admin exports.

## Future Cycle Updates

Before launching a new call, update the Worker constants:

- `SUBMISSION_CALL_ID`
- `SUBMISSION_R2_PREFIX`

Then run D1 migrations and deploy.
