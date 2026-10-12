# Expert consultation plan (simple)

*v0.1, 2026-10-13. A deliberately light process for short, directed expert input on specific task families (D9, D18). It follows D7 (contributors, not research subjects; written consent plus a short contributor note), D8 (personal capacity; nothing confidential or export-controlled), D11 (recording only with consent; local transcription), D12 (English) and D13 (unpaid, acknowledged). Candidate names and family-specific material live in the private repository, never here. This is not legal advice.*

## 1. The process in five steps

| Step | What happens | Time for the expert |
|---|---|---|
| 1. Ask | A short outreach message (§3), with no attachments | 1 min |
| 2. Prepare | If they agree: send the family's scenario brief (PDF) and the contributor note (§5) | 15–20 min of reading |
| 3. Talk | A 30–45 minute call following the outline (§4) | 30–45 min |
| 4. Confirm | Within 48 hours, Guglielmo sends a one-page write-up (§6) for them to correct, redact or approve | 5–10 min |
| 5. Use and credit | Agreed changes go into the family specification with provenance "E" (expert input); acknowledgement as they chose | — |

Two or three consultations per family is the target. Never send task instances, reference solutions or hidden data to anyone: experts see the scenario and the design questions only.

## 2. Practicalities

- **Recording:** only with explicit consent, asked again at the start of the call. Transcribe locally on Guglielmo's Mac (e.g. Whisper); never upload audio to third-party services. If there's no consent, take notes by hand.
- **AI processing:** Claude may draft the write-up from the transcript or notes (D11). The contributor note says so, with an opt-out. The expert always approves the write-up.
- **Retention:**
  - Delete audio once the write-up is approved.
  - Keep transcripts and notes in the private repository, deleting them on request.
  - Contact details stay out of all repositories.
- **Confidentiality both ways:** experts share nothing employer-confidential or export-controlled. They are asked not to share the brief or paste it into AI tools, because it is benchmark material.

## 3. Outreach templates

**LinkedIn connection note (≤300 characters)**

> Hi [Name], I'm an aerospace engineer (PoliMi / Cranfield / Imperial) building an open benchmark of AI agents on real aerospace engineering work. Your work on [topic] is very relevant. Could I ask for a 30-min call to sanity-check one scenario? Thanks, Guglielmo

**Email**

> Subject: 30 minutes of your expertise on a [static-test correlation] scenario?
>
> Dear [Name],
>
> I'm Guglielmo Cimolai, an aerospace engineer (PoliMi BSc, Cranfield MSc in Aerospace Structures, Imperial MSc in AI). I'm building AerospaceBench, an independent, open benchmark that tests whether AI agents can do realistic aerospace engineering work, not answer exam questions. Each task is a full work package checked by independent physics-based verifiers.
>
> I came across your work on [specific paper/project], and I'd be very grateful for your view on one scenario: [one line, e.g. "explaining why a wing-box strain survey doesn't correlate with the FE model, and deciding whether the test can proceed to limit load"]. I'd send a short brief first (about 15 minutes of reading), then a 30–45 minute call at your convenience.
>
> This is a personal, unpaid project. I'd gladly acknowledge your contribution by name (or keep it anonymous if you prefer). Nothing confidential would be asked.
>
> Kind regards,
> Guglielmo Cimolai
> [LinkedIn] · github.com/gimocimo/AerospaceBench

**Reminder (after 7–10 days, once only)**

> Just a gentle follow-up in case my note got buried. No problem at all if it's not a good time.

## 4. Call outline (30–45 minutes)

| Min | Topic |
|---|---|
| 0–3 | Thanks; check the consent choices (recording, AI-drafted write-up, acknowledgement); confirm nothing confidential will be discussed |
| 3–8 | Their background on this topic (what they have actually done, and roughly how often) |
| 8–15 | **Realism:** does the scenario read like real work? What would a real engineer receive, and what's missing or odd? |
| 15–30 | **The family's specific questions** (from the brief's question list), most important first. Ask for concrete examples ("the last time you saw this…") and typical numbers |
| 30–38 | **Judging the answer:** what would make them accept or reject an engineer's deliverable here? Which mistakes are unacceptable, and which are minor? |
| 38–42 | Anything we haven't asked; who else they'd suggest |
| 42–45 | Next steps: the write-up within 48 hours, and their acknowledgement preference |

**Tips:**
- Don't show Claude-written example solutions or "answers".
- Ask about practice, not opinions about AI.
- Note any number they give, together with its source ("rule of thumb", "company standard", "standard X").

## 5. Contributor note (send with the brief; the expert replies "I agree" by email)

> **AerospaceBench — contributor note**
>
> Thank you for helping. In plain terms:
>
> 1. **Voluntary and unpaid.** You can stop or withdraw at any time. If you withdraw within 4 weeks of the call, I will remove your input from the benchmark.
> 2. **Personal capacity.** Please share only general professional knowledge. Do not share anything confidential to your employer or clients, or anything export-controlled.
> 3. **Recording (optional).** If you agree, the call is audio-recorded and transcribed on my own computer. The audio is deleted once you approve the write-up.
> 4. **AI assistance (optional).** I may use an AI assistant (Claude) to draft the write-up from the transcript or my notes. You can opt out. You will always see and approve the write-up.
> 5. **Use of your input.** You allow me to use, adapt and publish your suggestions as part of AerospaceBench, under the open licence chosen for the benchmark. You keep any rights you already hold in your own knowledge and materials.
> 6. **Credit.** You choose: named acknowledgement (name and, if you wish, affiliation), or anonymous. No co-authorship is implied.
> 7. **Confidentiality of the brief.** Please don't share the brief or paste it into AI tools: it is benchmark material, and public exposure would weaken the test.
> 8. **Your data.** Your contact details are kept privately, used only for this project, and deleted on request.
>
> Please reply "I agree", noting any opt-outs (recording / AI drafting) and your credit preference.

## 6. Write-up template (one page, sent within 48 hours)

- **Contributor** (as they wish to be credited) · date · family · recorded: yes/no · AI-drafted: yes/no.
- **Realism verdict:** one or two sentences in their words.
- **Points raised**, one line each, marked as *fact or practice* / *rule of thumb* / *opinion*, with any numbers and sources.
- **Suggested changes to the scenario** (what, and why).
- **Their view on judging the deliverable:** acceptable vs unacceptable mistakes.
- **Open questions or follow-ups.**
- **Please confirm or correct by [date].**

## 7. Tracker (private repository only)

One row per contact: pseudonymous ID, family, route (alumni network, LinkedIn, referral), status (asked / agreed / brief sent / call done / write-up approved / declined), consent choices, credit preference, link to the write-up. Real names and emails stay out of git, or appear only in the private repository if Guglielmo prefers.
