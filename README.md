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

📬 Need Help?

I can generate:

Full Storage photo upload flow

Drag-and-drop sorting UI

Dedicated RSVP analytics dashboard

GitHub Actions CI/CD

Local Supabase docker environment


Just ask!