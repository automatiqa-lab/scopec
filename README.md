# SCOPEC

Supply Chain Operations Planning & Execution in Commodities.

SCOPEC is a supply chain operations planning and execution tool for commodities. One planning kernel runs S&OP and S&OE on the same data model, commodity knowledge lives in domain profiles as data, not in code, and agents act on exceptions under an autonomy ladder they earn. Soft commodities come first, hard commodities after.

Part of [Automatiqa Lab](https://www.automatiqa.io). Project page: [automatiqa.io/scopec](https://www.automatiqa.io/scopec/).

![SCOPEC system schematic: inputs and a domain profile feed a planning kernel that runs S&OP and S&OE over kernel services, a network twin and an action ledger; an agentic execution layer runs the OODAA loop on exceptions; a glass box reviews and reverses every action; Auspex is an optional prediction provider and the CTRM stays the system of record](docs/scopec-how-it-works-aleks-sidorecs.png)

*Figure 1 - How SCOPEC works. By Aleks Sidorecs.*

## Why I am building it

Planning software is built around discrete products and factories. Trading software runs contracts, positions and the hedge book, and stops where the physical flow begins. A commodity operation sits between the two: lots instead of units, origin, grade and certification as attributes that decide what a lot can become, crop years and shutdown schedules as the calendar, yields and co-products where a bill of materials would be.

I've spent two decades in supply chain. In commodities the monthly plan usually lives in one spreadsheet and the weekly execution in another, and the two meet in a meeting. By then the vessel has sailed. SCOPEC puts both on one kernel, so a deviation on the quay reaches the plan before the next review does.

## How it works

The numbers follow the callouts in Figure 1.

1. **Inputs arrive as commodity objects** - contracts and call-offs, intake and production plans, lots with origin, grade and certification, positions in transit and afloat, and the disruptions that hit them.
2. **A domain profile is data, never code** - attributes, quality specs, units, calendar, KPI and rule packs, compliance. The kernel never branches on commodity. The soft commodities profile comes first; the hard commodities profile, for processors, is built over the same kernel later.
3. **One kernel, two horizons.** S&OP plans volumes by family each month, three to twenty-four months out. S&OE runs orders, lots and shipments each week and day, up to eight weeks out. Time fences separate them; the published plan moves from one to the other.
4. **Gap to plan is the hinge.** When actuals from operations breach tolerance, S&OE opens an exception with an owner and proposes an amendment to the published plan.
5. **Every constant is a parameter** with scope, effective dates and provenance, snapshotted into each scenario. Thresholds and rules are data too, so two plan versions can be diffed parameter by parameter.
6. **Predictions arrive through a provider contract SCOPEC owns.** Each prediction slot binds to the kernel's deterministic tier, to [Auspex](https://github.com/automatiqa-lab/auspex) as an optional external provider, or to an ensemble; the evaluator in the model registry sets the default binding from backtests. Every number records which binding produced it.
7. **The action ledger records every planner, model and agent action,** explained in planning language. The glass box lets you review it, trace a number back to its inputs, and reverse it with a reason.
8. **The CTRM stays the system of record.** Contracts and positions are imported through an adapter; hedging, mark-to-market and the trading book stay in the CTRM and enter SCOPEC as parameters and scenarios.
9. **The network twin tests the plan.** One discrete-event simulation core runs a scenario forward with stochastic inputs from the prediction slots and returns distributions, not points - service, days of cover, demurrage, margin, and the probability that the published plan holds. The solvers say what to do; the twin says how robust it is. The same core generates the synthetic company the models are tested on, and every run is logged with its seed.
10. **Agentic execution runs the OODAA loop on exceptions.** An exception from the kernel is the trigger. A domain agent - shortage and allocation, purchase order and shipment follow-up, quality hold and claim, replenishment, gap-to-plan amendment - observes it, orients with context from kernel services, decides among options whose numbers come from the solvers and providers, never from the model, passes the gate, acts through the kernel API, and adjusts: the outcome goes to the ledger, and the ledger evidence widens or narrows its autonomy. The kernel API is the agent's tool set, typed and allowlisted, so an agent can do nothing a planner could not; the same tools are exposed over MCP.

**Autonomy ladder.** Observe, propose, apply with approval, apply with notification, autonomous within limits - set per action type and scope as parameters, earned upward on ledger evidence, cut back the same way, with a kill switch per agent. Evals measure each decision and set the level per action.

**Legend.** Solid line: signal or action. Dashed: measurement or an external provider. Dash-dot: system boundary. Green: the loop that keeps the plan honest - actuals, gap to plan, the agentic execution layer, the glass box. Bowtie valve: approval gate. Circled letters: instruments - A for actuals, G for the gate, EV for evals. Hatched block: the physical operation.

## Roadmap

**Stage 1 - soft commodities.**

1. Kernel and data model - lots with genealogy, recipes with yields and co-products, contracts and positions, the parameter and rules store, the scenario store, the action ledger, and the simulation core as a synthetic company generator. In progress.
2. Soft commodities profile - crop calendars, intake forecasts, quality degradation in storage, certification and EUDR traceability, blend optimisation.
3. S&OP and the deterministic model library - demand, inventory, MRP with yields and co-products, capacity as a linear programme, finance and risk; the network twin beside the solvers.
4. S&OE - contract allocation, available-to-promise, shortage control tower, shipment follow-up, gap to plan.

**Stage 2 - agentic execution.** Domain agents on the exception engine, the autonomy ladder, MCP tools, and the assistant - every model-written line marked under Article 50.

**Stage 3 - hard commodities.** Metals and minerals for processors: assays, wet and dry tonnes, payable metal, pricing formulas and settlement, shutdown calendars. Built as data over the same kernel, and allowed to break it where the abstraction is wrong.

Out of scope: energy, and the trading book.

## Status

In development, about 20%. Design and data model first; code lands here at v0.1. Built in public.

## EU AI Act

SCOPEC's agents and assistant draft text with a language model, so that output carries transparency marking under Article 50.

- Content a model wrote is marked; content a person wrote, and numbers from deterministic models, are not. Planning numbers carry their provenance in the action ledger instead.
- Marking travels in two channels: a line you can read, and metadata a machine can detect in every export. The schema is `automatiqa-disclosure/1`.
- The model is never named in visible output. Article 50 requires disclosing that content is AI-generated, not which system produced it.
- No model-generated output ships yet. The disclosure layer is wired before the first agent commit, not after.

If you deploy SCOPEC for other people in the EU you are a deployer: keep the marking intact and make sure recipients can recognise AI-generated content as such.

Full classification and reasoning: [COMPLIANCE.md](COMPLIANCE.md).

## Licence

Apache-2.0. See [LICENSE](LICENSE) and [NOTICE](NOTICE).

The argument these projects sit under: [The Intelligent Orchestration Manifesto](https://www.automatiqa.io/manifesto/).
