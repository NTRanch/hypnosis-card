# hypnosis-card — measured specification

The clean-room study record for the Torogao remake of **Hypnosis
Card**, by Nice Thoughts Ranch. Two builds of the original were
studied as a version pair — v1 (Unity 2019.4.40f1, Mono) and v2
(Unity 2021.3.13f1, Mono). This repository currently holds the
**measured specification only — the systems build has not started.**

## Contents

- `hypnosis-card-dynamics.md` — the S3 dynamics spec from the
  version-pair diff. Headline fact: the card-effect table is keyed by
  **name, never by positional ID** — of the v1 effect names, 89
  survive into v2, 17 are added, and 83 are renumbered, so any
  positional mapping silently corrupts the data.
- `hypnosis-card-animation.md` — the S2b animation note for the pair.

## Method

Clean-room: constants and behaviour were measured from the studied
builds; no assets, text, or code from the originals are included or
will ship. When the build starts it will follow the programme's
dual-target law — native Linux and Android from one crate plus its
data file, with a deterministic trajectory fixture shared by both
targets.

The studied game remains the property of its respective developer;
this repository is an independent study and reimplementation record.

## License

BSD 2-Clause. See `LICENSE`.
