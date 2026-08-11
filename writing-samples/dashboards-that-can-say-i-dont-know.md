# How to Build a Dashboard That Can Say "I Don't Know"

A dashboard can look authoritative while it is built on stale data, a hidden
fallback, or a source that failed quietly. The graph is not the product. The
product is the chain of evidence that lets a reader decide whether the graph is
worth trusting right now.

Useful dashboards therefore do more than show a number. They show where it
came from, when it was observed, which parts failed, and which conclusions are
interpretation rather than measurement.

## Treat each metric as a small contract

For every metric, record four things before rendering it:

1. **Source**: the API, dataset, or generated artifact that produced it.
2. **Observation time**: when that particular value was fetched or computed.
3. **Meaning**: what the value measures and what it does not measure.
4. **Failure behavior**: whether the dashboard hides it, marks it stale, uses
   a named fallback, or stops the report.

A network-health API can report that a public endpoint says it is healthy. It
cannot prove that every application using the network is healthy. A third-party
market-data snapshot can show a provider's last value. It cannot make an
intraday movement a durable causal explanation.

Naming this boundary prevents a number from acquiring extra meaning just
because it moves from JSON into a large dashboard card.

## Store the snapshot with the report

A reproducible dashboard should publish three related outputs together:

- a machine-readable snapshot used to generate the view;
- a human-readable report with source notes and failure flags;
- the rendered HTML or application page.

The [Solana dashboard implementation](https://github.com/tzh476/solana-ecosystem-dashboard)
uses this pattern: one scheduled script produces JSON, Markdown, and a static
dashboard from public APIs. Its companion [radar project](https://github.com/tzh476/solana-ecosystem-radar)
documents the same separation between data, report, and rendered view.

That arrangement answers a useful review question: “What exactly did the
dashboard know when it displayed this?” A reader can inspect the snapshot
instead of guessing from a continuously changing front end.

## Make fallbacks visible

Fallbacks improve availability, but invisible fallbacks destroy
interpretability. If source A fails and source B supplies a similar-looking
number, record all of the following:

- source A failed at a stated time;
- source B supplied the displayed value;
- the two sources may have different definitions or update schedules;
- the dashboard did not compare them as if they were identical.

The correct visual result is sometimes not a chart point. It can be a stale
badge, anomaly notice, unavailable marker, or plain explanation. “Unknown” is
more useful than a precise-looking fabrication.

## Keep thresholds transparent

Dashboards often turn a continuous signal into a red, yellow, or green label.
That can help if the threshold is inspectable. It becomes misleading when the
threshold looks like a prediction model but is actually a hidden guess.

Use language like this:

> Flag this metric when the observed value crosses a chosen operational
> threshold. The flag is a prompt to inspect the source, not a forecast.

The radar project's anomaly rules use this pattern: they expose thresholds for
throughput, slot time, validator delinquency, price movement, and protocol
value movement rather than presenting opaque health scores. A reader can change
or reject a threshold without reverse-engineering the dashboard.

## Refreshes need a provenance trail too

“Auto-updating” does not mean “always current.” A scheduled job can fail, run
late, fetch a partial response, or reuse a stale artifact. Treat the refresh
itself as data:

- record the job's start and completion time;
- include source-level success and failure results;
- publish the generated artifact timestamp;
- preserve the last successful snapshot rather than overwriting it with an
  empty one;
- make a manual rerun possible without secret configuration.

This makes operational failures reviewable and avoids calling a page “live”
when its underlying data is hours old.

## Separate observation from interpretation

The final discipline is editorial. A dashboard may observe that a metric moved
or crossed a threshold. It should label any explanation as an interpretation
unless the source itself proves causality.

- **Observation**: a source reported an eight-percent 24-hour move.
- **Interpretation**: the move may warrant reviewing related metrics.
- **Not established**: why the move happened or what happens next.

This distinction protects readers from treating a monitoring surface as a
prediction engine.

## A practical launch checklist

Before publishing a dashboard, verify:

1. Every number has a named source and observation time.
2. The raw snapshot and rendered report agree on the same run.
3. Failed sources are visible rather than silently replaced.
4. Fallbacks name the replacement source and its limitations.
5. Thresholds are documented as rules, not forecasts.
6. The page distinguishes measured facts from commentary.
7. A fresh run can be reproduced without hidden credentials.

A trustworthy dashboard is not the one that never says “I don't know.” It is
the one that says it precisely, early, and with enough evidence for the reader
to decide what to do next.
