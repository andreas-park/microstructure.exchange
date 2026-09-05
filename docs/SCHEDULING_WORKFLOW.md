# TME Scheduling Workflow Draft

This folder contains a front-end draft for the future season scheduling workflow:

- `index.html`: public website preview with a dynamic 2026-27 season section.
- `schedule.html`: private author page for choosing an available presentation date and adding a paper link.
- `admin.html`: organizer/admin draft for generating offered dates, private author links, invitation CSVs, and presentation edits.

The draft uses browser `localStorage` only so the workflow can be tested without a backend. Production should use the existing Cloudflare Worker/D1 setup.

## Recommended Production Model

### Tables

`season_slots`

- `id`
- `season`
- `term`
- `starts_at`
- `is_offered`
- `reserved_token`
- `created_at`
- `updated_at`

`selected_authors`

- `id`
- `season`
- `name`
- `email`
- `token_hash`
- `token_last_four`
- `assigned_slot_id`
- `created_at`
- `updated_at`

`talks`

- `id`
- `season`
- `term`
- `slot_id`
- `presenter_name`
- `presenter_email`
- `paper_title`
- `paper_url`
- `recording_url`
- `status`: `tentative`, `confirmed`, `completed`
- `is_public`
- `notes`
- `created_at`
- `updated_at`

### Date Selection

Authors receive links like:

`/schedule?token=<private-token>`

The page should show only slots where `is_offered = 1` and `reserved_token IS NULL`, plus the author's existing reservation if they already chose a date.

On submit, the backend must reserve the date atomically:

```sql
UPDATE season_slots
SET reserved_token = ?
WHERE id = ?
  AND is_offered = 1
  AND (reserved_token IS NULL OR reserved_token = ?)
```

If the update affects zero rows, another author already took the slot and the page should ask the author to choose again.

### Paper Intake

Default should be a paper URL, not an upload. Most authors will provide SSRN, a journal URL, or a personal-site PDF.

Direct upload can be added later with R2, but it is not required for the first production version.

### Admin

Admin should be protected by either Cloudflare Access or a proper login. A shared secret URL is not enough once recording/paper edits can affect the public site.

Admin capabilities:

- Generate default Tuesday slots for 14 weeks from a start date.
- Toggle which slots are offered.
- Add selected authors and generate private scheduling links.
- Export invitation CSV.
- Edit presenter, title, paper URL, recording URL, status, and public visibility.
- Add YouTube recording links after webinars.

### Public Website

The 2026-27 season section should read from the `talks` table and display public, confirmed/completed talks:

- date
- presenter
- paper title
- paper URL when available
- recording URL when available

Completed talks can later be rolled into the permanent past-talk archive.
