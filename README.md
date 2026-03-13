# Multi-Platform Aerial Imagery Domain Generalization Dataset (Camera / UAV / Satellite)

This repository provides an integrated dataset for **domain generalization (DG)** research in **vehicle object detection** under **aerial / overhead views**. The dataset covers **three sensing platforms**—**traffic cameras**, **unmanned aerial vehicles (UAVs)**, and **satellites**—and is organized into **11 domains** defined by **weather, illumination, and scene** factors. It is designed for evaluating **single-source (single-domain) training** with **out-of-domain (OOD) testing**.

> ⚠️ This dataset is **integrated from multiple existing public datasets**. Please comply with the original licenses/terms of each source dataset and cite them as listed in the BibTeX section below.

---

## 1. Motivation and Research Scope

This dataset targets DG questions commonly encountered in real-world deployment:

- **Weather OOD**: train on normal clear conditions → test on rain/fog/snow  
- **Illumination OOD**: train on daytime → test on nighttime  
- **Scene OOD (Satellite)**: train on one satellite scene → test on other satellite scenes  
- **Device OOD**: train on one platform (camera/UAV/satellite) → test on other platforms

A key design principle is **“train on normal, easy-to-collect domains; test on abnormal/hard domains”**, reflecting practical data collection constraints and deployment needs.

---

## 2. Domain Definitions (11 Domains)

We define 11 domains, each representing a relatively consistent data distribution:

1. **Camera–Day–Clear**  
2. **Camera–Rain**  
3. **UAV–Day–Clear**  
4. **UAV–Night**  
5. **UAV–Fog**  
6. **UAV–Snow**  
7. **Satellite–Day–Clear–Residential**  
8. **Satellite–Day–Clear–Traffic**  
9. **Satellite–Day–Clear–Airport**  
10. **Satellite–Day–Clear–Port**  
11. **Satellite–Day–Clear–Natural**

---

## 3. Data Sources (Integrated From Public Datasets)

This integrated dataset is constructed from the following sources:

- **Traffic Camera (Domains 1–2)**  
  - Source: **AAU RainSnow** (Kaggle dataset): https://www.kaggle.com/datasets/aalborguniversity/aau-rainsnow

- **UAV (Domains 3–5: Day/Clear, Night, Fog)**  
  - Source: **[38] Sun et al., IEEE TCSVT 2022** (Drone-based RGB-Infrared cross-modality vehicle detection)

- **UAV (Domain 6: Snow)**  
  - Source: **[39] Mokayed et al., arXiv 2023** (Nordic Vehicle Dataset, NVD)

- **Satellite (Domains 7–11: Multi-scene)**  
  - Source: **xView** (Lam et al., arXiv 2018; overhead satellite imagery)

> Note: This repository unifies the **domain partitioning**, **data organization**, and **label mapping** across sources. The copyrights and licensing of the original images/annotations remain with the original dataset owners.

---

## 4. Task and Annotations

- **Task**: Object Detection  
- **Annotations**: Bounding boxes, inherited from the original datasets and normalized into a unified format  
- **Focus classes**: Vehicle-related categories (e.g., *vehicle/car/truck*). The final class mapping should follow the label definition in this repository (e.g., `meta/class_map.json`).

---

## 5. Recommended Directory Structure (Example)

You may organize the integrated dataset as follows (adapt to your framework, e.g., YOLO/MMDetection/Detectron2):


## 6. Experimental Settings (SDG / OOD Benchmark)

This dataset is designed for **Single-Domain Generalization (SDG)** evaluation: train on **one** source domain and test on one or more OOD target domains.

### 6.1 Weather OOD (Abnormal Weather)

Train on easy-to-collect clear conditions, test on adverse weather:

- **Camera Weather-OOD**: `Camera–Day–Clear → Camera–Rain`
- **UAV Weather-OOD**: `UAV–Day–Clear → UAV–Fog`
- **UAV Weather-OOD**: `UAV–Day–Clear → UAV–Snow`

### 6.2 Illumination OOD (Day → Night)

Train on daytime, test on nighttime:

- **UAV Illumination-OOD**: `UAV–Day–Clear → UAV–Night`

### 6.3 Scene OOD (Satellite, Multi-Scene)

