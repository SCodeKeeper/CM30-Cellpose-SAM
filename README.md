# Cell Segmentation and Fragment Classification using Cellpose 4

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
image_stack = imread("Hela_CM30.tif")
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

1. Place your `.tif` file in the root or data folder
2. Run the notebook: `25_cellpose_sam_spheres.ipynb`
3. Check the `/results` folder for outputs (videos and CSV)

---

## 🧠 Classification Logic

**Cell states (based on frame range):**

* Frames 0 → N/3: Circular (pre-fixation)
* N/3 → 2N/3: Fixed
* 2N/3 → End: Dead (post-fixation)

**Fragments (4 types):**

* Small area
* High eccentricity
* Low solidity
* Irregular shape

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
