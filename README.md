# Cell Segmentation and Fragment Classification using Cellpose 4

*Date: June 30 2025*

This project performs automatic segmentation of cells in time-lapse microscopy images using **Cellpose 4 (SAM)**, and classifies each frame by:
- **Cell state**: Circular, Fixed, Dead
- **Fragments**: Four types of abnormal morphology

It calculates key metrics such as:
- Number of cells per frame
- Average area and brightness
- Flow magnitude and direction (from Cellpose flows)

---

## 📌 Objective

To analyze **cell seeding efficiency** and detect morphological changes over time using image segmentation and flow-based classification.

---

## 📂 Features

- ✅ **Cell segmentation** using Cellpose v4
- ✅ **Cell state classification** across time
- ✅ **Abnormal fragment detection** (4 fragment types)
- ✅ **Measurement outputs** per frame: cell count, area, brightness
- ✅ **AVI video generation** with masks and overlays
- ✅ **Exportable CSV reports**

---

## 🖼️ Sample Input

Multi-frame `.tif` microscopy file, e.g.:

```python
image_stack = imread("CM30.tif")
```

---

## 📊 Output

* `seeding_efficiency.csv`: per-frame classification (Circular, Fixed, Dead, Fragments)
* `frame_summary.csv`: number of cells, average area, brightness per frame
* `segmentation.avi`: color-coded AVI video with 4-class fragment masks
* `overlay_fragments.avi`: original image with segmentation mask overlay

---

## 📦 Dependencies

* Python 3.8+
* [Cellpose 4](https://github.com/MouseLand/cellpose)
* `numpy`, `scikit-image`, `opencv-python`, `matplotlib`, `pandas`

Install using:

```bash
pip install cellpose scikit-image opencv-python matplotlib pandas
```

---

## 🚀 Usage

Depending on the preference, you can check upload the project to Google Colab for better performance, or to clone the repository locally. Either way, the `projectPath` variable must point to the root directory of the project, ensuring that the project structure is respected, otherwise, files won't be read.

The computations takes a very long time, and a good computer is necessary for this tasks.

1. Clone the project, or upload to Google Drive
2. Check the project structure
3. Modify `projectPath` accordingly. If Google Colab is not used, ignore executing the google colab dependencies.
4. Run the notebook: `CM30_cellpose.ipynb`
5. Check the `/results` folder for outputs (AVI videos and CSV)

---

## 🧠 Classification Logic

### Cells states
**Cell states (based on frame range):**

* Frames 0 → N/3: Circular (pre-fixation)
* N/3 → 2N/3: Fixed
* 2N/3 → End: Dead (post-fixation)

### Fragment Classification Logic

The following is a heuristic-based classification logic used to classify segmented fragments into four biologically meaningful categories, based on morphological properties:

```python
if area < 40 and ecc > 0.9:
    cls = 0  # apoptotic
elif sol < 0.75:
    cls = 1  # necrotic
elif ecc > 0.9:
    cls = 2  # fixed
else:
    cls = 3  # regular round
```

#### 📋 Explanation

| Class | Condition                   | Biological Interpretation                               |
| ----- | --------------------------- | ------------------------------------------------------- |
| `0`   | `area < 40` and `ecc > 0.9` | **Apoptotic**: small and elongated fragments            |
| `1`   | `sol < 0.75`                | **Necrotic**: low solidity, irregular/loss of integrity |
| `2`   | `ecc > 0.9`                 | **Fixed**: elongated or spindle-shaped cells            |
| `3`   | *(default)*                 | **Regular round**: healthy-looking circular cells       |

#### 🔍 Shape Descriptors Used

* **Area**: Number of pixels in the object — small area often indicates cell debris.

* **Eccentricity**: Measures elongation (0 = perfect circle, 1 = a line).

* **Solidity**: Ratio of contour area to convex hull — lower values mean irregular shapes.

#### 🧬 Biological Rationale

* **Apoptotic cells** shrink and may appear both **small** and **elongated**.
* **Necrotic cells** often rupture, resulting in **irregular, concave shapes**.
* **Fixed cells** tend to **stretch and elongate**, especially after attachment.
* **Regular cells** remain **round and symmetric**, indicating healthy morphology.

You can adjust these thresholds based on dataset variability or add more shape descriptors (e.g., circularity, aspect ratio) if needed.

---

## 📈 Example Result

| Frame | Circular | Fixed | Dead | Fragments |
| ----- | -------- | ----- | ---- | --------- |
| 0     | 1115     | 0     | 0    | 889       |
| 15    | 0        | 728   | 0    | 942       |
| 30    | 0        | 0     | 837  | 902       |

---

## 📄 License

This project is licensed under the GNU Affero General Public License v3.0 - see the [LICENSE](LICENSE) file for details.

---

## 🤝 Acknowledgments
This project is for academic/research use. Original segmentation model by the [Cellpose team](https://www.cellpose.org/).
