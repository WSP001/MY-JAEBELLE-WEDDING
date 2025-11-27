
Here you go — first the merged, production-ready README.md (with Pro Features baked in), then a step-by-step migration / deployment checklist your programming team can follow like a playbook.


---

# 💍 MY JAEBELLE WEDDING WEBSITE

A modern, elegant wedding website for **Morgan JEA (Echols)** & **Shawn Keller**, featuring:

- A **public, guest-facing site** (Vite + React, Netlify, Netlify Forms)
- A **secure admin panel** (Next.js + NextAuth magic links + Supabase)
- **Pro Features** like Supabase Storage photo uploads, drag-and-drop ordering, RSVP analytics, CI/CD, and local Supabase development

---

## 🌸 High-Level Overview

This project is split into two apps:

1. **Public Wedding Website (Vite + React)**  
   - Deployed to: `https://shawn-morgan-wedding.netlify.app`  
   - Shows:
     - Hero section with couple & wedding date
     - “The Big Day” details and story
     - Schedule / timeline of events
     - Wedding party
     - Registry links
     - Photo gallery
     - RSVP form powered by **Netlify Forms**

2. **Admin Panel (Next.js + Supabase + NextAuth)**  
   - Recommended deployment: `https://admin.shawn-morgan-wedding.netlify.app`  
   - Restricted to authorized users via **magic link login**
   - Allows editing:
     - Wedding details (names, date, venue, story, hero image)
     - Events / timeline
     - Wedding party members
     - Registry links
     - Gallery photos (including uploads & ordering)
   - All data is stored in **Supabase PostgreSQL**

---

## 🏗 Architecture

