# vault

a private bookmark manager that works on your phone and desktop. built this because i was tired of losing links across devices.

no backend to manage. just a single html file + free supabase for sync.



---

## what it does

- save links from any source — twitter, github, youtube, reddit, articles, whatever
- **real-time sync** across all your devices (add on phone, see it on pc instantly)
- filter by source, folder, or tags
- bulk import — paste raw text and it extracts all urls
- optional **PIN lock** so nobody else can open it on your phone
- works offline, installs as a PWA on your home screen

---

## stack

just html, css, and vanilla js. no frameworks. supabase for the database + real-time.
runs as a static file so hosting is free forever.

---

## setup

### 1. get it running locally
just open `index.html` in your browser. works immediately in local mode (no signup needed).

### 2. add cloud sync (optional but recommended)
you need a free [supabase](https://supabase.com) account. no credit card.

create a project, then run this in the SQL editor:

```sql
create table if not exists public.links (
  id text primary key,
  url text not null,
  title text default '',
  folder text default 'Unsorted',
  tags jsonb default '[]'::jsonb,
  notes text default '',
  source text default 'web',
  added bigint default (extract(epoch from now()) * 1000)::bigint,
  updated_at timestamp with time zone default timezone('utc'::text, now())
);

alter table public.links enable row level security;
create policy "Allow access" on public.links for all using (true) with check (true);
```

then go to **Database → Publications → supabase_realtime** and toggle on the `links` table.

then in vault, open **Settings**, paste your supabase project URL + anon key, hit connect.

### 3. deploy so you can open it on your phone
push this repo to github, then deploy on [vercel](https://vercel.com) or [cloudflare pages](https://pages.cloudflare.com) — both are free. you get an https url in about 20 seconds.

open that url on your phone → tap share → **Add to Home Screen** → done.

---

## pin lock

go to settings, set a 4-digit pin. vault locks itself and shows a keypad when you reopen it.

---

## your data

your supabase credentials never touch any server other than supabase's. the app is just a static html file. you own everything.

---

made as a side project because browser bookmarks are a mess

