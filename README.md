# KSuite — Product Engineering Case Study

KSuite is a local-first embroidery digitizing tool with a shared production engine, desktop editor, HTTP API, and browser studio.

## Why it is interesting

Embroidery software has a stricter definition of “works” than a screen preview. The output must respect physical dimensions, machine hoop limits, stitch budgets, trims, relocations, thread changes, and the topology of the original artwork.

## System design

```text
PNG / JPG / WEBP / SVG
        ↓
physical geometry + semantic regions
        ↓
typed embroidery intermediate representation
        ↓
stitch generation + deterministic routing
        ↓
production QA + PES read-back validation
```

The desktop app and web studio share the Python production pipeline. The browser can manage projects and previews, but it cannot claim production export when the service is disconnected.

## Engineering decisions

- Physical units are explicit from import through export.
- Needle stitches, travel, trims, and thread changes remain distinct commands.
- Alpha is authoritative; the engine does not invent a full-design base fill.
- Machine policy is data-backed by hoop profile.
- Density retries protect meaningful small details.
- CI covers engine/API behavior and the web surface before packaging.

## Repository tour

The full implementation is maintained in [jt-wilkinson/ksuite](https://github.com/jt-wilkinson/ksuite). Start with [`ENGINEERING.md`](https://github.com/jt-wilkinson/ksuite/blob/main/ENGINEERING.md) for the architecture contract and [`docs/engine-v2.md`](https://github.com/jt-wilkinson/ksuite/blob/main/docs/engine-v2.md) for production acceptance rules.

## Honest constraints

The asynchronous API job state is process-local, the macOS bundle requires signing and notarization for a public release, and packaging success does not replace a sew-out on the intended fabric, stabilizer, needle, and thread.

## Status

Active personal project. The case study is a concise public narrative; implementation details and generated artifacts remain in the main repository.