```text
Public Site (Vite + React, Netlify)
  ├─ Static pages + Tailwind styling
  ├─ Netlify Forms for RSVP (HTML form → Netlify backend)
  └─ Optional reads from Supabase (public read-only)

Admin Panel (Next.js, App Router, Netlify)
  ├─ NextAuth (magic link sign-in via Resend)
  ├─ Server-side Supabase client (Service Role)
  ├─ Admin UI (React Hook Form + DnD sorting)
  ├─ CRUD API routes (wedding, events, party, registry, photos)
  └─ RSVP Analytics (via Netlify API, reading Netlify Forms data)

Supabase
  ├─ PostgreSQL tables:
  │  - weddings
  │  - events
  │  - party_members
  │  - registry_links
  │  - photos
  │  - guests (optional future)
  │  - rsvps (optional future)
  ├─ Row-Level Security (RLS) with public read-only
  └─ Storage bucket: wedding-photos (for gallery images)


---

✨ Core Features

Public Site (Vite + React)

Hero Section

Couple names: Morgan JEA (Echols) & Shawn Keller

Wedding date: November 14, 2026

Location: Atlanta, GA

CTA button linking to #rsvp


The Big Day + Our Story

Human-friendly formatted date & time

Venue name & address

“Our Story” copy with supporting photo


Schedule / Timeline

Events like Rehearsal Dinner, Ceremony, Reception, After Party

Day, time, and order presented clearly


Wedding Party

Bridesmaids, groomsmen, maid of honor, best man

Headshots and roles for each member


Registry

Link cards for Amazon, Target, Crate & Barrel, etc.


Gallery

Masonry-style grid of engagement / couple photos

Alt text for accessibility


RSVP Form (Netlify Forms)

Fields: Name, Attending (yes/no), Meal choice, Notes

Submissions:

Stored in Netlify Forms

Accessible via Netlify dashboard and API

Used by Admin RSVP analytics





---

🚀 Admin Panel Features

Magic Link Login

Handled by NextAuth (Auth.js) with Email provider

Emails sent using Resend

Only authorized emails can access /admin


Wedding Editor

Edit couple names, date/time, city, state

Edit venue name & address

Edit hero image URL

Edit story (HTML or text)


Events Editor

Add / edit / delete events

Fields: title, starts_at, ends_at, location, order

Drag-and-drop reordering (DnD) with automatic order updates


Wedding Party Editor

Add / edit / delete party members

Fields: role, name, image_url, bio, order

Drag-and-drop reordering


Registry Editor

Add / edit / delete registry links

Fields: label, url


Photo Gallery Editor

Upload new photos to Supabase Storage

Fields: image_url, alt, order

Drag-and-drop reordering

Inline thumbnail preview


RSVP Analytics Dashboard

Reads from Netlify Forms API

Shows:

Total RSVP count

Yes / No counts

Meal breakdown

Latest submission list (name, attending, meal, notes)



Admin API Routes

All CRUD endpoints live under /api/admin/*

Protected using getServerSession(authOptions)

Use Supabase Service Role key server-side only




---

🏅 Enterprise “Pro Features”

These are the advanced pieces that turn this from a simple site into a robust, maintainable wedding platform:

1. Supabase Storage Photo Upload Flow

Bucket: wedding-photos

Admin can upload files via /api/admin/photos/upload

Backend:

Receives multipart form data

Stores file in wedding-photos at weddings/{slug}/{uuid}.ext

Generates public URL and inserts row into photos

Automatically computes next order index




2. Drag-and-Drop Sorting

Powered by @dnd-kit in the admin UI

Supports reordering for:

Events

Party members

Photos


Writes new order back to Supabase, updating the order column



3. RSVP Analytics Dashboard

Admin page at e.g. /admin/rsvps

Backend calls Netlify API:

Resolves form ID for rsvp

Fetches submissions

Computes stats: total, yes, no, meal counts


UI shows:

KPI cards (Total / Yes / No)

Meal counts list

Data table of latest RSVPs




4. GitHub Actions CI/CD

Optional but recommended

Public site:

On push to main, run tests/build

Deploy via netlify deploy --dir=dist --prod


Admin site:

On push to main, run tests/build

Deploy via netlify deploy --prod --build


Uses GitHub Secrets:

NETLIFY_AUTH_TOKEN

NETLIFY_SITE_ID




5. Local Supabase Docker Environment

Powered by Supabase CLI (supabase start)

Local containers:

Postgres, Studio, etc.


Schema and seed loaded via:

supabase db execute --file supabase/schema.sql

supabase db execute --file supabase/seed.sql


Admin app .env.local points to local URL and keys for offline dev





---

🧩 Repositories & Structure

Depending on your setup, you likely have:

MY-JAEBELLE-WEDDING/ – public Vite wedding site

jaebelle-admin/ – Next.js Admin panel


Public Site (Vite)

Key files:

index.html – root HTML, Tailwind CDN, fonts

src/index.tsx – React entry

src/App.tsx – main layout

src/constants.ts – wedding data (if not fully dynamic)

src/types.ts – shared interfaces

src/components/*.tsx – Hero, AboutSection, RsvpSection, WeddingParty, Schedule, Gallery, Registry, Footer

public/rsvp-thank-you.html – RSVP success page

netlify.toml – Netlify build + SPA + Forms configuration


Admin Panel (Next.js)

Key files:

app/api/auth/[...nextauth]/route.ts – magic link auth

app/admin/page.tsx – main admin dashboard

app/admin/signin/page.tsx – login page

app/admin/rsvps/page.tsx – RSVP analytics dashboard

app/api/admin/wedding/route.ts – update wedding details

app/api/admin/events/route.ts – CRUD events

app/api/admin/party/route.ts – CRUD party members

app/api/admin/registry/route.ts – CRUD registry links

app/api/admin/photos/route.ts – CRUD photos

app/api/admin/photos/upload/route.ts – photo upload → Supabase Storage

app/api/admin/rsvps/route.ts – RSVP analytics via Netlify Forms API

app/admin/ui/AdminClient.tsx – main admin UI (tabs, forms, DnD)

app/admin/ui/SortableList.tsx – drag-and-drop helper

lib/supabaseServer.ts – Supabase admin (Service Role) client

supabase/schema.sql – DB schema

supabase/seed.sql – seed data for Morgan & Shawn



---

🔑 Environment Variables

Public Site (Netlify)

Typical env vars:

VITE_SUPABASE_URL=...         # optional if public site reads from Supabase
VITE_SUPABASE_ANON_KEY=...

(If the public site is fully static, these can be omitted.)

Admin Panel (Netlify)

NEXTAUTH_URL=https://admin.shawn-morgan-wedding.netlify.app
NEXTAUTH_SECRET=your_strong_secret

RESEND_API_KEY=key_xxx
EMAIL_FROM=noreply@jaebelle.com

SUPABASE_URL=https://YOURPROJECT.supabase.co
SUPABASE_ANON_KEY=ey...
SUPABASE_SERVICE_ROLE_KEY=ey...   # server-only

NETLIFY_ACCESS_TOKEN=your_netlify_api_token          # for RSVP analytics
NETLIFY_SITE_ID=public_site_id_for_shawn-morgan-wedding
NETLIFY_RSVP_FORM_NAME=rsvp

For local dev, you can set these in .env.local with local Supabase URLs.


---

🧪 Development

Public (Vite)

npm install
npm run dev
npm run build

Admin (Next.js)

npm install
npm run dev
npm run build


---

🌐 Deployment

Public Site

Hosted on Netlify:

Build command: npm run build

Publish directory: dist


netlify.toml SPA redirect:

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200


Admin Panel

Hosted on Netlify as a separate site:

Build command: npm run build

Publish directory: .next


Netlify automatically recognizes Next.js and configures functions.



---

🗺 Roadmap

Hotel & travel section for guests

“Things to do in Atlanta” section

Guestbook (live guest notes)

Export RSVPs as CSV (Netlify Forms or Supabase)

Mobile-optimized admin view (PWA-style)



---

❤️ Credits

Couple:

Morgan JEA (Echols)

Shawn Keller


Wedding Date: November 14, 2026
Location: Atlanta, GA

Development:

The SeaTrace Programming Team(s)

Netlify, Supabase, and this AI-assisted build pipeline


---

## ✅ Step-by-Step Migration & Deployment Checklist (for your programming team)

This is the **playbook** to go from where you are to the full “Pro” setup, in the smoothest order:

---

### Phase 0 – Baseline: Confirm Existing Setup

1. **Public site**  
   - Confirm `npm run build` passes locally.  
   - Confirm Netlify site `shawn-morgan-wedding.netlify.app` builds & deploys successfully.  
   - Confirm RSVP form appears under **Netlify → Forms** as `rsvp`.  

2. **Admin site (if already scaffolded)**  
   - Confirm Next.js app runs locally (`npm run dev`).  
   - Confirm magic link login via NextAuth + Resend works in dev or staging.  
   - Confirm Supabase `schema.sql` and `seed.sql` have been applied to your hosted Supabase project.

Only when these are stable, move on.

---

### Phase 1 – Supabase Storage Photo Upload Flow

**Goal:** Let Admin upload photos into Supabase Storage and auto-create `photos` records.

1. **Create storage bucket** in Supabase:
   - Name: `wedding-photos`
   - Public: enabled (for now).

2. **Add API route** in the Admin app:
   - `app/api/admin/photos/upload/route.ts`  
   - Use the code we discussed:
     - Accept multipart `file`, `weddingSlug`, `alt`
     - Upload to `wedding-photos`
     - Compute public URL
     - Insert into `photos` with incremented `order`
     - Return newly created `photo`

3. **Add upload UI** to Photos tab:
   - In `AdminClient.tsx`, Photos section:
     - Add `<form encType="multipart/form-data">` with:
       - File input
       - Alt text input
       - Hidden `weddingSlug`
     - On submit:
       - POST to `/api/admin/photos/upload`
       - On success, `photosArray.append(json.photo)` and reset the form.

4. **Test locally**:
   - `npm run dev` (admin)
   - Upload a few photos
   - Confirm they appear:
     - In Supabase Storage
     - In `photos` table
     - In Admin UI

5. **Redeploy admin** to Netlify when stable.

---

### Phase 2 – Drag-and-Drop Sorting (DnD)

**Goal:** Let Admin drag to reorder Events, Party, and Photos; save new order to DB.

1. **Install `@dnd-kit` in Admin app**:
   ```bash
   npm i @dnd-kit/core @dnd-kit/sortable @dnd-kit/modifiers

