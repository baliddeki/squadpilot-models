# SquadPilot model artifacts

Machine-generated data published by SquadPilot's private application
repository. This public repository contains no credentials or application
source; it exists so the mobile app can download model data without embedding
a GitHub token.

## Files

- `calibrated_parameters.json` contains the champion parameters used by the
  on-device projection engine. A challenger replaces it only after
  walk-forward evaluation, plausibility clamps and move limits.
- `ownership.json` is the small effective-ownership artifact consumed by the
  rank-aware decision layer.
- `model_bundle.json` contains diagnostic backend point distributions. Point
  projections remain on device until the backend simulator has its own
  held-out points promotion gate.

The first ML-capable app release does not need to wait for several live
gameweeks. Before the current season has five evaluation splits, challengers
are judged on the latest complete season. Once enough live evidence exists,
the pipeline switches evaluation sets automatically without another app-store
release.

Both consumers cache the last valid download and fall back to values compiled
into the app. `schema_version` prevents a newer incompatible bundle from being
partially applied by an older app.

Do not edit the JSON files or this README here. The scheduled publishing job
will replace them.
