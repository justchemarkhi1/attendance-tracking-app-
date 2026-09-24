# Attendance Tracking App

A simple, single-page attendance check-in app for the PACK ONE — Launch 2026 event.

## Features

- Lists all pre-registered attendees (company + name), loaded from Supabase.
- One-tap check-in / undo check-in, synced live across every device open on the page.
- Search by name or company, and filter by All / Pending / Checked in.
- "Add walk-in" for people who show up without a prior registration — they're added to the backend and checked in immediately.
- Live counts: registered, checked in, remaining.

## Stack

Plain HTML/CSS/JS (no build step) + [Supabase](https://supabase.com) (Postgres backend, realtime updates) via the `@supabase/supabase-js` CDN module.

## Running it

Just open `index.html` in a browser, or serve the folder with any static file server:

```bash
npx serve .
```

## Backend

Data lives in a single `attendance_people` table:

| column | type | notes |
|---|---|---|
| `id` | uuid | primary key |
| `company` | text | nullable |
| `name` | text | required |
| `checked_in` | boolean | default false |
| `checked_in_at` | timestamptz | set on check-in |
| `is_walk_in` | boolean | true for people added on the day |
| `created_at` | timestamptz | default now() |

Row Level Security is enabled with public (anon) read/insert/update policies, since this is meant to be used as an open check-in kiosk at the event with no login. If you need to lock it down later, swap the anon policies for authenticated ones.
