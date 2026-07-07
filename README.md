# OlmoEarth Workshop — Limpopo dry-season irrigation mapping

Ai2 **OlmoEarth** fine-tuning workshop use case (IWMI).

Fine-tune the OlmoEarth foundation model to map **monthly dry-season (May–Sep) irrigated
areas at 10 m** in the **Limpopo River Basin** from **Sentinel-1 + Sentinel-2**. This is the
deep-learning successor to our published Random Forest system ([Kiala et al. 2026](https://doi.org/10.1007/s43832-026-00360-z);
RF baseline OA 0.80 / κ 0.60). Best OlmoEarth model: **OA 0.874 / κ 0.748** (spatial CV on 190 field points).

## Open in Colab

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ZoloKiala/OlmoEarth_workshop/blob/main/OlmoEarth_Limpopo_Irrigation.ipynb)

## Contents

| File | What it is |
|---|---|
| `OlmoEarth_Limpopo_Irrigation.ipynb` | Colab-ready notebook: build labels → extract S1+S2 patches → embeddings quick-check → ODC-pretrain→field fine-tune → spatial-CV eval → predict |
| `data/field_points.geojson` | 190 ground-truthed field points (validation set) |
| `data/train_odc_2024dry_hiconf.geojson` | ODC-distilled high-confidence weak labels (pretraining set) |

## How to run (Colab)

1. Open the notebook (badge above, or *File → Open notebook → GitHub*).
2. **Runtime → Change runtime type → GPU** (T4 works; A100 faster for the LARGE fine-tune).
3. Run top-to-bottom. In the **data-setup cell (2a)** either upload the two files from `data/`,
   or clone this repo in Colab and point at `data/`:
   ```python
   !git clone https://github.com/ZoloKiala/OlmoEarth_workshop.git
   # then set:  DATA = Path('OlmoEarth_workshop/data')  (and skip files.upload)
   ```

Imagery is pulled live from Microsoft Planetary Computer — **no Earth Engine / credentials, no
large local files**. The OlmoEarth-LARGE weights download automatically from Hugging Face.

## Label schema (per OlmoEarth docs)

Point GeoJSON with **Location + Time + Target**. Target is Boolean `irrigated` (0 = non-irrigated,
1 = irrigated), with an int `oe_labels.category` mirror and a mid-month `date`.
See the [labeled-data requirements](https://docs.olmoearth.allenai.org/model-fine-tuning/#labeled-data-requirements)
and [training-data considerations](https://docs.olmoearth.allenai.org/training-data-considerations/).

## Live demo & models

- **App / API:** https://iwmihq-limpopo-irrigation-mapper.hf.space  (`/api`, `/docs`)
- **Model weights:** https://huggingface.co/IWMIHQ/limpopo-irrigation-models