2. Create SortableList.tsx:

app/admin/ui/SortableList.tsx

Add SortableList and SortableItem component as provided.



3. Wire DnD in Photos tab:

Wrap the .map of photosArray.fields in <SortableList>.

On onReorder(ids):

Compute new order of photos

Update photosForm values and order indexes.


Reuse your existing “Save Photos” to persist the new order.



4. Repeat for Events and Party (if desired):

Events:

Use eventsArray.fields and update order.


Party:

Use partyArray.fields and update order.




5. Test locally:

Drag items in each tab.

Click “Save”.

Verify order column in Supabase tables changes and public site reflects the new order.



6. Redeploy admin to Netlify when DnD is working smoothly.




---

Phase 3 – RSVP Analytics Dashboard

Goal: Give Admin a live view of RSVPs pulled from Netlify Forms.

1. Create Netlify API token & capture site ID:

Go to Netlify → User Settings → Access tokens → New token.

Note: NETLIFY_ACCESS_TOKEN.

Find your public site ID from the site settings (for shawn-morgan-wedding.netlify.app).



2. Add env vars to Admin site (in Netlify Admin site settings):

NETLIFY_ACCESS_TOKEN=your_token
NETLIFY_SITE_ID=public_site_id
NETLIFY_RSVP_FORM_NAME=rsvp


3. Create API route app/api/admin/rsvps/route.ts:

Uses Netlify API to:

List forms for the site

Find the one named rsvp

List submissions for that form

Compute:

Total

Yes / No

Meal breakdown


Return { stats, submissions }.




4. Create admin page app/admin/rsvps/page.tsx:

Use useSWR or fetch client-side:

Call /api/admin/rsvps

Render:

KPI tiles (Total / Yes / No)

Meal stats

Table of submissions (name, attending, meal, notes)





5. Add link in Admin navigation:

Header nav: link to /admin/rsvps.



6. Test locally:

Set env vars in .env.local with your Netlify token & site ID.

Run admin: npm run dev.

Visit /admin/rsvps.

Confirm counts match Netlify Forms dashboard.



7. Redeploy admin to Netlify with env vars set in the UI.




---

Phase 4 – GitHub Actions CI/CD

Goal: Automate build & deploy for both public and admin codebases.

> If you prefer Netlify’s built-in Git integration only, this phase is optional. CI here focuses on adding tests and explicit deploys via Actions.



For the Public Site Repo (MY-JAEBELLE-WEDDING)

1. Add Secrets in GitHub:

NETLIFY_AUTH_TOKEN

NETLIFY_SITE_ID (for the public site)



2. Create workflow file: .github/workflows/deploy-public.yml

