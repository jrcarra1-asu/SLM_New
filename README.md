# SOAPIE Case Note Simulator

**AFRCC / DoD Case Note Quality Grader & Practice Simulator**  
*Streamlit app for mastering concise, compliant S.O.A.P.I.E. documentation*

This tool helps Recovery Care Coordinators (RCCs), case workers, and advocates rapidly develop the exact skills taught in the official Defense Health Agency (DHA) “Writing Case Notes” training module. It is purpose-built as the core NLP component for your AI-enabled VR-XR interview simulation on Meta Quest 2/3+.

## Why This Exists (Your VR-XR Project Alignment)

Your goal is a transferable AI model that:
- Uses NLP to **read and grade** case notes in real time
- Gives **actionable tips** on capturing critical details during client interviews
- Produces **concise yet highly informational** notes focused on client progress and updates

`SLMapp-2.py` delivers exactly that today — as a transparent, auditable rule-based SLM proxy — while providing a clear migration path to a fine-tuned small language model (ONNX export) for on-device Unity inference.

## How It Works (Technical — Fully Transparent & ISO 42100 Compliant)

The engine in `SLMapp-2.py` performs deterministic analysis with zero external API calls or model weights in the current version:

1. **Input**: Free-text case note (narrative prose recommended; target ≤150 words for guidance only).

2. **Strict Structural Checks** (drawn directly from DHA PDF):
   - Rejects **any** bullet points, numbered lists, or fragments (`- • * > 1. a)`)
   - Requires ≥3 complete sentences (detects proper `[.!?]\s+[A-Z]` endings)
   - Flags excessive ALL-CAPS or unapproved jargon

3. **SOAPIE Coverage** (minimum 4 of 6 elements for “Complete”):
   - **S** — Subjective: client statements, feelings, history (“says”, “reports”, “feels”, “expressed”)
   - **O** — Objective: direct observations & data (“noticed”, “observed”, “appeared”, “RCC noted”)
   - **A** — Assessment: professional conclusion (“believes”, “assesses”, “recognizes”, “ready for”)
   - **P** — Plan: future actions & measurable goals (“agreed”, “deadline”, “will”, “goal”)
   - **I** — Implementation: actions executed (“attended”, “completed”, “as planned”)
   - **E** — Evaluation: outcomes & effectiveness (“received”, “outcome”, “effective”, “achieved”)

4. **Binary Verdict + Full Audit Trail**:
   - **Complete** = No bullets + ≥4 SOAPIE elements + coherent sentences + professional tone
   - **Incomplete** = Returns precise, itemized reasons (e.g., “Only 3/6 elements covered”, “Contains sentence fragments”)

5. **Actionable Output**:
   - Specific tips tied to missing elements
   - Fully rewritten compliant note (narrative, concise, full SOAPIE flow)
   - Structured JSON (timestamp, coverage map, status, reasons, improved_note) — ready for Unity C# or DoD-CMS logging

All decisions are explainable line-by-line in the source — satisfying ISO 42100 transparency, DAFI 34-1101 documentation standards, AFRCC KPIs, and strict data governance (no PII ever stored or transmitted; human-in-the-loop always).

## How to Use & Deploy (Emphasized: SLMapp-2.py)

**Use `SLMapp-2.py` for all production and demo deployments.**

### 1. Local Run (Fastest for Development)
```bash
git clone https://github.com/jrcarra1-asu/SLM_New.git
cd SLM_New
pip install -r requirements.txt
streamlit run SLMapp-2.py
```
Browser opens at `http://localhost:8501`. Paste a sample note and grade instantly.

### 2. One-Click Cloud Deploy (Recommended for Sharing)
- Push/fork the repo to GitHub
- Connect at [share.streamlit.io](https://share.streamlit.io)
- Select **SLMapp-2.py** as the main file
- Deploy → instant public URL for VR-XR testers or training cohorts

### 3. VR / Unity Integration (Meta Quest 2 → 3+)
- Export the JSON result from the app
- In Unity: deserialize → feed to Sentis/Barracuda (current rule engine) or future ONNX model
- Real-time coaching overlay during simulated interviews: “Your note is Incomplete because it lacks an Evaluation statement — here is a compliant version.”

**Future Path**: Use the companion `Vizitech (1).ipynb` notebook to fine-tune a DistilBERT-style SLM on `train.jsonl`/`test.jsonl` → export to ONNX/TorchScript → drop-in replacement for the rule engine inside the headset (target <50 ms, fully offline).

## Quick Tips for Effective Case Notes (DHA / AFRCC Best Practices)

- **When**: During or immediately after every interaction (ask permission first). Enter into DoD-CMS or Service system as soon as possible.
- **How**: Narrative prose only — never bullets. Full sentences. Approved terminology only.
- **What to Capture** (progress-focused):
  - Client’s own words about status, concerns, accomplishments (S)
  - Your observations of well-being and behavior (O)
  - Your professional synthesis of needs (A)
  - Agreed short- and long-term measurable goals (P)
  - What was actually done (I)
  - Measurable outcomes and goal progress (E)
- Keep it concise (≤150 words ideal) while remaining complete and readable.

Iterate in the simulator until you receive “Complete” — then export the JSON for your VR training log.

## Repository Contents

- `SLMapp-2.py` — **Primary recommended app** (stricter binary verdict, best for production)
- `SLMapp.py` — Earlier variant
- `SLM.py` — Core logic module
- `Vizitech (1).ipynb` — SLM fine-tuning & ONNX export notebook
- `train.jsonl` / `test.jsonl` — AFRCC-style labeled examples
- `requirements.txt` — Streamlit and minimal dependencies

## Compliance Summary

- **Source Material**: DHA “Writing Case Notes” PDF (SOAPIE definitions & examples)
- **Standards**: DAFI 34-1101, AFRCC Training KPIs, ISO 42100 (transparency, accountability, human oversight), good data governance
- **Guardrails**: Local processing only, no PII retention, full explainability, never automates clinical decisions

**If it isn’t documented correctly, it didn’t happen.** This simulator ensures every note you practice meets that standard.

For Unity scene integration help, ONNX export assistance, or custom fine-tuning, open an issue.

