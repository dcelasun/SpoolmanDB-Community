# Upstream imports (no ID changes) — 2026-11-16

## Baseline and method

Starting Community commit: `63c67d4` (clean `main`). Upstream: Donkie/SpoolmanDB `main` at `3cf89fbb291865a80dd6b857396e963b9e15531c` (2026-11-15). This covers every upstream merge since the [2026-10-30 reconciliation](2026-10-30-reconciliation.md) (`c67dcd85`). It also covers three open upstream PRs, reviewed at their head SHAs: Donkie/SpoolmanDB#512 `e322c009`, #531 `374784ed` and #532 `a5b86712`.

For each upstream PR, earlier Community decisions in `docs/audits/` were checked first; Donkie/SpoolmanDB#500, #501 and #502 were already decided on 2026-11-07, and those decisions stand. Then the before/after source JSON was expanded with `scripts/compile_filaments.py`. Every added or changed record was compared with Community in two ways:

1. Exact ID and the `reconcile_upstream.py` candidate key.
2. A looser check: same manufacturer, same material and same colour name, in any product line.

Only additions and value corrections that leave existing IDs unchanged are applied here. Renames that change IDs (the queengooborg cleanups and the spelling fixes inside data PRs) wait for the rename-retirement process proposed in Icezaza2543/SpoolmanDB-Community#72.

Results:

| Check | Result |
|---|---|
| New public IDs | 431 |
| Changed, rekeyed or removed historical IDs | 0 |
| Duplicate candidates | 0 unresolved (the 1 existing candidate is a reviewed non-duplicate) |
| `validate.py --strict` | Pass |
| `pytest` | 298 passed |
| Stable Spoolman contract | Pass |
| Baseline check (`compile_id_baseline.py --strict --base-ref 63c67d4`) | Pass |

## Decisions per upstream PR

| Upstream PR | Decision | New IDs | Notes |
|---|---|---:|---|
| #506 Tinmorry TPU 95A Black hex | **Applied** | 0 | `3E413F` → `000000`. The Community value was the old upstream value. The PR says the earlier hex showed the unprinted filament. |
| #505 Extrudr PLA NX2 Matt | **Applied** | 56 | New 1 kg / 1.75 mm group copied as-is. It also gets the line's existing `country_of_origin` and `tds_url`, which are line-level. Hexes are converted from manufacturer RAL numbers ([source](https://extrudr.com/en/de/products/pla-nx2-matt/)). |
| #500 Deeplee PLA+ High Speed 2.0 | **No change (blocked)** | 0 | Still blocked under the [2026-11-07 decision](2026-11-07-release-catalog.md#deeplee--block_evidence): no manufacturer evidence for this SKU, its density or its plastic spool and 113 g tare. The upstream PR adds only a third-party profile. |
| #501 Fusion PCTG | **No change (blocked)** | 0 | Still blocked under the [2026-11-07 decision](2026-11-07-release-catalog.md#fusion--block_evidence): density, tare, hex and print values come only from third-party profiles. Also, the existing Community PCTG line lives under "Fusion Filaments", not "Fusion". |
| #503 Polymaker Panchroma | **Partially applied** | 1 | Panchroma Matte: 37 hexes updated to the manufacturer's published values ([source](https://shop.polymaker.com/en-eu/products/matte-pla)). Electric Magenta added as a **refill-only** group (`is_refill: true`) because it is published only on the refill variant. The removal of "Formerly …" from names is deferred to the rename process. Dual Matte colours and hexes were already in Community. |
| #504 Numakers PLA+ | **Partially applied** | 8 | 32 hexes updated (the Community values were the old upstream values). Eight missing colours added. Spelling renames (Flourescent → Fluorescent, Lavendar → Lavender, Grey → Gray, Fuscia/Magenta → Magenta, Cool (Lithophane) White → Cool White) are deferred to the rename process. |
| #529 Bambu Lab PLA Palette, TPU-75A | **Applied, tare left out** | 15 | Both products confirmed as current Bambu products: the PLA Palette CMYKW five-spool kit, and TPU 75A at 2.85 mm for the H3C. See [3DJake TPU 75A](https://www.3djake.com/bambu-lab/tpu-75a-pink) and [ALL3DP](https://all3dp.com/4/bambu-labs-new-tpu-75a-is-ultra-soft-prints-fast-brings-back-2-85-mm-filament/). The PR's 250 g tare and spool type have no source, so they are left out. Note that adding a spool type later will change these IDs (`_n` suffix). New material `TPU-75A` (density 1.17) added to `materials.json`. |
| #508 NOVTAU | **Applied** | 243 | New manufacturer file, copied as-is. Data comes from the manufacturer's own product pages and in-house measurements, per the PR. |
| #512 Voxelab (open) | **Partially applied** | 28 | Colours missing from the existing Community `High Speed` PLA+ and PLA groups were added to those groups. Upstream lists Abyssal Red under PLA with a different hex, while Community lists it under PLA+, so that colour is left out as a conflict. PLA+, PETG-CF and ASA-CF were already present. Source: Amazon listings in the PR. |
| #531 Giantarm (open) | **Applied** | 80 | Fourteen colours added to the existing `Giantarm PLA` group. The Red placeholder `FF0000` was replaced with the listed `C41D26`. New Silk PLA, PETG and TPU-95A groups use upstream naming. No tare information is available. Source: Amazon listings in the PR. |
| #532 Overture Easy (open) | **No change** | 0 | All 28 records already exist as Community `PLA Easy` / `PLA Easy Glow`. |
| #511 Hatchbox ABS Black | **No change** | 0 | Already present as `hatchbox_abs_absblack_1000_175_c`. |
| #516 RatRig field fix | **No change** | 0 | Community already has the corrected `spool_type`, with the same IDs. |
| #507 ELEGOO Marble colours | **No change (conflict)** | 0 | Both colours exist. The Community hexes came from the Open Filament Database, the upstream hexes from 3dfilamentprofiles. Neither is a manufacturer source, so the Community values are kept. |
| #502 Das Filament | **No change** | 0 | Matches the [2026-11-07 decision](2026-11-07-release-catalog.md#das-filament--partial-safe-addition): the Community 1 kg variants and the TPU V2 Flexibel line already cover this. The upstream tare of 214 g has no measurement evidence and does not identify the spool type. |
| #509, #514, #519–#524, #536 and open #515, #517, #518, #525–#527 | **Deferred** | — | Name and material cleanups that change IDs, file reformatting, linter and docs. These wait for the rename-retirement process (Icezaza2543/SpoolmanDB-Community#72). Any field fixes inside them that leave IDs unchanged will be handled with those PRs. |

## Follow-ups

- The other Polymaker Panchroma Matte groups (`Army/Muted/Pastel Panchroma Matte PLA`) still carry the older hexes. Aligning them needs a separate review.
- Bambu PLA Palette and TPU-75A are missing tare and spool type until a measurement or manufacturer source is found.