Use the YAML from the README (build Vite → netlify deploy --dir=dist --prod).


3. Push to main:

Confirm Actions runs successfully.

Confirm site updates on shawn-morgan-wedding.netlify.app.




For the Admin Repo

1. Add Secrets in GitHub:

NETLIFY_AUTH_TOKEN

NETLIFY_SITE_ID (for admin site)



2. Create workflow file: .github/workflows/deploy-admin.yml

Use the YAML from the README (build Next.js → netlify deploy --prod --build).


3. Push to main:

Confirm Actions run, builds, deploy, and functions all work.





---

Phase 5 – Local Supabase Docker Environment

Goal: Give your devs a full local stack for offline work.

1. Install Supabase CLI:

npm install -g supabase
# or: brew install supabase/tap/supabase


2. Initialize Supabase in Admin repo:

supabase init

This creates a supabase/ config folder (you already have schema.sql and seed.sql alongside it).


3. Start local stack:

supabase start

Note: CLI prints local SUPABASE_URL, anon, service_role values.


4. Load schema & seed into local DB:

supabase db execute --file supabase/schema.sql
supabase db execute --file supabase/seed.sql


5. Point Admin app to local Supabase:

Create .env.local in admin:

SUPABASE_URL=http://localhost:54321
SUPABASE_ANON_KEY=local_anon_key
SUPABASE_SERVICE_ROLE_KEY=local_service_role_key
NEXTAUTH_URL=http://localhost:3000

Run:

npm run dev

Confirm Admin panel reads/writes local DB.



6. Switch back to production Supabase for Netlify:

Keep local env only in .env.local (not in Netlify).

Netlify uses the hosted Supabase env vars you already configured.



EDIT THIS:


# MY-JAEBELLE-WEDDING
WEDDING RSVP APP Production-grade README.md tailored exact setup (Vite public site + Next.js Admin + Supabase + Netlify), and the full architecture built together.

