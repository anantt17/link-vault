# 🔒 Vault — Private Cloud Link Archive

A private, beautiful, mobile-first bookmark vault with instant Supabase cross-device cloud sync and optional Master PIN protection.

---

## 🚀 Quick Start (Local)

You can run this immediately without deploying anywhere:
1. Double-click `index.html` or open it in any web browser.
2. It works out-of-the-box in **Local Mode** (saved in browser storage).
3. Connect your free Supabase database whenever you are ready for cross-device phone sync.

---

## ☁️ Cross-Device Phone Sync Setup (2 Minutes, Free)

To sync links between your PC and your phone:

### Step 1: Create a Free Supabase Project
1. Go to [supabase.com](https://supabase.com) and create a free account (no credit card required).
2. Click **New Project** and give it a name (e.g. `my-vault`).

### Step 2: Create the `links` Table
1. In your Supabase Dashboard, click on **SQL Editor** in the left sidebar.
2. Paste the following SQL script and click **Run**:

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

-- Enable Row Level Security and allow access with your anon key:
alter table public.links enable row level security;
create policy "Allow access" on public.links for all using (true) with check (true);
```

### Step 3: Connect in Vault Settings
1. In Supabase, go to **Project Settings** (gear icon) -> **API**.
2. Copy your **Project URL** and **anon (public)** key.
3. Open Vault, click **⚙️ Settings**, paste both into the fields, and click **Connect & Sync**.
4. That's it! Your links will now automatically sync across any device you open Vault on.

---

## 📱 Hosting & Accessing from your Phone ($0 Forever)

To access it on your phone anytime on 5G or Wi-Fi:

### Recommended: Vercel or Cloudflare Pages (Free)
1. Push this folder to a **private GitHub repository**:
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   ```
2. Go to [vercel.com](https://vercel.com) or [pages.cloudflare.com](https://pages.cloudflare.com).
3. Import your private repository.
4. Click **Deploy**. In 20 seconds, you get an automated HTTPS URL (e.g. `https://vault-anant.vercel.app`).
5. Open that URL on your phone!

---

## 📲 Install as an App on Phone (PWA)

### On iPhone (Safari):
1. Open your Vault URL in **Safari**.
2. Tap the **Share button** (the square with the arrow pointing up).
3. Scroll down and tap **Add to Home Screen**.
4. Vault will appear on your phone home screen with its custom gold vault icon and launch full-screen without any browser bars!

### On Android (Chrome):
1. Open your Vault URL in **Chrome**.
2. Tap the **three dots (⋮)** menu in the top right.
3. Tap **Add to Home screen** or **Install app**.

---

## 🔒 Master PIN Protection

- In Vault, open **⚙️ Settings** and set a 4-to-6 digit PIN.
- Your vault will automatically lock, requiring your PIN to view or add links.
