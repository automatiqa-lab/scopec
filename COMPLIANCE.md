# EU AI Act compliance - SCOPEC

Not legal advice. This file records how SCOPEC meets its transparency obligations, and why.
It is the project's defence file: every entry is dated, and its history is in git.

Contact for compliance and incident reports: aleks@automatiqa.io

## Role classification

**SCOPEC is open-source software published by Automatiqa Lab, which will be its provider once it
generates content with a language model.** The lab places SCOPEC on GitHub under its own name. The
planned agents and assistant draft text - explanations, recommendations, messages - with a model
chosen by whoever runs SCOPEC, so the lab is the provider of the system that generates that text and
carries Article 50(2) marking; whoever runs SCOPEC for other people is a deployer under Article
50(4). The lab does not train, fine-tune or distribute models and is not a provider of a
general-purpose AI model; Chapter V obligations stay with the model providers. The Article 2(12)
open-source exemption is not relied on, because it ends with any commercial use.

Recorded 2026-10-11.

## Obligations that apply

| Obligation | Applies | How this project meets it |
|---|---|---|
| Art. 50(1) interaction disclosure | yes, once the assistant ships | The assistant identifies itself as AI at the start of every session |
| Art. 50(2) machine-readable marking | yes | Disclosure middleware at the agent boundary; metadata in every export path (JSON, HTML, Markdown, PDF, XLSX, PPTX) and in the action ledger |
| Art. 50(4) deployer disclosure | deployer-side | See "What remains the deployer's job" |
| Art. 4 AI literacy | yes | This file, the README section and the glass box explanations |
| Art. 5 prohibited practices | screened | No profiling of people, no manipulation, no biometrics; SCOPEC plans goods, not persons |
| Annex III high-risk | no | See screening below |

## Annex III screening

SCOPEC plans the physical flow of commodities - contracts, lots, positions, capacity, shipments.

- **Critical infrastructure (Annex III point 2)** is the near miss. It covers safety components in
  the management of critical digital infrastructure, road traffic, and the supply of water, gas,
  heating and electricity. SCOPEC plans commodity supply chains, not utilities, and energy is out of
  its scope by design. It is not a safety component of any of them.
- **Employment (point 4)**: SCOPEC assigns no work to people and evaluates no one. Exceptions carry a
  named owner as a task assignment inside a planning team, not as a decision about the person.
- **Essential services and credit (point 5)**: counterparty risk is scored for companies and
  contracts, never for natural persons.
- No other Annex III area is touched.

Screened 2026-10-11. Re-run on any change of purpose or new modality.

## What this project does out of the box

At 20% no model-generated output ships. The design fixes the architecture now:

- The disclosure boundary sits at the agent layer: every agent action is written to the action
  ledger through one constructor, which carries the marking fields, so no path can bypass it.
- The glass box shows a readable AI-generated line next to model-written text; exports carry the
  `ai_generated` metadata object.
- Wording comes from `disclosure.yaml`, copied from the lab's canonical source.

Marking schema: `automatiqa-disclosure/1`, wording from `disclosure.yaml` version 1 (added with the
first agent commit).

## Carve-outs, and why

- **Numbers from deterministic models and statistical forecasters** - plans, forecasts, safety
  stocks, solver outputs - are planning outputs, not synthetic text. They carry full provenance in
  the action ledger (model, binding, parameters, data version) instead of an AI label; labelling
  every number would misstate what produced it and train planners to ignore the label.
- **Content a planner writes or materially edits** before publishing a plan is under the planner's
  editorial control and is not labelled.
- **Retrieved source data** - contracts, receipts, quality reports - is never labelled.

## What remains the deployer's job

- Keep the marking intact in every export and downstream system.
- Tell your own recipients when content they receive was AI-generated.
- Put authentication in front of any exposed instance; SCOPEC holds commercial data.

## Decision log

| Date | Decision | Why |
|---|---|---|
| 2026-10-11 | Classified as provider of the content-generating system once agents ship; Annex III screened negative | Agents and assistant draft text with a model the operator chooses |
| 2026-10-11 | Deterministic and statistical outputs carry ledger provenance, not an AI label | They are planning numbers, not synthetic text; blanket labels mislead |
| 2026-10-11 | Disclosure middleware is wired before the first agent commit | Retrofitting a boundary leaves paths uncovered |
