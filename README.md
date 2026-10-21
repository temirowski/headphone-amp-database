# Headphone and Headphone Amplifier Specification Database

578 headphones and IEMs and 280 headphone sources (USB dongles, Bluetooth receivers, portable players, desktop amplifiers, DAC/amps and audio interfaces), built from manufacturer documents, with a provenance column for every figure.

- Browse it, with calculators built on top of it: **https://thephonepath.com/**
- Same files on the site: https://thephonepath.com/data/
- How it was built: https://thephonepath.com/how-this-data-was-built/

## Using this data? Credit it like this

The licence is CC BY 4.0, so anything you publish with these files (an article, a chart, a video, an app) needs a credit. Copy whichever fits:

- HTML: `Data: <a href="https://thephonepath.com/">The Phone Path</a> (CC BY 4.0)`
- Markdown: `Data: [The Phone Path](https://thephonepath.com/) (CC BY 4.0)`
- Plain text (video descriptions, print): `Data: The Phone Path, https://thephonepath.com/ (CC BY 4.0)`

If you write about one headphone or amplifier, linking its own page helps your readers more, for example https://thephonepath.com/headphones/sennheiser-hd-600/. Every row has its page in the `page` column.

## Why this exists

Headphone sensitivity is published in two incompatible units (dB/mW and dB/V), often without saying which, and amplifier power is usually published at one load only. Mixing them up shifts the result by 10·log10(1000/Z) dB: 15 dB for a 32 ohm headphone.

This dataset stores every headphone sensitivity in one unit (dB/mW) **and** keeps what the manufacturer actually printed next to it, plus a `basis` column saying how each number was obtained. Every amplifier power figure carries its own basis per load: published at that load, derived, or not yet traced.

## Files

| File | Rows | What it holds |
|---|---|---|
| `data/headphones.csv` | 578 | impedance, sensitivity in dB/mW and dB/V, power needed for 100 dB SPL, the value and unit as printed, basis, source URL |
| `data/sources.csv` | 280 | price, power into 32, 150, 300 and 600 ohms with a basis for each, output impedance, output noise, source URL |
| `data/brands.csv` | 156 | per manufacturer: how many figures they publish themselves |
| `data/*.json` | | same data with the licence and method note in the header |

Headphones by type: 345 over-ear / on-ear, 222 in-ear monitor, 11 electrostatic (needs an energiser).
Sources by type: 69 desktop amplifier, 65 audio interface, 51 USB dongle, 39 portable player, 34 desktop DAC/amp, 13 Bluetooth receiver, 9 portable amplifier.

## Headphones: the `basis` column

| Value | Models | Meaning |
|---|---|---|
| `published-per-mW` | 253 | the maker printed this figure per milliwatt; nothing was done to it |
| `converted-from-per-volt` | 122 | the maker printed it per volt; converted with the published impedance |
| `converted-from-stated-voltage` | 17 | the maker printed an SPL at one specific drive voltage; converted with the published impedance |
| `unit-not-stated-by-maker` | 148 | the maker printed a bare decibel figure with no reference; stored as printed, may be either reading |
| `not-published-by-maker` | 32 | no sensitivity figure in the maker's documentation; value carried over from the original build, untraced |
| `maker-document-unreachable` | 5 | the maker's document could not be reached, so the value is untraced rather than unpublished |
| `maker-reference-inconsistent` | 1 | the maker printed a reference its own datasheet contradicts; recorded as printed and flagged |

## Sources: basis per load

Across the four standard loads there are 346 published figures, 734 derived ones and 40 not yet traced to a document.

| Value | Figures | Meaning |
|---|---|---|
| `published` | 346 | the maker printed a power figure at exactly this load |
| `interpolated` | 159 | derived between two published loads, holding output voltage |
| `extrapolated` | 429 | derived beyond the published loads, holding output voltage (understates rather than flatters) |
| `voltage-bound` | 96 | an upper bound from a published maximum output voltage |
| `source-impedance` | 42 | derived from the published output voltage and output impedance |
| `recomputed` | 8 | recomputed from the maker's own published points |
| `unverified` | 40 | not yet traced to a manufacturer document |

## Columns

**headphones.csv**: `brand`, `model`, `type`, `impedance_ohm`, `published_impedance` (as printed), `sensitivity_db_mw`, `sensitivity_db_v`, `power_mw_at_100db` (power needed for a 100 dB SPL peak), `published_value`, `published_unit`, `published_condition` (sensitivity exactly as printed), `basis`, `source_url` (manufacturer document), `page`.

**sources.csv**: `name`, `type`, `tier`, `price_usd`, `power_mw_32_ohm` / `_150_ohm` / `_300_ohm` / `_600_ohm` each with a `basis_*` column, `output_impedance_ohm`, `output_noise_uv`, `rated_output`, `published_points` (the power figures exactly as printed), `confidence`, `source_url`, `page`.

**brands.csv**: counts per manufacturer of what they publish and what they leave out, and `share_from_maker_pct`.

## Formulas

- dB/mW = dB/V − 10·log10(1000 / Z)
- power for an SPL target (mW) = 10^((target − sensitivity_dB/mW) / 10)
- voltage (V) = sqrt(P[W] · Z)

## Two warnings

1. These are **manufacturer claims, not measurements**. Independent measurement beats any of this.
2. Rows marked `unit-not-stated-by-maker` are genuinely ambiguous. If a 10–15 dB error matters for your use, filter them out.

## How it was made

The spec sheets were read and collected with AI assistance, because it is several hundred documents. Every figure links to the manufacturer page it came from, and the build runs automated checks that recompute every number shown on the site from these files. If a figure disagrees with a manufacturer document, the document wins. Please open an issue.

## Licence

[Creative Commons Attribution 4.0](https://creativecommons.org/licenses/by/4.0/). Commercial use included. Attribution line:

> Data: The Phone Path (thephonepath.com), CC BY 4.0

Snapshot: 2026-10-10.
