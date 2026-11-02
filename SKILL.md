---
name: geoskill-soil-erosion-modeling
description: 'RUSLE 模型空间化估算土壤侵蚀模数：A = R×K×L×S×C×P，含降雨侵蚀力、土壤可蚀性、坡长坡度、植被覆盖与水保措施六因子。Computes soil erosion modulus with RUSLE (R K L S C P) spatialized factors. 输出侵蚀模数与强度分级 GeoTIFF。'
---

# 土壤侵蚀建模 | Soil Erosion Modeling

A = R × K × LS × C × P (t·ha⁻¹·yr⁻¹): R is estimated from mean annual rainfall × an intensity coefficient; K is looked up by soil texture (sand/loam/clay/silt, 0.05–0.42); LS uses the simplified Moore & Burch equation (flow accumulation × slope); C is derived from an exponential decay of NDVI (better vegetation cover, lower erosion, in [0.01, 1]); P is looked up by practice (no measures / contour tillage / terraces, 1.0/0.55/0.25). The erosion modulus is then classified into 5 grades from slight to extremely intense using the 500/2500/5000/8000 thresholds.

Use cases: soil and water conservation planning, erosion hotspot identification, and watershed sediment source analysis.

## Dependencies / 依赖

```bash
pip install numpy rasterio scipy
```

## Usage / 使用方法

### Example 1: Synthetic Rainfall/Terrain/Vegetation Scenario

```bash
python geoskill-soil-erosion-modeling.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./output
```

### 示例 2：真实 6 波段输入（precip,slope,ndvi,soil,practice,flow）

```bash
python geoskill-soil-erosion-modeling.py --input rusle_inputs.tif --output-dir ./real
```

### Example 3: 10 m Resolution Pixels

```bash
python geoskill-soil-erosion-modeling.py --bbox 116 39 117 40 --synthetic --cell-size 10 --output-dir ./fine
```

### Example 4: Different Regions

```bash
python geoskill-soil-erosion-modeling.py --bbox 121 31 122 32 --synthetic --output-dir ./shanghai
```

### Example 5: Silent Batch Mode

```bash
python geoskill-soil-erosion-modeling.py --bbox 113 23 114 24 --synthetic --quiet --output-dir ./batch
```

## Output / 输出

| File | Format | Description |
|---|---|---|
| `erosion_modulus.tif` | GeoTIFF (float32) | Erosion modulus (t·ha⁻¹·yr⁻¹), EPSG:4326 |
| `erosion_grade.tif` | GeoTIFF (float32) | Erosion intensity grade 0-4 |
| `rusle_params.json` | JSON | Factor means and per-grade pixel counts |
| `output-manifest.json` | JSON | Run manifest (input/output/QA/software versions) |

## Data Source / 数据源 / Source

Local GeoTIFF (optional 6-band factor input); the RUSLE factor equations and K/P reference values are taken from the USDA-published RUSLE handbook; synthetic mode is generated locally with no external data source.

## Privacy / 隐私声明 / Privacy

- Runs fully offline by default and makes no network requests
- `--synthetic` mode reads no external data
- All computation is done locally; no user data is uploaded

## License / License

MIT

---

> 中文完整版：需要时再读 [references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)，不要默认全量加载。
