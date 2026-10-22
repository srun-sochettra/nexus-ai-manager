# Architecture and implementation notes

## Scope

This is a Next.js interface with server-side AI route handlers and a separate business-data API dependency. The source uses NexusAI branding while the manifest still names the package test-next. The documentation refresh changes neither package metadata nor application branding.

## Data flow

Employee/product/category clients read NEXT_PUBLIC_API_URL and default to a local port-8080 API. Server-rendered pages load records through these clients and pass them to client components. The floating chatbot also fetches record lists in the browser.

The frontend expects these paths:

| Area | Paths used by the client |
| --- | --- |
| Employees | GET /api/v1/employees/all; GET, PUT and DELETE /api/v1/employees/{id}; POST /api/v1/employees/add |
| Products | GET /api/v1/products/all; GET, PUT and DELETE /api/v1/products/{id}; POST /api/v1/products/add |
| Categories | GET /api/v1/categories/all |
| Accounts | POST /api/v1/auth/login and /api/v1/auth/register |

List/detail helpers read a data property. Account helpers additionally check a success property. These are frontend expectations, not a verified specification of the separate backend. Its schema, CORS configuration, token validation and persistence were not inspected.

## Authentication

Username/password forms call the external API. AuthContext stores returned user/token values in localStorage. Product mutations use getAuthHeaders and handleAuthError; employee mutations construct only a JSON content-type header.

Google sign-in is configured through NextAuth's Google provider. Its callbacks default the role to USER. The client context stores a Google marker in the token slot; no backend token exchange is implemented in that path. Browser role checks control visible actions but cannot establish server-side permission enforcement.

## AI tools

The Gemini handlers cover dashboard summaries, questions, search, model listing and image-based product suggestions. The vision prompt limits suggested categories to Food or Drink. It uses the Google Generative AI SDK, not a separate Cloud Vision client.

Hugging Face routes provide English-to-target translation model mappings and a binary text-classification model. The UI's neutral fallback does not establish that the configured classifier returns a neutral class. Model mappings and UI availability do not prove the providers currently serve those models.

The chat UI retains conversation messages locally, but its requests send the current question and record arrays, not the displayed conversation history. It should not be advertised as a persistent multi-turn memory service.

## Dashboard interpretation

Some cards and charts are calculated in the browser from loaded records. Optional AI results are separate generated content, not authoritative aggregates. Product chart data uses the first eight loaded products rather than sorting them into a top-eight ranking. Department chart data is limited to six entries, and the displayed department count uses that limited array.

These source details do not establish real-time subscriptions, historical trends or anomaly detection. No such infrastructure was found in the inspected paths.

## Other boundaries

The contact form delays and sets local success state without a network submission. Social links include placeholders; support addresses and response promises were not verified.

Several root UI primitives import radix-ui, while package.json declares individual @radix-ui packages rather than that umbrella package. This is a dependency mismatch to investigate if those primitives enter the build graph, not an executed build failure.

No automated test files or test script were found in the supplied export. No dependency installation, compilation, lint, browser check, backend call or AI request was performed during this refresh.
