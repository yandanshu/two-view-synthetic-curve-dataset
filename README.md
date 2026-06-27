# Two-View Synthetic Curve Dataset

A **two-view synthetic curve dataset** for evaluating curve reconstruction and triangulation algorithms. This repository provides **384** pre-generated stereo cases, **7 scaled real-world image pairs** from ETH3D, and a Jupyter notebook to reproduce or extend the synthetic dataset.

---

## Overview

Under a known stereo camera setup, this dataset samples multiple parametric **3D space curves**. For each configuration it provides:

- **3D curve ground-truth point clouds** (`.ply`)
- **Left / right camera projected views** (pseudo images, `.png`)

It is intended for quantitative evaluation and comparison of curve reconstruction, two-view geometry, and triangulation methods.

---

## Repository Contents

| Item | Description |
|------|-------------|
| `pointclouds/` | 384 ground-truth point cloud files |
| `images/` | 384 stereo pseudo-image pairs (each with `1.png` and `2.png`) |
| `ETH3D_image_pairs/` | 7 real stereo image pairs from ETH3D (scaled to 0.2×) |
| `generate_dataset.ipynb` | Jupyter notebook to regenerate or customize the synthetic dataset |

---

## Directory Layout

```
two-view-synthetic-curve-dataset/
├── generate_dataset.ipynb
├── pointclouds/
│   ├── 01_0_0_0.ply
│   └── ...
├── images/
│   ├── 01_0_0_0/
│   │   ├── 1.png    # left camera
│   │   └── 2.png    # right camera
│   └── ...
└── ETH3D_image_pairs/
    ├── courtyard/
    └── ...
```

---

## Dataset Scale

There are **8 × 3 × 4 × 4 = 384** cases. Each case is named:

```
{curve_id}_{param}_{trans}_{rot}
```

Example: `01_0_0_0` denotes curve type 01, the first geometry parameter set, no extra translation, and no extra rotation.

| Dimension | Values | Description |
|-----------|--------|-------------|
| Curve type `curve_id` | `01`–`08` | 8 types of 3D curves |
| Geometry `param` | `0`–`2` | 3 parameter sets per curve type |
| Translation `trans` | `0`–`3` | none / +X / +Y / +Z |
| Rotation `rot` | `0`–`3` | none / 45° about X / Y / Z |

### Curve Types

| ID | Type |
|----|------|
| 01 | Circle |
| 02 | Ellipse |
| 03 | Parabola |
| 04 | Hyperbola |
| 05 | Planar Archimedean spiral |
| 06 | B-spline |
| 07 | Composite curve |
| 08 | Spatial spiral |

---

## Camera and Sampling Parameters

| Parameter | Default |
|-----------|---------|
| Image resolution | 1024 × 1024 |
| Focal length fx, fy | 512 |
| Principal point cx, cy | 512, 512 |
| Baseline | 10 |
| Target distance | \(2^{n_d} \times\) baseline (default \(n_d=2\), i.e. 40) |
| Points per curve | 50,000 |

The left and right cameras are placed symmetrically about the X axis, with optical axes converging on a common target point. Pseudo images draw white pixels at projected locations; noise is disabled by default (`noise_std=0`).

---

## ETH3D Real Image Pairs (`ETH3D_image_pairs/`)

In addition to the synthetic data, this repository includes **7 real stereo image pairs** selected from **[ETH3D](https://www.eth3d.net/)** for experiments on real-world imagery.

- Images are **scaled to 0.2×** (20% of the original width and height) to reduce file size and simplify download and experimentation.
- Each pair consists of two JPG files; subdirectory names correspond to ETH3D scene names.

### Directory Layout

```
ETH3D_image_pairs/
├── courtyard/
│   ├── ETH3D__DSC_0306.JPG
│   └── ETH3D__DSC_0307.JPG
├── facade/
│   ├── ETH3D__DSC_0361.JPG
│   └── ETH3D__DSC_0362.JPG
├── kicker/
│   ├── ETH3D__DSC_6512.JPG
│   └── ETH3D__DSC_6513.JPG
├── office/
│   ├── ETH3D__DSC_0219.JPG
│   └── ETH3D__DSC_0220.JPG
├── relief/
│   ├── ETH3D__DSC_0454.JPG
│   └── ETH3D__DSC_0456.JPG
├── relief2/
│   ├── ETH3D__DSC_0484.JPG
│   └── ETH3D__DSC_0485.JPG
└── terrace/
    ├── ETH3D__DSC_0267.JPG
    └── ETH3D__DSC_0268.JPG
```

### Usage Notes

- Images in this repository are downscaled. **If you use official ETH3D calibration, scale intrinsics accordingly** (e.g., multiply focal length and principal point by 0.2), or re-estimate calibration on the resized images.
- Use and redistribution of ETH3D data must comply with the [official ETH3D license](https://www.eth3d.net/). Please cite the ETH3D paper when using `ETH3D_image_pairs/` in publications.

### ETH3D Citation

> T. Schöps, et al. *A Multi-View Stereo Benchmark with High-Resolution Images and Multi-Camera Videos.* CVPR 2017.

---

## Usage

### Use the provided dataset directly

- Ground-truth point cloud: `pointclouds/{case_name}.ply`
- Left / right views: `images/{case_name}/1.png`, `2.png`
- ETH3D real pairs: `ETH3D_image_pairs/{scene_name}/` (two `.JPG` files per scene)

### Regenerate or extend the dataset

1. Install dependencies (Python 3.8 and a virtual environment are recommended):

```bash
pip install numpy==1.24.4 opencv-python==4.8.1.78 matplotlib==3.7.5 \
            plotly==5.22.0 open3d==0.13.0 "notebook==6.5.7" ipykernel scipy
```

2. Open `generate_dataset.ipynb` and run the cells in order.

3. Modify `root_path`, curve parameters, or camera settings in the notebook to produce custom data.

---

## Notes

- Regenerating all 384 cases can take a long time; you may use the bundled `pointclouds/` and `images/` directly.
- The batch generation cell displays preview figures; comment out the plotting code if you only need output files.
- Ensure `root_path` points to a directory where you have write permission.
