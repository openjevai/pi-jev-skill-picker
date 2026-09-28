# OpenJEV Support

This fork adds optional [OpenJEV](https://openjev.sh) support alongside the original [TypeSafe](https://typesafe.ai) integration. TypeSafe remains the default; anyone with a TypeSafe key sees zero behaviour change.

## What was added

- **`extensions/jev.ts`** — `OPENJEV_ENDPOINT` and `OPENJEV_MODEL` constants; `provider` field on `JevConfig`; provider selection logic in `loadConfig` (reads `OPENJEV_API_KEY` and `JEV_PROVIDER`); updated error messages to mention both providers.
- **`extensions/skill-jev.ts`** — updated fallback error message and tool description to be provider-neutral.
- **`test/jev.test.ts`** — updated the "no API key" test regex to match the new error text.
- **`README.md`** — OpenJEV note after the intro; configuration table updated with `OPENJEV_API_KEY`, `JEV_PROVIDER`, and OpenJEV defaults.
- **`package.json`** — added `openjev` keyword.

## Provider selection rule

1. **Explicit choice wins**: `JEV_PROVIDER=openjev` (env) or `"provider": "openjev"` (config file).
2. **Otherwise, if `TYPESAFE_API_KEY` is set** → TypeSafe (unchanged default).
3. **Otherwise, if only `OPENJEV_API_KEY` is set** → OpenJEV.

When OpenJEV is selected, the endpoint defaults to `https://api.openjev.sh/v1/systemone`, the model defaults to `openjev`, and the API key is read from `OPENJEV_API_KEY` (env) or `openjevApiKey` (config file). `PI_SKILL_JEV_MODEL` and `PI_SKILL_JEV_ENDPOINT` still override the defaults if set.

## How to configure

Set `OPENJEV_API_KEY` in the environment (key from https://openjev.sh/dashboard), or add `"openjevApiKey"` to `skill-jev.json`. If both TypeSafe and OpenJEV keys are present, TypeSafe is used unless `JEV_PROVIDER=openjev` is set.

## How it was verified

- A live POST to `https://api.openjev.sh/v1/systemone` with model `openjev`, state `ping`, and one noul question returned HTTP 200.
- `grep` confirmed no hardcoded `api.typesafe.ai` default was introduced — the original TypeSafe endpoint constant remains as the TypeSafe default, and OpenJEV uses its own endpoint.

## Upstream

Original project: https://github.com/safzanpirani/pi-jev-skill-picker by @safzanpirani (MIT license).
