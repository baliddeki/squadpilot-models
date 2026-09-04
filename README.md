# SquadPilot model bundles

Machine-generated projection parameters for the SquadPilot FPL app. **Nothing
here is hand-edited** — every file is overwritten by a scheduled job in the
app's own (private) repository.

This repository is public for one reason: a mobile app cannot hold a GitHub
token, so it cannot read a private repository. Only these generated files live
here; the app source does not.

## Files

| File | What it is |
|---|---|
| `calibrated_parameters.json` | Fitted values for constants the app would otherwise guess — minutes, per-90 rate priors, shrinkage strengths, fixture scaling, bonus divisors. |
| `model_bundle.json` | Per-player projections for the next few gameweeks, with a points distribution and effective ownership. |

## How the parameters are produced

Structural parameters are fitted on several completed seasons pooled together —
how many minutes a starter plays, how fast an observed start rate earns trust,
what a typical forward produces per 90. Those are properties of the
competition, and are better estimated from four seasons than from three
gameweeks.

Player form is **not** carried across seasons. Accumulation resets at every
season boundary, and FPL reassigns player ids each summer, so pooling on the
bare id would chain one footballer's record onto another's.

A refit is only published if it beats the parameters currently shipped, scored
walk-forward on data it did not train on. Early in a season, when the current
season is too short to tell a good model from a bad one, the comparison falls
back to the most recent complete season. Each bundle records the metrics that
justified it under `provenance`.

Every fitted value is also clamped to a plausible range and limited in how far
it may move per refit, so a bad week nudges the parameters rather than lurching
them.

## Consuming these

The app reads `calibrated_parameters.json` and falls back to constants compiled
into the binary if it is missing, stale, or unparseable. A failed download is
never a broken app — it is last week's parameters, or the built-in ones.

`schema_version` gates compatibility: a bundle newer than the reading code
understands is refused outright rather than half-applied.
