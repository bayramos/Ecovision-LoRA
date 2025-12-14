# EcoVision-LoRA

This repository contains the code and notebooks for the project:

**Predicting Urban Economic Intensity from High-Resolution Satellite Imagery Using Vision Transformer Features and Nighttime Lights**

🚧 **Repository Under Preparation** 🚧  
This repository is actively being prepared and finalized.  
Notebooks, documentation, and instructions are being added, cleaned and finalized.
The full documentation, cleaned code, and usage instructions will be available shortly.

Please revisit within 24 hours.

---

## Project Overview

This project studies the relationship between:
- High-resolution RGB satellite imagery (SpaceNet)
- Built environment structure
- Nighttime light intensity (VIIRS)

I use:
- Frozen and fine-tuned **DINOv2 Vision Transformer** features
- Classical regression baselines
- **LoRA-based parameter-efficient fine-tuning**

Experiments are conducted on:
- **Shanghai (AOI 4)**
- **Las Vegas (AOI 2)**

Nighttime lights are used as a proxy for urban economic intensity.

---

## Repository Structure (current - being developed)
Ecovision-LoRA/
│
├── notebooks/
│   ├── _VIIRS_reprojection.ipynb
│   ├── _dataretrieval.ipynb
│   ├── _extract_csv_list.ipynb
│   ├── _shanghai_training.ipynb
│   └── _vegas_training.ipynb
│
└── README.md

---

## Data Sources

- **Satellite Imagery:** SpaceNet-2 (Shanghai AOI 4, Las Vegas AOI 2)
- **Nighttime Lights:** VIIRS VNL v21 (Year 2020 composite)
- **Projections:** UTM Zone 11N (Las Vegas), UTM Zone 51N (Shanghai)

---

## Notes

- This repository is being actively updated.
- Code cleanup, environment setup instructions, and trained model checkpoints will be added.
- Results and methodology are described in the project report.

---

## License

To be added.