This GitHub repo (either the public site or the admin repo) two separate READMEs (one for public HERE:

📖 README.md

MY JAEBELLE WEDDING WEBSITE

A modern, elegant wedding website for Morgan JEA (Echols) & Shawn Keller, featuring a public guest-facing site and a secure administrator dashboard for editing all wedding content.

Built using Vite + React, Supabase, Next.js, Netlify, and magic-link authentication (NextAuth).


---

🌸 Overview

This project consists of two applications:

1. Public Wedding Website (Vite + React)

📍 Deployed to: https://shawn-morgan-wedding.netlify.app
This site includes:

Elegant hero section

Wedding details & story

Ceremony & reception schedule

Wedding partyV .  ..  lnjggd. m. ?;  ?nm just in. m. m. even fx s jgrjx. Egnz,v v 

Registry links

Photo gallery

RSVP form (handled by Netlify Forms)

Static, fast, CDN-optimized


2. Secure Admin Panel (Next.js + Supabase + NextAuth)

📍 Recommended deploy: https://admin.shawn-morgan-wedding.netlify.app
This private control panel includes:

Magic-link login via NextAuth Email Provider + Resend

Server-side Supabase client (Service Role)

Full CRUD management of:

Wedding details

Events / schedule

Wedding party

Registry links

Photo gallery


Server API routes (protected)

Real-time updates saved back to Supabase



---

🏗 Architecture

┌─────────────────────────────────────────────────────────┐
│                    PUBLIC WEBSITE                       │
│                 Vite + React + Tailwind                 │
│                                                         │
│  • Guests see beautiful wedding landing pages            │
│  • RSVP via Netlify Forms                                │
│  • Static, extremely fast                                │
└──────────────▲──────────────────────────────────────────┘
               │
               │ Fetches public data (read-only)
               ▼
      ┌──────────────────────────┐
      │        SUPABASE          │
      │  PostgreSQL + RLS + API  │
      │  Wedding, Events, Party  │
      │  Registry, Photos        │
      └───────▲──────────────────┘
              │ Writes via Admin
              ▼
┌─────────────────────────────────────────────────────────┐
│                   ADMIN PANEL                           │
│        Next.js (App Router) + NextAuth + Supabase        │
│                                                         │
│  • Magic link login (Resend)                             │
│  • Protected API routes                                  │
│  • CRUD editors using React Hook Form                    │
│  • Upsert + delete via Supabase Service Role             │
└──────────────────────────────────────────────────────────┘


---

🚀 Public Website Setup

📁 Tech Stack

Vite + React 19

TailwindCSS

Netlify Forms

Typescript

Clean component structure


📦 Install

npm install

🏃‍♂️ Run locally

npm run dev

🔨 Build

npm run build

🌐 Deployment (Netlify)

The root contains a netlify.toml:

[build]
  command = "npm run build"
  publish = "dist"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

[build.processing.forms]
  enabled = true

RSVPs are automatically saved under Netlify → Forms.


---

🗄️ Supabase Setup

1. Create a new Supabase project:

https://app.supabase.com/

2. Add environment variables (for Admin panel AND optionally public site):

SUPABASE_URL=...
SUPABASE_ANON_KEY=...
SUPABASE_SERVICE_ROLE_KEY=...

3. Run schema.sql (creates your DB tables)

This includes tables:

weddings

events

party_members

registry_links

photos

guests

rsvps


RLS is enabled with safe public read policies.

4. Run seed.sql

Seeds Morgan & Shawn’s wedding:

Wedding details

Full event schedule

Wedding party

Registry links

Photo gallery



---

🔐 Admin Panel Setup (Next.js)

📁 Tech Stack

Next.js 14 App Router

NextAuth (magic email login)

Resend email provider

Supabase Service Role

React Hook Form + FieldArray

Server Actions + API Routes


📦 Install

npm install

🔒 Authentication

Add to environment:

NEXTAUTH_URL=https://admin.shawn-morgan-wedding.netlify.app
NEXTAUTH_SECRET=yourStrongSecret
RESEND_API_KEY=key_xxxx
EMAIL_FROM=noreply@jaebelle.com

Magic link route

app/api/auth/[...nextauth]/route.ts

Uses Resend to send login links.


---

🔧 Admin Editor Features

Every section uses React Hook Form and Next.js server API routes.

Supported CRUD:

Section	Create	Update	Delete	Upsert	Ordering

Wedding	✔️	✔️	—	✔️	n/a
Events	✔️	✔️	✔️	✔️	✔️
Party Members	✔️	✔️	✔️	✔️	✔️
Registry Links	✔️	✔️	✔️	✔️	n/a
Photos	✔️	✔️	✔️	✔️	✔️


Includes preview thumbnails for photos.


---

📡 API Routes (Admin)

All protected by:

const session = await getServerSession(authOptions)
if (!session?.user?.email) return 401

Included API endpoints:

/api/admin/wedding – update top-level wedding details

/api/admin/events – bulk upsert + delete

/api/admin/party – bulk upsert + delete

/api/admin/registry – bulk upsert + delete

/api/admin/photos – bulk upsert + delete


All use the Supabase Service Role via:

import { supabaseAdmin } from "@/lib/supabaseServer"


---

🌐 Deployment (Admin Panel)

Deploy this separate Next.js app to Netlify:

1. New site → Import repo


2. Set environment variables


3. npm run build


4. Netlify auto-detects Next.js runtime


5. Recommended domain:
https://admin.shawn-morgan-wedding.netlify.app




---

🧵 Editing Data (in Admin Panel)

Tabs included:

Wedding – names, dates, story, hero image

Events – add/edit/remove schedule items

Party – add/edit/remove wedding party

Registry – add or remove registry cards

Photos – add photos with alt text + order


All changes persist to Supabase and appear instantly on the public site.


---

📸 Photo Uploads (coming soon)

The blueprint is ready to add:

Supabase Storage bucket

Pre-signed upload URLs

Drag-and-drop reordering


Generate the full upload production flow to make ready.


---

🎯 Roadmap

Add drag-and-drop ordering for events/party/photos

Add analytics for RSVP count (from Netlify Forms API)

Add custom subdomain for the wedding site (e.g. jaebelle.wedding)

Church directions map embed

Hotel/lodging section

Live guestbook

Mobile admin app (PWA)



---

❤️ Credits

Created for:
✨ Morgan JEA (Echols) & Shawn Keller
Wedding Date: November 14, 2026
City: Atlanta, GA

Developed by:
Roberto → The SeaTrace Programming Team(s)
and this AI-powered build assistant.


---

Bolting on the “pro” features to my Jaebelle.wedding already-beautiful that includes Dev_stack.

Here is what to build:

Public site: Vite + React (Netlify, Netlify Forms for RSVP)

Admin site: Next.js App Router + Supabase + NextAuth (magic link)

Supabase: already has weddings, events, party_members, registry_links, photos, guests, rsvps per our schema


---

1. Full Supabase Storage Photo Upload Flow (Admin)

Goal: In the Admin Photos tab, upload an image file, store it in Supabase Storage, and automatically insert a row into photos with a public URL.

1.1. Supabase Storage setup

In Supabase UI:

1. Go to Storage → Buckets → New bucket

Name: wedding-photos

Public: ✅ (public reads OK; writes are via service role)



2. (Optional) Add RLS/storage policies later if you lock it down.



1.2. Server route to handle uploads

Create app/api/admin/photos/upload/route.ts in your admin Next.js app:

import { NextResponse } from "next/server"
import { getServerSession } from "next-auth"
import { authOptions } from "../../auth/[...nextauth]/route"
import { supabaseAdmin } from "@/lib/supabaseServer"
import { randomUUID } from "crypto"

export async function POST(req: Request) {
  const session = await getServerSession(authOptions)
  if (!session?.user?.email) {
    return NextResponse.json({ error: "Unauthorized" }, { status: 401 })
  }

  const formData = await req.formData()
  const file = formData.get("file") as File | null
  const weddingSlug = formData.get("weddingSlug") as string | null
  const alt = (formData.get("alt") as string | null) ?? ""

  if (!file || !weddingSlug) {
    return NextResponse.json({ error: "Missing file or weddingSlug" }, { status: 400 })
  }

  const supabase = supabaseAdmin()

  // Find wedding id
  const { data: wedding, error: wErr } = await supabase
    .from("weddings")
    .select("id")
    .eq("slug", weddingSlug)
    .single()

  if (wErr || !wedding) {
    return NextResponse.json({ error: "Wedding not found" }, { status: 404 })
  }

  const ext = file.name.split(".").pop() || "jpg"
  const path = `weddings/${weddingSlug}/${randomUUID()}.${ext}`

  // Upload file to storage
  const arrayBuffer = await file.arrayBuffer()
  const buffer = Buffer.from(arrayBuffer)

  const { error: uploadError } = await supabase.storage
    .from("wedding-photos")
    .upload(path, buffer, {
      contentType: file.type || "image/jpeg",
      upsert: false,
    })

  if (uploadError) {
    return NextResponse.json({ error: uploadError.message }, { status: 400 })
  }

  const { data: publicUrlData } = supabase.storage
    .from("wedding-photos")
    .getPublicUrl(path)

  const publicUrl = publicUrlData.publicUrl

  // Insert into photos table
  const { data: maxOrder } = await supabase
    .from("photos")
    .select("order")
    .eq("wedding_id", wedding.id)
    .order("order", { ascending: false })
    .limit(1)
    .maybeSingle()

  const nextOrder = (maxOrder?.order ?? 0) + 1

  const { error: insertError, data: photo } = await supabase
    .from("photos")
    .insert({
      wedding_id: wedding.id,
      image_url: publicUrl,
      alt,
      order: nextOrder,
    })
    .select("*")
    .single()

  if (insertError) {
    return NextResponse.json({ error: insertError.message }, { status: 400 })
  }

  return NextResponse.json({ ok: true, photo })
}

1.3. Admin UI: add upload button in Photos tab

In AdminClient.tsx (admin app), inside the photos tab, add a small upload form:

// inside AdminClient component, under PHOTOS tab JSX:
{tab === "photos" && (
  <>
    <form
      onSubmit={async (e) => {
        e.preventDefault()
        const formEl = e.currentTarget
        const formData = new FormData(formEl)
        formData.set("weddingSlug", props.wedding.slug)

        const res = await fetch("/api/admin/photos/upload", {
          method: "POST",
          body: formData,
        })
        if (!res.ok) {
          alert("Upload failed")
          return
        }
        const json = await res.json()
        // Optionally push new photo into form state:
        photosArray.append(json.photo)
        formEl.reset()
      }}
      className="flex flex-col md:flex-row gap-3 bg-white p-3 mb-4 rounded border"
      encType="multipart/form-data"
    >
      <input type="hidden" name="weddingSlug" value={props.wedding.slug} />
      <input type="file" name="file" accept="image/*" required />
      <input
        type="text"
        name="alt"
        placeholder="Alt text (description)"
        className="border px-2 py-1 rounded flex-1"
      />
      <button className="bg-[#1E473A] text-white px-4 py-2 rounded" type="submit">
        Upload Photo
      </button>
    </form>

    {/* existing form for editing photos (savePhotos) stays below this */}
    {/* ...existing photos form JSX... */}
  </>
)}

That’s a full upload flow: file → Supabase Storage → photos DB row → immediately appears in gallery.


---

2. Drag-and-Drop Sorting UI (Admin)

Let’s implement drag-and-drop ordering for Events and Photos using @dnd-kit.

2.1. Install

In the admin app:

npm i @dnd-kit/core @dnd-kit/sortable @dnd-kit/modifiers

2.2. Sortable list wrapper

Create app/admin/ui/SortableList.tsx:

"use client"
import {
  DndContext,
  closestCenter,
  PointerSensor,
  useSensor,
  useSensors,
} from "@dnd-kit/core"
import {
  arrayMove,
  SortableContext,
  useSortable,
  verticalListSortingStrategy,
} from "@dnd-kit/sortable"
import { CSS } from "@dnd-kit/utilities"

export function SortableItem({ id, children }: { id: string; children: React.ReactNode }) {
  const { attributes, listeners, setNodeRef, transform, transition } = useSortable({ id })
  const style = {
    transform: CSS.Transform.toString(transform),
    transition,
  }
  return (
    <div ref={setNodeRef} style={style} {...attributes} {...listeners}>
      {children}
    </div>
  )
}

export function SortableList({
  items,
  onReorder,
  children,
}: {
  items: { id: string }[]
  onReorder: (idsInNewOrder: string[]) => void
  children: React.ReactNode
}) {
  const sensors = useSensors(useSensor(PointerSensor))
  return (
    <DndContext
      sensors={sensors}
      collisionDetection={closestCenter}
      onDragEnd={(event) => {
        const { active, over } = event
        if (!over || active.id === over.id) return
        const oldIndex = items.findIndex((i) => i.id === active.id)
        const newIndex = items.findIndex((i) => i.id === over.id)
        const sorted = arrayMove(items, oldIndex, newIndex).map((i) => i.id as string)
        onReorder(sorted)
      }}
    >
      <SortableContext items={items} strategy={verticalListSortingStrategy}>
        {children}
      </SortableContext>
    </DndContext>
  )
}

2.3. Use it in Photos tab

Update your Photos tab in AdminClient.tsx:

import { SortableList, SortableItem } from "./SortableList"

// inside photos tab JSX, replace the direct `.map` with:
<SortableList
  items={photosArray.fields as any}
  onReorder={(ids) => {
    const current = photosForm.getValues("photos")
    const idToItem = Object.fromEntries(current.map((p: any) => [p.id, p]))
    const reordered = ids.map((id) => idToItem[id])
    // update order field to match new index
    reordered.forEach((item, index) => {
      item.order = index + 1
    })
    photosForm.reset({ weddingSlug: props.wedding.slug, photos: reordered })
  }}
>
  {photosArray.fields.map((f, idx) => (
    <SortableItem key={f.id} id={f.id as string}>
      <div className="grid md:grid-cols-5 gap-3 border-b pb-3 items-start bg-gray-50 rounded p-2">
        {/* existing inputs... */}
      </div>
    </SortableItem>
  ))}
</SortableList>

You can repeat the same pattern for events and party members: drag to reorder; click “Save” to persist new order values to DB.


---

3. Dedicated RSVP Analytics Dashboard (Admin)

Right now RSVPs are in Netlify Forms, not Supabase. Let’s add an admin page that calls the Netlify API and shows:

Total RSVPs

Yes / No counts

Meal breakdown

List of latest RSVPs


3.1. Env vars (Admin app)

In the admin Netlify site:

NETLIFY_ACCESS_TOKEN=your_netlify_api_token
NETLIFY_SITE_ID=your_public_site_id   # the ID for shawn-morgan-wedding.netlify.app
NETLIFY_RSVP_FORM_NAME=rsvp

Get NETLIFY_SITE_ID and create a personal access token in Netlify (scoped to Forms).

3.2. API route: app/api/admin/rsvps/route.ts

import { NextResponse } from "next/server"
import { getServerSession } from "next-auth"
import { authOptions } from "../auth/[...nextauth]/route"

const NETLIFY_API = "https://api.netlify.com/api/v1"

async function getRsvpFormId(): Promise<string | null> {
  const res = await fetch(
    `${NETLIFY_API}/sites/${process.env.NETLIFY_SITE_ID}/forms`,
    {
      headers: {
        Authorization: `Bearer ${process.env.NETLIFY_ACCESS_TOKEN}`,
      },
      cache: "no-store",
    }
  )
  if (!res.ok) return null
  const forms = (await res.json()) as any[]
  const formName = process.env.NETLIFY_RSVP_FORM_NAME || "rsvp"
  const form = forms.find((f) => f.name === formName)
  return form?.id || null
}

export async function GET() {
  const session = await getServerSession(authOptions)
  if (!session?.user?.email) {
    return NextResponse.json({ error: "Unauthorized" }, { status: 401 })
  }

  const formId = await getRsvpFormId()
  if (!formId) {
    return NextResponse.json({ error: "RSVP form not found" }, { status: 404 })
  }

  const res = await fetch(
    `${NETLIFY_API}/forms/${formId}/submissions`,
    {
      headers: {
        Authorization: `Bearer ${process.env.NETLIFY_ACCESS_TOKEN}`,
      },
      cache: "no-store",
    }
  )

  if (!res.ok) {
    return NextResponse.json({ error: "Failed to fetch RSVPs" }, { status: 500 })
  }

  const submissions = (await res.json()) as any[]

  const stats = {
    total: submissions.length,
    yes: 0,
    no: 0,
    meals: {} as Record<string, number>,
  }

  submissions.forEach((s) => {
    const attending = (s.data.attending || "").toLowerCase()
    if (attending === "yes") stats.yes++
    if (attending === "no") stats.no++

    const meal = (s.data.meal || "").toLowerCase()
    if (meal) {
      stats.meals[meal] = (stats.meals[meal] || 0) + 1
    }
  })

  return NextResponse.json({ stats, submissions })
}

3.3. Admin page: app/admin/rsvps/page.tsx

"use client"
import useSWR from "swr"

const fetcher = (url: string) => fetch(url).then((r) => r.json())

export default function RsvpDashboard() {
  const { data, error, isLoading } = useSWR("/api/admin/rsvps", fetcher)

  if (isLoading) return <p className="p-6">Loading RSVPs…</p>
  if (error || data?.error) return <p className="p-6 text-red-600">Error loading RSVPs</p>

  const { stats, submissions } = data

  return (
    <main className="max-w-5xl mx-auto p-6 grid gap-6">
      <h1 className="text-2xl font-semibold mb-2">RSVP Analytics</h1>

      <section className="grid grid-cols-3 gap-4">
        <div className="bg-white rounded border p-4">
          <div className="text-sm text-gray-500">Total RSVPs</div>
          <div className="text-2xl font-bold">{stats.total}</div>
        </div>
        <div className="bg-white rounded border p-4">
          <div className="text-sm text-gray-500">Yes</div>
          <div className="text-2xl font-bold text-green-700">{stats.yes}</div>
        </div>
        <div className="bg-white rounded border p-4">
          <div className="text-sm text-gray-500">No</div>
          <div className="text-2xl font-bold text-red-700">{stats.no}</div>
        </div>
      </section>

      <section className="bg-white rounded border p-4">
        <h2 className="font-semibold mb-2">Meal Choices</h2>
        <ul className="space-y-1">
          {Object.entries(stats.meals).map(([meal, count]) => (
            <li key={meal}>
              <span className="capitalize">{meal}</span>: <b>{count}</b>
            </li>
          ))}
          {Object.keys(stats.meals).length === 0 && <li>No meal selections yet.</li>}
        </ul>
      </section>

      <section className="bg-white rounded border p-4">
        <h2 className="font-semibold mb-2">Latest RSVPs</h2>
        <div className="max-h-80 overflow-auto text-sm">
          <table className="w-full border-collapse">
            <thead>
              <tr className="border-b">
                <th className="text-left p-2">Name</th>
                <th className="text-left p-2">Attending</th>
                <th className="text-left p-2">Meal</th>
                <th className="text-left p-2">Notes</th>
              </tr>
            </thead>
            <tbody>
              {submissions.map((s: any) => (
                <tr key={s.id} className="border-b">
                  <td className="p-2">{s.data.name}</td>
                  <td className="p-2">{s.data.attending}</td>
                  <td className="p-2">{s.data.meal}</td>
                  <td className="p-2">{s.data.notes}</td>
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      </section>
    </main>
  )
}

Add a nav link to /admin/rsvps in your admin header if you like.


---

4. GitHub Actions CI/CD (Netlify Deploy)

We’ll set up GitHub Actions for:

public Vite site (repo: MY-JAEBELLE-WEDDING)

admin Next.js site (admin repo)


> Note: Netlify already has its own CI, but Actions can run tests and deploy via CLI if you prefer.



4.1. Secrets in GitHub

For each repo (Settings → Secrets and variables → Actions):

Set:

NETLIFY_AUTH_TOKEN – personal access token

NETLIFY_SITE_ID – site ID for this particular site


4.2. Public site workflow (.github/workflows/deploy.yml)

name: Deploy Public Wedding Site

on:
  push:
    branches: [ main ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Use Node
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install deps
        run: npm ci

      - name: Build
        run: npm run build

      - name: Deploy to Netlify
        run: npx netlify deploy --dir=dist --prod --auth=$NETLIFY_AUTH_TOKEN --site=$NETLIFY_SITE_ID
        env:
          NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN }}
          NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID }}

4.3. Admin site workflow

Same idea, but publish is .next and build is Next.js:

name: Deploy Admin Panel

on:
  push:
    branches: [ main ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install deps
        run: npm ci

      - name: Build
        run: npm run build

      - name: Deploy to Netlify
        run: npx netlify deploy --prod --auth=$NETLIFY_AUTH_TOKEN --site=$NETLIFY_SITE_ID --build
        env:
          NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN }}
          NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID }}

