EcoVision-LoRA

🚧 Repository under active preparation 🚧
This repository is being finalized and cleaned. The full, fully reproducible pipeline will be ready within ~1 day.
Code structure, comments, and execution order are already stable, but some notebooks have not yet been re-run end-to-end.

notebooks/
│
├── 1a_shanghai_pipeline.ipynb      # Shanghai data preparation (independent)
├── 1b_vegas_pipeline.ipynb         # Las Vegas data preparation (independent)
├── 2_pretraining_tests.ipynb       # Backbone-only feature/regression tests
├── 3_cleanup_format_data.ipynb     # Data merging, cleaning, normalization
├── 4a_shanghai_training.ipynb      # LoRA fine-tuning (Shanghai)
├── 4b_vegas_training.ipynb         # LoRA fine-tuning (Las Vegas)

Execution Order (Important)

The notebooks must be run in the following order:
1.  Data preparation (independent):
        1a_shanghai_pipeline.ipynb
        1b_vegas_pipeline.ipynb
2.  Backbone-only experiments (after 1a & 1b):
        2_pretraining_tests.ipynb
3.  Data cleanup and formatting (after 2):
        3_cleanup_format_data.ipynb
4.  LoRA fine-tuning (independent, after 3):
    •    4a_shanghai_training.ipynb
    •    4b_vegas_training.ipynb

Initial Setup

1. Environment Requirements
    •    Python 3.10 (recommended). Python 3.12 is not supported due to dependency limitations (e.g., timm).
    •    PyTorch (with MPS support recommended for Apple Silicon)
    •    torchvision
    •    transformers
    •    timm
    •    numpy, pandas, scikit-learn
    •    xgboost
    •    geopandas, rasterio, shapely
    •    matplotlib, seaborn
    •    jupyter

⚠️ GPU is not required, but training is faster with GPU or Apple MPS.

⸻

2. Satellite Imagery (SpaceNet)

We use SpaceNet-2 imagery:
    •    AOI 4: Shanghai
    •    AOI 2: Las Vegas

Data is downloaded from official public SpaceNet S3 mirrors.

✔️ Download scripts are included directly inside the notebooks
(1a_shanghai_pipeline.ipynb and 1b_vegas_pipeline.ipynb)

No manual download is required for SpaceNet data. The data will be downloaded under 
the folder data/spacenet/

⸻

3. Nighttime Lights (Required Before Running)

⚠️ This step is done manually before running the pipeline.

I use:
    •    VIIRS VNL v21
    •    Year 2021 annual composite

Source
NASA Earth Observation (NEO) / NOAA VIIRS Nighttime Lights

🔗 Download URL:
https://eogdata.mines.edu/nighttime_light/annual/v20/2021/VNL_v2_npp_2021_global_vcmslcfg_c202203152300.average_masked.tif.gz

Reachable through:
https://eogdata.mines.edu/products/vnl/

Instructions
    1.    Create a free account (required as of today)
    2.    Download the VIIRS VNL v21 – Year 2021 composite
    3.    Place the downloaded files under:
    
data/night_lights/

The notebooks assume the nighttime lights data already exists locally and do not download it automatically.

⸻

Coordinate Systems

For accurate spatial overlap:
    •    Las Vegas → UTM Zone 11N
    •    Shanghai → UTM Zone 51N

Reprojection and cropping are handled inside the notebooks.

⸻

Training Details (Summary)
    •    Backbone: DINOv2 Vision Transformer (frozen)
    •    Regression loss: Mean Absolute Error (MAE)
    •    Optimizer: AdamW
    •    LoRA fine-tuning:
    •    Only LoRA adapters + regression head are trainable
    •    Backbone weights remain frozen
    •    Region-specific hyperparameters are used for Shanghai and Las Vegas
    •    Training performed on Apple Silicon (MPS backend)

⸻

Reproducibility Checklist
    •    Public datasets only (SpaceNet, VIIRS)
    •    Exact data sources documented
    •    Clear execution order across notebooks
    •    Random train/validation split (80/20)
    •    Metrics reported: MAE and R²
    •    Region-specific preprocessing disclosed
    •    Final end-to-end rerun (in progress)
    •    Cleaned comments and assertions (in progress)

⸻

Notes on Current Status
    •    Some notebooks have not yet been re-run after final cleanup.
    •    Comments and variable naming are being standardized.
    •    A finalized release with verified outputs will follow shortly.

⸻

Citation

If you use this repository, please cite this repository and/or the accompanying paper (to be added).

    


