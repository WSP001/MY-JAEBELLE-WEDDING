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

Wedding party

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


I can generate the full upload flow when you're ready.


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

Love this. Let’s bolt on the “pro” features to your already-beautiful wedding stack. I’ll keep everything copy-pasteable for your dev.

I’ll assume:

Public site: Vite + React (Netlify, Netlify Forms for RSVP)

Admin site: Next.js App Router + Supabase + NextAuth (magic link)

Supabase: already has weddings, events, party_members, registry_links, photos, guests, rsvps per our schema


I’ll break it into the 5 things you asked for.


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

