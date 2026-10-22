# Security and data-handling guidance

The supplied code has unresolved trust-boundary and data-handling concerns. Use synthetic data in a controlled development environment until they are addressed. This document is not a security audit or a claim that the application is safe to deploy.

## Source observations

### AI route access and data scope

The inspected dashboard, Gemini, search, sentiment, translation and vision handlers contain no explicit session/role checks or rate limiting. Several accept record arrays directly from the request. No separate middleware file is included in the export. External infrastructure protections were not inspected, so these observations do not establish the complete deployed access policy.

Authorise requests server-side, constrain data to the caller's permitted scope, validate inputs and bound provider usage before exposing these routes publicly. These are recommendations, not changes included in this documentation.

### Logs and errors

The Gemini and translation handlers log partial API-key values. Other logs include questions, submitted text, provider responses and session data. Several handlers return underlying error messages to clients. Remove sensitive logging and review error redaction before using real credentials or records. Rotate exposed credentials where necessary; deleting a log statement does not erase historical logs.

### Remote image fetching

The vision handler fetches the submitted imageUrl and buffers the response without explicit URL-destination restrictions, redirect controls, byte limits or timeout handling in that function. It labels the provider input as JPEG regardless of the fetched response type. This creates a server-side request and resource-abuse risk that needs a separate implementation fix and testing.

### Authentication boundaries

ProtectedAction and tab visibility checks run in the browser. Product mutation helpers attach a stored bearer token; employee mutation helpers do not use that shared auth helper. The external backend is absent, so its actual enforcement and accepted credentials cannot be verified here.

AuthContext stores external credentials in localStorage. For Google users it stores a marker instead of obtaining a backend access token. Do not treat that marker as proof of backend authorisation. Review session storage and the token exchange contract before claiming unified authentication.

### AI disclosure and output trust

Gemini prompts can contain employee names, emails, departments and salaries. Translation/text classification sends supplied text to Hugging Face. Use only authorised, minimised data and review provider terms separately. No retention, deletion, encryption or compliance guarantee is established here.

Generated JSON is parsed after removing code fences in several routes, without a runtime response schema in those handlers. Treat model output as untrusted and verify record matches and numeric results independently. Do not use generated output or text classification to evaluate employees.

## Reporting

Do not publish secrets, employee records or actionable exploit details in a public issue. Use a verified private maintainer channel if one exists, or request a private contact without posting sensitive details. No security email, supported-version schedule or response deadline is confirmed by the supplied repository.

The app's contact form only simulates success. It is not a vulnerability-reporting route. If credentials are exposed, revoke or rotate them and review logs and history.

These findings describe selected source paths, not executed attacks, a complete vulnerability inventory or repaired behaviour.
