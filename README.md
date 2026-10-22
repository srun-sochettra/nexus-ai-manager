# NexusAI Manager

**Product and employee record management with an AI-assisted interface.**

A Next.js application combining searchable management tables, CRUD forms, dashboard charts and AI tools. The interface already uses the **NexusAI** name; this documentation aligns the project title with that branding.

![NexusAI interface showcase](https://github.com/user-attachments/assets/bfba03c5-768a-4f3a-8d3b-eff8a50e7bee)

This repository contains the Next.js interface and AI route handlers. The employee/product backend is a separate dependency, not bundled here. Treat the current project as a development/demo application, not a verified enterprise-ready system.

## What is included

- **Management tables:** employee and product listings with client-side search, sorting, filters and pagination.
- **CRUD interfaces:** create, look up, update and delete records through an external REST API.
- **Dashboard:** charts calculated from loaded records, plus an optional Gemini-generated summary.
- **AI assistant and search:** route handlers send supplied record data to Gemini for answers and natural-language matching.
- **Product image helper:** Gemini suggestions for a product name, price and Food/Drink category, with a preview before applying them to the form.
- **Translation:** English-to-target model mappings for Spanish, French, German, Chinese, Arabic, Japanese, Korean and Portuguese through Hugging Face.
- **Authentication UI:** external username/password login and registration, alongside Google sign-in through NextAuth.

A text-classification demo is also present. It is not a validated assessment of employees, their emotions or their performance, and should not be used for employment decisions.

## Stack

| Area | Source configuration |
| --- | --- |
| Application | Next.js 16.2.2, React 19.2.4, JavaScript, App Router |
| Styling | Tailwind CSS v4, Framer Motion, Lucide icons |
| Charts | Recharts |
| Sign-in | NextAuth v4 with Google, plus external API authentication |
| AI routes | Google Generative AI SDK and Hugging Face HTTP requests |
| Business data | External employee/product/category REST endpoints |

The image helper uses Gemini, not a separate Google Cloud Vision API integration. Declared packages and model names do not establish current provider availability, free usage or successful execution.

## Local setup

Use Node.js/npm compatible with the declared Next.js version. No engines field or dependency lockfile is included in the supplied export, so this guide does not repeat the old README's unverified Node.js minimum.

From your existing repository checkout, using PowerShell:

```powershell
npm install
Copy-Item .env.example .env.local
```

Edit `.env.local` using the included variable names:

| Variable | Purpose |
| --- | --- |
| `NEXT_PUBLIC_API_URL` | External business API base; defaults to the local port-8080 address in the source |
| `GEMINI_API_KEY` | Server-side Gemini requests |
| `HUGGINGFACE_API_KEY` | Server-side translation/text-classification requests |
| `GOOGLE_CLIENT_ID` / `GOOGLE_CLIENT_SECRET` | Google provider configuration |
| `NEXTAUTH_SECRET` | NextAuth secret configuration |
| `NEXTAUTH_URL` | Application address for NextAuth |

Use your own credentials. Only the public API base belongs in a NEXT_PUBLIC-prefixed setting. The existing code logs partial API keys and request/session details; review [security guidance](SECURITY.md) before supplying credentials or sensitive data.

Start a compatible business backend separately. The previous README links to [the companion Spring repository](https://github.com/SRUN-Sochettra/spring-boot-basics.git); its current contents, setup and compatibility were not inspected for this refresh.

The frontend expects employee, product and category endpoints under `/api/v1/`, with list/detail data wrapped in a `data` property. See [architecture notes](docs/architecture.md) for the client contract and authentication gaps.

Then run:

```powershell
npm run dev
```

Use the address printed by Next.js. Data-backed pages fetch from the external API and can fail if that backend is unavailable or incompatible. Provider keys alone do not make the management pages self-contained.

## Checks and production commands

```powershell
npm run lint
npm run build
npm run start
```

`start` runs the production server after a successful build. No test script or automated test files are included in the supplied export. No installation, lint, build or provider call was executed during this documentation refresh.

Some root UI components import the `radix-ui` umbrella package, but it is not declared in the supplied manifest. Whether those files affect the current build depends on the import graph; verify rather than assuming the declared individual Radix packages satisfy those imports.

## Important boundaries

- Client-side role checks are not server-side authorisation. The external backend's enforcement is not available for inspection here.
- Google sign-in stores a local marker rather than exchanging the session for an external backend token. Backend write compatibility is not established.
- AI handlers accept client-supplied record arrays. Their answers and generated JSON are not authoritative database queries or verified financial calculations.
- Employee names, email addresses and salary fields can be sent to Gemini. Use synthetic or explicitly authorised, minimised data only.
- The vision route fetches a supplied URL without explicit destination, size or timeout checks in that handler. Do not expose it publicly without hardening.
- The contact form simulates success after a delay; it does not send a message. Displayed contact details and response promises are not verified support arrangements.

See [SECURITY.md](SECURITY.md) for source-backed risks. These issues are documented, not fixed by this package.

## Project layout

```text
src/app/             Pages, layouts and Next.js API handlers
src/components/      Management forms, tables, AI tools and layout
src/context/         Authentication context
src/lib/api/         External REST API clients
components/ui/       Root-level reusable UI components
lib/                 Root-level utility helper
public/              Static assets
```

## Contributing and licence

Read [CONTRIBUTING.md](CONTRIBUTING.md) before making changes.

No licence file is included in the supplied export. The npm `private` flag is package metadata, not a source-code licence or proof that the GitHub repository is private. This refresh does not assign redistribution rights.
