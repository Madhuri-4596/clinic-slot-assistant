# Clinic Slot Assistant — project story draft

This draft reflects the current implementation and the authorized live test. Add the contribution and video links before submitting it on Devpost.

## Inspiration

My work on healthcare applications showed me how much coordination happens around appointments. A cancellation creates an opening for the clinic and a chance for someone on a waitlist to be seen sooner. Clinic Slot Assistant explores how a short, purposeful AI phone conversation can help staff coordinate that opportunity.

## What it does

Staff choose an open appointment and a person whose contact consent is recorded, then preview and approve a call. The assistant asks about the exact appointment time and returns a yes, no, or unknown response for review. Staff see the response, supporting evidence, and transcript before confirming a booking. An unclear answer, refusal, or unanswered call keeps the appointment open.

The prototype uses fictional people and appointments. A labelled simulation lets visitors explore four outcomes without dialing a phone. The server-side CALL-E integration was tested with a consenting volunteer: the first attempt reached screening, and a second attempt explicitly requested by the volunteer connected, captured an acceptance, and returned `can_attend: yes`. The result appeared in the app and supported a staff-confirmed fictional booking. This prototype does not connect to a real clinic calendar.

## How it is built

The application uses Next.js, React and TypeScript, with the official CALL-E TypeScript SDK. The server sends an explicit recipient and a structured yes/no/unknown response schema. The API key stays on the server. Local state retains the call reference and staff decisions across refreshes.

Each invitation has a persisted idempotency key. Starting another call for the same slot or person is blocked while a result remains unresolved. Status checks retrieve the existing provider call instead of starting a new one.

## Engineering challenges

The key challenge was defining what should happen when an answer—or even the network response that starts a call—is uncertain. An ambiguous reply must not become a booking. An uncertain call-creation response must not trigger a second call. The implementation keeps those states visible, preserves their references, and requires staff review.

A browser test also caught a difference between the local request host and the framework's internal URL. The origin check now validates the actual request host, with a regression test that rejects external origins.

## What is working

- Appointment selection, waitlist search, consent checks, and call preview.
- Explicitly simulated acceptance, refusal, unclear-answer, and no-answer paths.
- Response evidence, transcript review, staff confirmation, and an activity log.
- Persistence across refresh and protection against duplicate invitations.
- Sixteen passing workflow tests, browser checks of simulated booking, and an authorized live test followed by a fictional booking.

## Next steps before submission

Record an actual demonstration, prepare the contribution PR, and add the resulting links. The account and live test are complete. Keep the private API key, volunteer number, and unapproved recording/transcript details out of the public submission. Any performance or impact claims should come from measured results; this draft claims no clinical deployment or time-saving benchmark.

## Built with

Next.js, React, TypeScript, Node.js, CSS, CALL-E SDK.


Public deployment preparation: a simulation-only Vercel build uses per-browser fictional state, blocks server workspace actions, and never requires CALL-E credentials. Organizer contribution: https://github.com/CALLE-AI/awesome-phone-call-agents/pull/505. Public URL will be added after deployment verification.
