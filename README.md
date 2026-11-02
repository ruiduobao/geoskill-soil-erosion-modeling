# Soil Erosion Modeling (geoskill-soil-erosion-modeling)

> Computes soil erosion modulus with RUSLE (R K L S C P) spatialized factors — rainfall erosivity, soil erodibility, slope length and steepness, vegetation cover, and conservation practices. Outputs erosion modulus and intensity-grade GeoTIFFs.

---

## 1. Overview

A = R × K × LS × C × P (t·ha⁻¹·yr⁻¹): R is estimated from mean annual rainfall × an intensity coefficient; K is looked up by soil texture (sand/loam/clay/silt, 0.05–0.42); LS uses the simplified Moore & Burch equation (flow accumulation × slope); C follows an NDVI exponential decay (better vegetation → less erosion, range [0.01, 1]); P is looked up by practice type (no measures / contour tillage / terracing: 1.0 / 0.55 / 0.25). The erosion modulus is then classified into 5 grades from slight to extremely severe using thresholds of 500/2500/5000/8000. Use cases: soil and water conservation planning, erosion hotspot identification and watershed sediment source analysis.

## 3. Quick Start

```bash
pip install -r requirements.txt
python geoskill-soil-erosion-modeling.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

## 4. CLI Parameters

Run `python <skill>.py --help` for the full list. Common parameters:

| Parameter | Type | Description |
|---|---|---|
| `--bbox` | `float[4]` | WGS84 bounding box `min_lon min_lat max_lon max_lat` |
| `--input` | `path` | Local input file (GeoJSON/GeoTIFF/etc.) |
| `--output-dir` | `path` | Output directory (default `./output`) |
| `--synthetic` | `flag` | Use synthetic data instead of real input |
| `--quiet` | `flag` | Suppress non-essential stdout |

## 5. Input / Output

| File | Format | Description |
|---|---|---|
| `erosion_modulus.tif` | GeoTIFF (float32) | Erosion modulus (t·ha⁻¹·yr⁻¹), EPSG:4326 |
| `erosion_grade.tif` | GeoTIFF (float32) | Erosion intensity grade 0-4 |
| `rusle_params.json` | JSON | Per-factor means and grade pixel counts |
| `output-manifest.json` | JSON | Run manifest (inputs/outputs/QA/software versions) |

## 6. Technical Principle

(see SKILL.md for details)

## 8. License

MIT License. See [`LICENSE`](./LICENSE) for full text.

---

> 中文完整版：[references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)
