# Enhancing Nighttime Road Images using Adaptive Histogram Equalization (CLAHE)

This project improves nighttime road images using **Contrast Limited Adaptive Histogram Equalization (CLAHE)** to boost visibility and safety. It measures brightness and contrast before and after enhancement and generates comparison results.

---

## Overview
Driving at night can be tough due to **low light and poor contrast**. This project uses **OpenCV's CLAHE** to enhance low-light images. This makes lane markings, road signs, and other objects clearer.

**Features:**
- Measures brightness and contrast before and after enhancement.
- Provides side-by-side comparison.
- Saves the enhanced image to a file.

---

## Requirements
step1:mkdir -p ~/Desktop/NightVision
cd ~/Desktop/NightVision
pwd
step2:sudo apt update
sudo apt install -y python3 python3-pip wget
python3 -m pip install --upgrade pip
step3:pip3 install numpy matplotlib opencv-python
step5:wget https://images.unsplash.com/photo-1501594907352-04cda38ebc29 -O night_road.jpg
ls -l
step6:python3 night_vision.py
step7:xdg-open comparison.png >/dev/null 2>&1 &

### Project Files
```
.
├── night_road.jpg          # Input image (your nighttime road photo)
├── comparison.png          # Comparison of original and enhanced (generated)
├── enhanced_road.jpg       # Enhanced output (generated)
├── enhance_night_road.py   # Main Python script
└── README.md               # Project documentation
```
### Code
```python
cat > night_vision.py <<'PY'
#!/usr/bin/env python3
import os, sys
import cv2
import numpy as np
import matplotlib
matplotlib.use('Agg')          # no GUI needed (prevents VM crashes)
import matplotlib.pyplot as plt

# --- image path: from CLI arg or default file in folder ---
IMAGE_PATH = sys.argv[1] if len(sys.argv) > 1 else "night_road.jpg"
if not os.path.exists(IMAGE_PATH):
    print(f"ERROR: '{IMAGE_PATH}' not found in {os.getcwd()}")
    sys.exit(1)

img = cv2.imread(IMAGE_PATH)
if img is None:
    print(f"ERROR: OpenCV could not read '{IMAGE_PATH}'.")
    sys.exit(1)

# to grayscale
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

# stats
def stats(x): return float(np.mean(x)), float(np.std(x))
b_before, c_before = stats(gray)

# CLAHE enhancement
clahe = cv2.createCLAHE(clipLimit=3.0, tileGridSize=(8,8))
enhanced = clahe.apply(gray)
b_after, c_after = stats(enhanced)

# save outputs
out_enhanced = "enhanced_road.jpg"
out_compare  = "comparison.png"
cv2.imwrite(out_enhanced, enhanced)

# side-by-side figure (saved to file)
fig, ax = plt.subplots(1,2, figsize=(12,6))
ax[0].imshow(gray, cmap='gray')
ax[0].set_title(f'Original\nBrightness:{b_before:.1f}  Contrast:{c_before:.1f}')
ax[0].axis('off')
ax[1].imshow(enhanced, cmap='gray')
ax[1].set_title(f'Enhanced (CLAHE)\nBrightness:{b_after:.1f}  Contrast:{c_after:.1f}')
ax[1].axis('off')
plt.suptitle('Night Vision Enhancement for Road Safety')
plt.tight_layout()
plt.savefig(out_compare, dpi=150)

print(f"✅ Saved files: {out_enhanced}, {out_compare}")
PY
```
### How to Run
Place your nighttime image as night_road.jpg in the project folder.

Run:
```bash
python enhance_night_road.py
```
### View results:
- enhanced_road.jpg → enhanced grayscale image.
- comparison.png → original vs enhanced side-by-side.

### Results
<img width="568" height="322" alt="Image" src="https://github.com/user-attachments/assets/398ea651-6e7c-4866-8115-3cbd7148ea22" />

<img width="564" height="287" alt="Image" src="https://github.com/user-attachments/assets/1479b1f7-d3fd-4c7e-8d06-27155a611d27" />


### Notes
Works best on low-light or nighttime images. Can be used for real-time enhancement with video streams. Prevents over-amplification of noise with clipLimit.

### License
Licensed under the MIT License; free to use and modify.

### Acknowledgements
Thanks to OpenCV for image processing tools, NumPy for numerical operations, and Matplotlib for visualizations.

---

If you also commit `night_road.jpg`, `enhanced_road.jpg`, and `comparison.png` to your GitHub repo, the **before/after table will display on the GitHub README page** without requiring users to run your code.

I can create a **more compact version** with just the essential code snippet and results if you want it to look cleaner.
