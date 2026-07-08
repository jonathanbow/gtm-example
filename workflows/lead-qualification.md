# Lead Qualification

- **Purpose:** Move an inbound lead from form fill to a qualified handoff to sales.
- **Apps involved:** Website, HubSpot, Clay, Slack
- **Frequency:** Real-time
- **Status:** Active
- **Last reviewed:** 2026-07-08

## Steps

**1. Website — Visitor submits contact form**
- Trigger: Event · Automation: Automatic
- Direction: Website → HubSpot
- Data: name, email, company
- Owner: -

**2. HubSpot — Create/update contact record**
- Trigger: Event · Automation: Automatic
- Direction: internal
- Data: contact fields
- Owner: -

**3. Clay — Enrich company data (headcount, funding, industry)**
- Trigger: Event (webhook) · Automation: Automatic
- Direction: HubSpot → Clay → HubSpot
- Data: domain, headcount, funding, industry
- Owner: -

**4. HubSpot — Score lead based on enrichment + form data**
- Trigger: Rule · Automation: Automatic
- Direction: internal
- Data: lead_score
- Owner: -

**5. HubSpot — Post summary to Slack if score > 80**
- Trigger: Event · Automation: Automatic
- Direction: HubSpot → Slack
- Data: name, company, score, source
- Owner: -

**6. Slack / AE — AE reviews and claims lead**
- Trigger: Manual · Automation: Manual
- Direction: Slack → AE
- Data: -
- Owner: Dave SDR

**7. HubSpot — AE updates deal stage to "Working"**
- Trigger: Manual · Automation: Manual
- Direction: internal
- Data: deal_stage
- Owner: Julie Admin

## Notes / failure modes

- Clay enrichment occasionally times out on smaller/unlisted companies — lead
  still scores, just with fewer signals. Not currently flagged when this happens.
- No fallback if Slack notification fails silently (e.g. channel archived) —
  lead can sit unclaimed. Worth adding a daily "unclaimed leads" digest.
- Step 6/7 is the only manual gate in the workflow — everything else runs
  unattended.
