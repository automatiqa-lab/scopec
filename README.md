# SCOPEC

Supply Chain Operations Planning & Execution in Commodities.

SCOPEC is a supply chain operations planning and execution tool for commodities. One planning kernel runs S&OP and S&OE on the same data model, and commodity knowledge lives in domain profiles as data, not in code. Soft commodities come first, hard commodities next.

Part of [Automatiqa Lab](https://www.automatiqa.io). Project page: [automatiqa.io/scopec](https://www.automatiqa.io/scopec/).

![SCOPEC system schematic: inputs and a domain profile feed a planning kernel that runs S&OP and S&OE, with kernel services, an approval gate for agents, an action ledger, a glass box for review and reversal, Auspex as an optional prediction service, and the trading book outside the scope](docs/scopec-how-it-works-aleks-sidorecs.png)

*Figure 1 - How SCOPEC works. By Aleks Sidorecs.*

## Why I am building it

Planning software is built around discrete products and factories. Trading software runs contracts, positions and the hedge book, and stops where the physical flow begins. A commodity operation sits between the two: lots instead of units, origin, grade and certification as attributes that decide what a lot can become, crop years and shutdown schedules as the calendar, yields and co-products where a bill of materials would be.

I've spent two decades in supply chain. In commodities the monthly plan usually lives in one spreadsheet and the weekly execution in another, and the two meet in a meeting. By then the vessel has sailed. SCOPEC puts both on one kernel, so a deviation on the quay reaches the plan before the next review does.

## How it works

The numbers follow the callouts in Figure 1.

1. **Inputs arrive as commodity objects** - contracts and call-offs, intake and production plans, lots with origin, grade and certification, positions in transit and afloat, and the disruptions that hit them.
2. **A domain profile is data, never code** - attributes, quality specs, units, calendar, KPI and rule packs, compliance. The kernel never branches on commodity. The soft commodities profile comes first; the hard commodities profile is built over the same kernel second.
3. **One kernel, two horizons.** S&OP plans volumes by family each month. S&OE runs orders, lots and shipments each week and day. Time fences separate them; the published plan moves from one to the other.
4. **Gap to plan is the hinge.** When actuals from operations breach tolerance, S&OE opens an exception with an owner and proposes an amendment to the published plan.
5. **Every constant is a parameter** with scope, effective dates and provenance, snapshotted into each scenario. Thresholds and rules are data too, so two plan versions can be diffed parameter by parameter.
6. **Each prediction slot has a binding** - the kernel's deterministic tier, [Auspex](https://github.com/automatiqa-lab/auspex) as an optional prediction service, or an ensemble. Every number records which binding produced it.
7. **Agents propose, a planner or a policy approves** at the gate. The action ledger records every planner, model and agent action, explained in planning language. The glass box lets you review it, trace a number back to its inputs, and reverse it with a reason.
8. **Physical planning only.** Hedging, mark-to-market and the trading book stay in the CTRM and enter SCOPEC as parameters and scenarios.

**Legend.** Solid line: signal or action. Dashed: measurement or an external service. Dash-dot: system boundary. Green: the loop that keeps the plan honest - actuals, gap to plan, agents under approval, the glass box. Bowtie valve: approval gate. Circled letters: instruments, A for actuals and G for the gate. Hatched block: the physical operation.

## Roadmap

1. **Kernel and data model** - lots, recipes with yields and co-products, contracts, positions, the parameter and rules store, the scenario store, and a synthetic company generator. In progress.
2. **Soft commodities profile** - crop calendars, intake forecasts with weather covariates, quality degradation in storage, certification and EUDR traceability, blend optimisation. Next.
3. **Deterministic model library and S&OP** - demand, inventory, MRP with yields and co-products, capacity as a linear programme, finance and risk.
4. **S&OE** - contract allocation, available-to-promise, shortage control tower, shipment follow-up, gap to plan.
5. **Hard commodities profile** - metals and minerals: assays, wet and dry tonnes, payable metal, shutdown calendars. Built as data over the same kernel, and allowed to break it where the abstraction is wrong.

Out of scope: energy, and the trading book.

## Status

In development, about 20%. Design and data model first; code lands here at v0.1. Built in public.

## EU AI Act

SCOPEC's planned agents and assistant draft text with a language model, so that output will carry transparency marking under Article 50.

- Content a model wrote is marked; content a person wrote, and numbers from deterministic models, are not. Planning numbers carry their provenance in the action ledger instead.
- Marking travels in two channels: a line you can read, and metadata a machine can detect in every export. The schema is `automatiqa-disclosure/1`.
- The model is never named in visible output. Article 50 requires disclosing that content is AI-generated, not which system produced it.
- No model-generated output ships yet. The disclosure layer is wired before the first agent commit, not after.

If you deploy SCOPEC for other people in the EU you are a deployer: keep the marking intact and make sure recipients can recognise AI-generated content as such.

Full classification and reasoning: [COMPLIANCE.md](COMPLIANCE.md).

## Licence

Apache-2.0. See [LICENSE](LICENSE) and [NOTICE](NOTICE).

The argument these projects sit under: [The Intelligent Orchestration Manifesto](https://www.automatiqa.io/manifesto/).
