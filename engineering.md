---
layout: page
title: Engineering
permalink: /engineering/
---

# Intelligence engineering

I don't only consume platforms — I encode the intelligence method into tooling, so the
discipline is enforced by construction rather than remembered. Each tool below is a
working program with tests, a continuous-integration job and a security boundary, not a
script.

## cryptonym — deciding whether two vendor names are the same actor

**Problem.** Vendors disagree on names — *Volt Typhoon*, *VOLTZITE*, *Insidious Taurus* —
and platforms store aliases as a flat list of assertions. Identity becomes an accident of
data entry.

**Method.** Treat a merge as a hypothesis. Evidence is typed by diagnostic strength:
infrastructure, code and operator artefacts can bear on identity; shared tradecraft
cannot. Competing explanations are tested with Analysis of Competing Hypotheses, and every
claim carries two axes — confidence and probability — kept separate. An external-truth
filter admits a binary truth only when *independent* sources assert it; genuine
disagreement is recorded as contested, not resolved by fiat.

**Evidence.** 87 tests; an adjudicated corpus with a committed coverage baseline; STIX 2.1
output validated against the reference library in CI.

## sharegate — a handling check before a product leaves the building

**Problem.** Most leaks are mundane: a missing TLP marking, a conflicting one, a
`TLP:CLEAR` on sensitive content, or malformed STIX.

**Method.** Two objective gates judged against published specifications — FIRST TLP 2.0
and STIX 2.1 — including a permissive-mark-on-sensitive-content check.

**Evidence.** 21 tests, 94% coverage. Because the ground truth is a schema, the checks are
not opinions.

## huntplan — turning a question into a falsifiable hunt

**Problem.** "Hunting" that is really browsing: no hypothesis, no way to close.

**Method.** Force a falsifiable *If … then …*, name the ATT&CK data sources that would
satisfy it, and require a closure criterion. Maturity is scored against the Hunting
Maturity Model.

**Evidence.** 12 tests; the hypothesis checks are stated as heuristics, not guarantees.

## assessment — a ledger of graded judgements

**Problem.** Judgements live in prose: never re-derivable, comparable or diffable.

**Method.** Any claim gets the same discipline — evidence, competing hypotheses, two axes,
and an external truth that must be earned — and a persistent ledger.

**Evidence.** 12 tests. It consumes the shared core (`cryptonym.method`), so the method is
inherited, not reimplemented.

---

The sources are private; the method and the evidence are here — and I am glad to walk
through any of them.

*Every tool encodes a rule that stops you skipping the hard part: grade the claim, earn
the truth, state what would change your mind.*
