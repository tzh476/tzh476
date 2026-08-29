# Stephen Smith / tzh476

Independent engineer available for small, bounded remote projects.

[Engineering evidence / project resume](ENGINEERING-EVIDENCE.md)

## Recent upstream work

- **[qBraid/qBraid#1358](https://github.com/qBraid/qBraid/pull/1358)** — pyQuil gate
  parameters were reaching Cirq as `pyquil.quilatom` objects instead of sympy
  expressions, so `cirq.is_parameterized` returned `False` on a circuit that *did* have a
  free parameter, and both `str(circuit)` and `unitary()` raised. Traced from the symptom
  to the two lines responsible, fixed at the conversion layer, and covered with six tests
  — five of which fail without the change. This is the upstream cause of
  [unitaryfoundation/mitiq#2687](https://github.com/unitaryfoundation/mitiq/issues/2687),
  open since March 2025, where an earlier attempt to patch Cirq's diagram formatter was
  declined; the analysis explains why that was the right call. (Open, awaiting review.)

## Technical writing samples

- [The Go parser bug that reserves memory for input it is about to reject](https://github.com/tzh476/go-prealloc-before-validate)
  — a defect class with a runnable demo, and why the obvious one-line fix is
  measurably a regression (a constant cap improved hostile input 6,463x while making
  a valid 1000-element list 2.36x worse; a proportional bound gives 7x with zero
  regression). Includes benchmarks against `buger/jsonparser` and `miekg/dns`.
- [A green CI run is an input, not a conclusion](writing-samples/green-ci-is-not-a-verdict.md)
- [How to build a dashboard that can say "I don't know"](writing-samples/dashboards-that-can-say-i-dont-know.md)

## Digital products

[allocguard](https://payhip.com/b/27A9r) — USD 19, card or PayPal, instant download. A Go checker for the
defect class in the writeup above: capacity reserved from untrusted input before that
input is validated. Reports a site only when the count comes from outside the
function and the fill loop can return early, so it stays silent on six of ten large
Go repositories scanned. Ships with source (MIT), stdlib only, exits 1 on findings.
Its low-precision recursion check is off by default, and the README says why.

[Cash-First Opportunity Scorecard](https://payhip.com/b/xElkF) — USD 29, card or PayPal, instant download. A 15-minute scorecard for deciding whether a bounty, paid writing call, or fixed-price gig is actually worth starting. Advertised prizes are not income.

**Available for one new bounded milestone.** The fastest starting point is a
USD 300 diagnosis-and-fix milestone: reproduce one backend, API, automation, or
data-pipeline defect; deliver the smallest verified correction; add a
regression test; and provide a concise handoff. Scope and payment terms are
agreed in writing before work begins.

[Open a structured milestone request](https://github.com/tzh476/tzh476/issues/new?template=fixed-milestone.yml)
or [request a scoped estimate by email](mailto:tzh476@gmail.com?subject=Scoped%20engineering%20milestone&body=Repository%20or%20system%3A%0ADesired%20outcome%3A%0AReproduction%20or%20fixtures%3A%0ADeadline%3A%0ABudget%20range%3A%0A).

## Good first milestones

| Scope | Typical fixed range |
|---|---:|
| Deploy a Codex-built MVP with a verified staging URL and runbook | USD 300–900 |
| Reproduce and fix a backend or data-pipeline bug, with regression tests | USD 300–600 |
| Integrate an API, LLM agent, RAG, or MCP workflow | USD 500–900 |
| Stabilize a Python, TypeScript, or Go automation pipeline | USD 400–900 |

The exact price is agreed from a written scope before work starts. A first
milestone should have explicit inputs, acceptance criteria, and a reproducible
verification path. I can work asynchronously and disclose any AI-assisted
tooling used during delivery.

For deployment work, the first milestone can cover diagnosis, corrected build
and runtime configuration, a staging deployment, health and smoke checks, and a
rollback/runbook handoff. Production access and infrastructure charges are not
required for the initial diagnosis.

## Working style

- Source-grounded diagnosis before implementation
- Small diffs, automated tests, and explicit failure handling
- No production secrets or customer data needed for an initial milestone
- Clear distinction between verified results, assumptions, and limitations

Public repositories below show work across Python, TypeScript, Go, C/C++, data
systems, automation, model tooling, and infrastructure.

For a scoped inquiry, use the structured request or email link above, or contact
[tzh476@gmail.com](mailto:tzh476@gmail.com). Include the repository or system,
desired outcome, available fixtures, deadline, and budget range. Do not send
passwords, tokens, private keys, or regulated personal data.