Train on a stable, vehicle-rich satellite scene (recommended: **Traffic**), test on other satellite scenes:

- **Satellite Scene-OOD**:
  - `Satellite–Traffic → Satellite–Residential`
  - `Satellite–Traffic → Satellite–Airport`
  - `Satellite–Traffic → Satellite–Port`
  - `Satellite–Traffic → Satellite–Natural`

### 6.4 Device OOD (Cross-Platform)

Train on a normal domain from one platform and test on other platforms.

#### Normal-to-Normal (clean device shift)

- `Camera–Day–Clear → UAV–Day–Clear`
- `Camera–Day–Clear → Satellite–Residential / Traffic / Airport / Port / Natural`
- `UAV–Day–Clear → Camera–Day–Clear`
- `UAV–Day–Clear → Satellite–Residential / Traffic / Airport / Port / Natural`
- `Satellite–Traffic → Camera–Day–Clear`
- `Satellite–Traffic → UAV–Day–Clear`

#### Normal-to-Hard (deployment-like)

- `Camera–Day–Clear → UAV–Night / UAV–Fog / UAV–Snow`
- `UAV–Day–Clear → Camera–Rain`

---

## 7. Metrics (Recommended)

For each train→test setting, we recommend reporting:

- **mAP@[0.5:0.95]** (COCO-style)
- **mAP@0.5**
- **AP_vehicle** (vehicle-focused AP; for multi-class datasets, report vehicle-related AP separately)

For robust cross-domain comparison across many targets, we recommend aggregated metrics:

- **Mean-OOD**: average performance over a target suite  
- **Worst-k OOD**: mean of the bottom-*k* target performances (e.g., *k* = 3 or 5)  
- **Retention**: relative performance ratio vs. the in-domain upper bound  
- **Stability**: standard deviation across targets  

---

## 8. Acknowledgment & Citation (BibTeX)

If you use this integrated dataset, please cite the original datasets/papers below:

```bibtex
@misc{aau_rainsnow_kaggle,
  title        = {AAU RainSnow (Traffic Surveillance) Dataset},
  author       = {{Aalborg University}},
  year         = {2023},
  howpublished = {Kaggle dataset},
  url          = {https://www.kaggle.com/datasets/aalborguniversity/aau-rainsnow},
  note         = {Accessed: 2026-03-13}
}

@article{sun2022drone_rgb_ir_uav,
  title        = {Drone-Based RGB-Infrared Cross Modality Vehicle Detection via Uncertainty-Aware Learning},
  author       = {Sun, Y. and Cao, B. and Zhu, P. and Hu, Q.},
  journal      = {IEEE Transactions on Circuits and Systems for Video Technology},
  year         = {2022},
  volume       = {32},
  number       = {10},
  pages        = {6700--6713},
  month        = oct,
  note         = {Please verify DOI from IEEE Xplore for the final bibliography.}
}

@misc{mokayed2023nvd,
  title         = {Nordic Vehicle Dataset (NVD): Performance of vehicle detectors using newly captured NVD from UAV in different snowy weather conditions},
  author        = {Mokayed, H. and Nayebiastaneh, A. and De, K. and Sozos, S. and Hagner, O. and Backe, B.},
  year          = {2023},
  eprint        = {2304.14466},
  archivePrefix = {arXiv},
  primaryClass  = {cs.CV},
  url           = {https://arxiv.org/abs/2304.14466}
}

@misc{lam2018xview,
  title         = {xView: Objects in Context in Overhead Imagery},
  author        = {Lam, D. and Kuzma, R. and McGee, K. and Dooley, S. and Laielli, M. and Klaric, M. and Bulatov, Y. and McCord, B.},
  year          = {2018},
  eprint        = {1802.07856},
  archivePrefix = {arXiv},
  primaryClass  = {cs.CV},
  url           = {https://arxiv.org/abs/1802.07856}
}
```
## 9. License and Terms

- This repository provides the **integrated organization and domain partitioning** for DG research.
- The **original images and annotations** remain subject to the licenses/terms of their respective source datasets.
- If you plan to redistribute any portion of the original data, please ensure redistribution is permitted by **all** source licenses.

---

## 10. Contact

For questions about domain partitioning, label mapping, or benchmark protocols, please open an Issue on GitHub.