(Using --build here lets Netlify run its Next runtime the way it expects.)


---

5. Local Supabase Docker Environment

You can run Supabase locally for dev using the Supabase CLI (which uses Docker under the hood).

5.1. Install Supabase CLI

On your dev machine:

npm install -g supabase
# or brew install supabase/tap/supabase

5.2. Initialize in your admin repo

From the root of the admin project:

supabase init

This creates a supabase/ folder with config.

5.3. Start local stack (Docker)

supabase start

This spins up local Postgres, Studio, etc. The CLI will print:

SUPABASE_URL (usually http://localhost:54321)

Anon and Service Role keys


5.4. Apply schema + seed locally

You already have supabase/schema.sql and supabase/seed.sql.

Run:

supabase db reset
# This runs migrations and seeds
# Or:
supabase db reset --db-url "postgresql://postgres:postgres@localhost:54322/postgres"

Or manually:

supabase db execute --file supabase/schema.sql
supabase db execute --file supabase/seed.sql

5.5. Point your Admin app to local Supabase (for dev)

Add .env.local in the admin app:

SUPABASE_URL=http://localhost:54321
SUPABASE_ANON_KEY=your_local_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_local_service_role_key
NEXTAUTH_URL=http://localhost:3000

Run admin dev server:

npm run dev

Now the admin panel is talking to local Supabase in Docker; your production Netlify site still talks to the hosted Supabase project.


---

Merge all these new features into a single updated README (with a “Pro Features” section), or

generate a step-by-step migration checklist for your dev so they can implement these in the smoothest order (upload → DnD → RSVP dashboard → CI → local Supabase).

