# Meridian Operations Workbench

Meridian is a static, browser-based training environment for product taxonomy annotation, catalog issue management, and checkout regression QA. The trainee profile shown in the workspace is `trainee11`.

## Assignment at a glance

- **Batch:** PT-100, with exactly 100 product records across 15 cosmetic families.
- **Catalog evidence:** product name and variant, brand, packaging, net contents, GTIN, lot, description, ingredients, suitability, claims, directions, proposed taxonomy, and synthetic reference taxonomy.
- **Product imagery:** 100 newly recomposed offline SVG still-life illustrations. Each record varies its product silhouette, package color, framing, surface props, backdrop, lighting accents, product label, and scene identifier across ten studio art directions. No image API key or remote image host is required.
- **Catalog quality exercise:** 22 records contain a subtle seeded text inconsistency across description, ingredients, suitability, claims, directions, product title, or net contents. A supplier master note is shown as the comparison source. Trainees must identify and document the discrepancy; the fix is not automatically applied when the issue is noticed.
- **Learning goals:** complete 100 decisions/escalations, achieve at least 95% practice accuracy on scored labels, self-attest evidence review for every submission, provide a 20-character evidence rationale for changes/escalations, and resolve and verify the 8-check checkout exercise.
- **Ticket goal:** create tickets for actionable catalog issues and carry them through assignment, investigation, resolution, and closure with evidence and a correct product link.

## Product review workflow

1. Open **Product taxonomy v3** / **Label review** and select an item in Batch PT-100.
2. Inspect the whole packshot and every relevant catalog field. Use zoom when required.
3. Check **Evidence reviewed** only after inspecting the image and record.
4. Confirm the proposal, choose a better taxonomy path, or escalate an unresolved conflict.
5. Set your own confidence. Category changes and escalations require an evidence rationale of at least 20 characters.
6. Submit. The decision, confidence, inspected flag, and rationale are saved in browser local storage. A category mismatch to the bundled synthetic reference gives practice feedback.
7. Use **Outstanding** for pending and skipped items, and **Reviewed** for submitted decisions and escalations. Skipped records do not count as complete.

## Assignment goals and quality checks

The Overview goal board tracks submitted coverage, the sample accuracy target, exception documentation, and the most recent QA-run result. Coverage treats submitted category decisions and documented escalations as completed; skipped items remain outstanding. The accuracy value counts scored category decisions only and excludes escalations. All reference answers and scoring feedback are available in the client-side demo, so these measurements are practice feedback, not blind assessment scores.

## Ticket workflow

Create a ticket from a product review or from **Work tickets**. A ticket captures type, linked SKU, affected work area, impact, priority, owner, optional due date, evidence reference, a 12-character summary, 40-character issue description, and 20-character expected outcome. For a text discrepancy, select the exact catalog field. The current value is captured as read-only evidence; enter the proposed value and a reference to the supplier master or other evidence. Active past-due work is marked overdue. The lifecycle is **Open → In progress → Resolved → Closed**. In progress, apply the correction to the linked catalog record and write a resolution summary. Resolution sends the change for reviewer sign-off; closure is a separate review action. Reopening requires a reason and returns the ticket to In progress. The list supports Active, Open, In progress, Resolved, Closed, and Overdue filters. Ticket changes, applied corrections, and activity history are stored in this browser. The Overview quality goal counts a defect as complete only after its matching field correction has been applied and the ticket is resolved or closed.

### Product text inspection and correction practice

Review the image, product copy, packaging, directions, and supplier master note together. The seeded issues are intentionally subtle: a misplaced phrase, contradictory use, mismatched package quantity, unsupported ingredient, or copy inconsistent with the product use. There are 22 affected records in the 100-item PT-100 catalog. Not every record is defective; file a ticket only when you can explain a supported discrepancy. For a correction ticket:

1. Select the specific SKU and field that contains the problem. The observed value is loaded from the catalog.
2. Compare it with the supplier master note and supporting details elsewhere in the product record. Record an evidence reference and explain the impact.
3. Propose the corrected value, acceptance outcome, owner, impact, and priority. Create the ticket.
4. Accept the ticket to move it to In progress. Recheck the evidence, apply the correction in the ticket, and record a resolution summary.
5. Submit for review. A reviewer checks the linked record and resolution before approving closure. Reopen with a reason if the correction is incomplete.

The Overview card shows progress for the 22 seeded catalog issues independently from taxonomy coverage and checkout QA. The supplier master note deliberately acts as training evidence; it exposes the intended source value in this demo and is not suitable for blind assessment.

## Checkout QA workflow

**Test runs** hosts a self-contained storefront fixture and executes eight browser-DOM assertions covering product detail, cart, SAVE10 discount, delivery selection, guest checkout, order confirmation, notification state, and cart clearing. Build 184 has two intentional defects: SAVE10 leaves a $48.00 subtotal unchanged instead of reducing it to $43.20, and the confirmation view does not wait for the order-created event. Inspect the failures, select one or both scoped patch choices, apply the patch set, rerun the checks, and review the assertion results. Restore build 184 to repeat the exercise.

This runs in the browser against an embedded training fixture. It does not edit source code, connect to a staging/production shop, process payment, place a live order, send a real email, run Playwright, or invoke GitHub Actions.


## Written mock interview

The **Mock interview** workspace presents trainee11 with a fixed 30-question English assessment. The trainee reads one prompt, writes an answer, submits it, and continues to the next question. After the thirtieth answer, the complete question-and-answer transcript is sent to the authenticated Supabase Edge Function for server-side scoring. The report shows an overall percentage, rubric scores, evidence-based notes, strengths, and practice priorities. The trainee can download the answers and score; recent session history is kept in this browser. There is no microphone or voice-interview path in the main application.

The Supabase project URL and publishable key in `interview-config.js` are browser-safe project configuration. The OpenAI key and Supabase service-role key must only be stored as Supabase server secrets. The final score requires the server function and its configured OpenAI API key; the browser does not invent a local score. To configure the backend, enable anonymous sign-ins, apply the quota migration, set `OPENAI_API_KEY` as a Supabase secret, and deploy `mock-interview`:

```powershell
supabase link --project-ref mweohabshlexvbtuoldq
supabase db push
supabase secrets set OPENAI_API_KEY=your-server-side-key OPENAI_MODEL=gpt-6-astra
supabase functions deploy mock-interview
```

The function validates Supabase anonymous sessions, limits each practice profile to 60 model requests per rolling 24-hour period, and sends the written transcript to OpenAI Responses with `store: false`. The assessment is practice feedback, not a hiring decision system.
## Run and deploy

Open `index.html` in a current browser, or publish the files from the repository root with GitHub Pages (`Settings → Pages → Deploy from a branch → main → /(root)`). Product illustrations are embedded as SVG data URLs and therefore do not depend on relative asset paths or project-subpath configuration. To regenerate optional standalone copies under `assets/products/`, run:

```powershell
node work/generate-product-packshots.cjs
```

The generator creates the 100 current packshots and removes obsolete generated SKU assets beyond this batch.

## Data and assessment limits

The profile is not authenticated. Annotation progress, tickets, and QA history are stored locally in the browser and are not synchronized to reviewers. The `supabase/` directory contains a not-yet-integrated server-side review scaffold. This browser build must not be used for blind assessment because the reference labels and immediate feedback are shipped to the browser. A real assessed deployment requires the frontend to be wired to authenticated server submissions, private reference data, server scoring, and reviewer reporting.
