EcoVision-LoRA

Predicting Urban Economic Intensity from High-Resolution Satellite Imagery using Vision Transformers and Nighttime Lights

This repository contains a complete, reproducible pipeline for estimating urban economic intensity proxies from satellite imagery. We combine high-resolution SpaceNet aerial imagery with VIIRS nighttime lights and Vision Transformer features, and apply parameter-efficient LoRA fine-tuning for regional adaptation.

The pipeline has been fully executed end-to-end and all notebooks run without errors.

⸻

Repository Structure

notebooks/
│
├── 1a_shanghai_pipeline.ipynb      # Shanghai data preparation (independent)
├── 1b_vegas_pipeline.ipynb         # Las Vegas data preparation (independent)
├── 2_pretraining_tests.ipynb       # Backbone-only feature extraction & regression
├── 3_cleanup_format_data.ipynb     # Data merging, cleaning, normalization
├── 4a_shanghai_training.ipynb      # LoRA fine-tuning (Shanghai)
├── 4b_vegas_training.ipynb         # LoRA fine-tuning (Las Vegas)


⸻

Execution Order (Important)

The notebooks must be run in the following order:

1. Data Preparation (independent)
    •    1a_shanghai_pipeline.ipynb
    •    1b_vegas_pipeline.ipynb

2. Backbone-Only Experiments (after 1a & 1b)
    •    2_pretraining_tests.ipynb

3. Data Cleanup & Formatting (after 2)
    •    3_cleanup_format_data.ipynb

4. LoRA Fine-Tuning (independent, after 3)
    •    4a_shanghai_training.ipynb
    •    4b_vegas_training.ipynb

⸻

Initial Setup

1. Environment Requirements
    •    Python 3.10 (recommended)
    •    Python ≥3.11 is not supported by several dependencies used here
    •    PyTorch (Apple Silicon MPS supported)
    •    torchvision
    •    transformers
    •    timm
    •    numpy, pandas
    •    scikit-learn
    •    xgboost
    •    geopandas
    •    rasterio
    •    shapely
    •    matplotlib, seaborn
    •    jupyter / jupyterlab
    •    awscli (for SpaceNet download)

⚠️ GPU is not required, but training is faster with GPU or Apple MPS.

⸻

2. DINOv2 Installation (Required)

DINOv2 is installed directly from the official Facebook Research repository.

Run inside your Python environment:

git clone https://github.com/facebookresearch/dinov2.git
cd dinov2
pip install -e .

xFormers is optional. Warnings about missing xFormers are expected and do not affect correctness.

⸻

Data Sources

3. Satellite Imagery (SpaceNet)

We use SpaceNet-2 imagery:
    •    AOI 4: Shanghai (4,582 tiles)
    •    AOI 2: Las Vegas (3,850 tiles)

Data is hosted on public AWS S3 mirrors.

Shanghai data is downloaded automatically inside:
    •    1a_shanghai_pipeline.ipynb

Las Vegas metadata is handled in the pipeline, but two large directories and a file are best downloaded manually via terminal (recommended to avoid notebook interruptions).

Recommended (Terminal Download for Las Vegas)

aws s3 sync s3://spacenet-dataset/spacenet/SN2_buildings/train/AOI_2_Vegas/PS-RGB/ \
    data/spacenet/AOI_2_Vegas/PS-RGB/ --no-sign-request

aws s3 sync s3://spacenet-dataset/spacenet/SN2_buildings/train/AOI_2_Vegas/geojson_buildings/ \
    data/spacenet/AOI_2_Vegas/geojson_buildings/ --no-sign-request

aws s3 sync s3://spacenet-dataset/spacenet/SN2_buildings/train/AOI_2_Vegas/ \
    data/spacenet/AOI_2_Vegas/ --no-sign-request

hese commands are also included (optionally) inside 1b_vegas_pipeline.ipynb, but terminal execution is more reliable.

All SpaceNet data is stored under:

data/spacenet/

⸻

4. Nighttime Lights (Required Before Running)

⚠️ This step must be done manually before running the notebooks.

We use:
    •    VIIRS VNL v21
    •    Year 2021 annual composite

Source
NASA / NOAA Earth Observation Group (EOG)

🔗 Direct download:
https://eogdata.mines.edu/nighttime_light/annual/v20/2021/VNL_v2_npp_2021_global_vcmslcfg_c202203152300.average_masked.tif.gz

Homepage:
https://eogdata.mines.edu/products/vnl/

Instructions
    1.    Create a free account on the EOG website
    2.    Download the VIIRS annual composite
    3.    Place the file under:
    
data/night_lights/

The notebooks assume the nighttime lights data already exists locally and do not download it automatically.

⸻

Coordinate Systems

For accurate spatial overlap:
    •    Las Vegas → UTM Zone 11N
    •    Shanghai → UTM Zone 51N

Reprojection, cropping and aggregation are handled inside the notebooks.

⸻

Training Details (Summary)
    •    Backbone: DINOv2 Vision Transformer (frozen)
    •    Target: mean nighttime radiance (VIIRS DN)
    •    Loss: Mean Absolute Error (MAE)
    •    Optimizer: AdamW
    •    LoRA fine-tuning:
    •    Only LoRA adapters + regression head are trainable
    •    Backbone weights remain frozen
    •    Region-specific hyperparameters used for Shanghai and Las Vegas
    •    Training performed on Apple Silicon (MPS backend)

⸻

Reproducibility Checklist
    •    Public datasets only (SpaceNet, VIIRS)
    •    Exact data sources documented
    •    Clear execution order across notebooks
    •    Random train/validation split (80/20)
    •    Metrics reported: MAE and R²
    •    Region-specific preprocessing disclosed
    •    Fully rerunnable end-to-end pipeline
    •    Deterministic preprocessing and logging

⸻

Citation

If you use this repository, please cite this codebase and/or the accompanying paper (to be provided)
