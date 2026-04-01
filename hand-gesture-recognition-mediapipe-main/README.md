# 🖐️ Hand Gesture Recognition using MediaPipe

Estimate hand pose using **MediaPipe** (Python version). This is a sample program that recognizes hand signs and finger gestures with a simple MLP using the detected key points.

> ❗ _**This is an English translated version of the [original repo](https://github.com/Kazuhito00/hand-gesture-recognition-using-mediapipe). All content including comments and notebooks has been translated to English.**_ ❗

![Demo](https://user-images.githubusercontent.com/37477845/102222442-c452cd00-3f26-11eb-93ec-c387c98231be.gif)

---

## 📦 Contents

- ✅ Sample program
- ✅ Hand sign recognition model (TFLite)
- ✅ Finger gesture recognition model (TFLite)
- ✅ Learning data for hand sign recognition + training notebook
- ✅ Learning data for finger gesture recognition + training notebook

---

## 🛠️ Requirements

| Library | Version |
|--------|---------|
| mediapipe | 0.8.1 |
| OpenCV | 3.4.2 or later |
| Tensorflow | 2.3.0 or later |
| tf-nightly | 2.5.0.dev or later *(only for LSTM TFLite model)* |
| scikit-learn | 0.23.2 or later *(only for confusion matrix)* |
| matplotlib | 3.3.2 or later *(only for confusion matrix)* |

---

## 🚀 Demo

Run the demo using your webcam:

```bash
python app.py
```

### ⚙️ Options

| Option | Description | Default |
|--------|-------------|---------|
| `--device` | Camera device number | `0` |
| `--width` | Camera capture width | `960` |
| `--height` | Camera capture height | `540` |
| `--use_static_image_mode` | Use static image mode for MediaPipe | Unspecified |
| `--min_detection_confidence` | Detection confidence threshold | `0.5` |
| `--min_tracking_confidence` | Tracking confidence threshold | `0.5` |

---

## 📁 Directory Structure

```
│  app.py
│  keypoint_classification.ipynb
│  point_history_classification.ipynb
│
├─model
│  ├─keypoint_classifier
│  │  │  keypoint.csv
│  │  │  keypoint_classifier.hdf5
│  │  │  keypoint_classifier.py
│  │  │  keypoint_classifier.tflite
│  │  └─ keypoint_classifier_label.csv
│  │
│  └─point_history_classifier
│      │  point_history.csv
│      │  point_history_classifier.hdf5
│      │  point_history_classifier.py
│      │  point_history_classifier.tflite
│      └─ point_history_classifier_label.csv
│
└─utils
    └─cvfpscalc.py
```

### 📄 File Descriptions

| File | Description |
|------|-------------|
| `app.py` | Main inference program. Also used to collect training data |
| `keypoint_classification.ipynb` | Model training script for hand sign recognition |
| `point_history_classification.ipynb` | Model training script for finger gesture recognition |
| `model/keypoint_classifier/` | Files for hand sign recognition (data, model, labels, module) |
| `model/point_history_classifier/` | Files for finger gesture recognition (data, model, labels, module) |
| `utils/cvfpscalc.py` | FPS measurement module |

---

## 🧠 Training

Both hand sign and finger gesture models support adding/changing training data and retraining.

### ✋ Hand Sign Recognition

#### 1. Collect Learning Data
Press **"k"** to enter key point logging mode (displayed as `MODE: Logging Key Point`).

<img src="https://user-images.githubusercontent.com/37477845/102235423-aa6cb680-3f35-11eb-8ebd-5d823e211447.jpg" width="60%"/>

Press **"0"–"9"** to save key points to `model/keypoint_classifier/keypoint.csv`.
- 1st column: Class ID (pressed number)
- Remaining columns: Key point coordinates

<img src="https://user-images.githubusercontent.com/37477845/102345725-28d26280-3fe1-11eb-9eeb-8c938e3f625b.png" width="80%"/>

Key point coordinates go through the following preprocessing:

<img src="https://user-images.githubusercontent.com/37477845/102242918-ed328c80-3f3d-11eb-907c-61ba05678d54.png" width="80%"/>
<img src="https://user-images.githubusercontent.com/37477845/102244114-418a3c00-3f3f-11eb-8eef-f658e5aa2d0d.png" width="80%"/>

Default training data includes 3 classes:

| Class ID | Gesture |
|----------|---------|
| 0 | Open hand |
| 1 | Closed hand |
| 2 | Pointing |

<img src="https://user-images.githubusercontent.com/37477845/102348846-d0519400-3fe5-11eb-8789-2e7daec65751.jpg" width="25%"/> <img src="https://user-images.githubusercontent.com/37477845/102348855-d2b3ee00-3fe5-11eb-9c6d-b8924092a6d8.jpg" width="25%"/> <img src="https://user-images.githubusercontent.com/37477845/102348861-d3e51b00-3fe5-11eb-8b07-adc08a48a760.jpg" width="25%"/>

#### 2. Train the Model
Open `keypoint_classification.ipynb` in Jupyter Notebook and run all cells top to bottom.
To change the number of classes, update `NUM_CLASSES = 3` and modify `keypoint_classifier_label.csv`.

#### Model Structure
<img src="https://user-images.githubusercontent.com/37477845/102246723-69c76a00-3f42-11eb-8a4b-7c6b032b7e71.png" width="50%"/>

---

### 👆 Finger Gesture Recognition

#### 1. Collect Learning Data
Press **"h"** to enter point history logging mode (displayed as `MODE: Logging Point History`).

<img src="https://user-images.githubusercontent.com/37477845/102249074-4d78fc80-3f45-11eb-9c1b-3eb975798871.jpg" width="60%"/>

Press **"0"–"9"** to save to `model/point_history_classifier/point_history.csv`.

<img src="https://user-images.githubusercontent.com/37477845/102345850-54ede380-3fe1-11eb-8d04-88e351445898.png" width="80%"/>

Default training data includes 4 classes:

| Class ID | Gesture |
|----------|---------|
| 0 | Stationary |
| 1 | Clockwise |
| 2 | Counterclockwise |
| 4 | Moving |

<img src="https://user-images.githubusercontent.com/37477845/102350939-02b0c080-3fe9-11eb-94d8-54a3decdeebc.jpg" width="20%"/> <img src="https://user-images.githubusercontent.com/37477845/102350945-05131a80-3fe9-11eb-904c-a1ec573a5c7d.jpg" width="20%"/> <img src="https://user-images.githubusercontent.com/37477845/102350951-06444780-3fe9-11eb-98cc-91e352edc23c.jpg" width="20%"/> <img src="https://user-images.githubusercontent.com/37477845/102350942-047a8400-3fe9-11eb-9103-dbf383e67bf5.jpg" width="20%"/>

#### 2. Train the Model
Open `point_history_classification.ipynb` in Jupyter Notebook and run all cells top to bottom.
To change the number of classes, update `NUM_CLASSES = 4` and modify `point_history_classifier_label.csv`.

#### Model Structure
<img src="https://user-images.githubusercontent.com/37477845/102246771-7481ff00-3f42-11eb-8ddf-9e3cc30c5816.png" width="50%"/>

LSTM model (set `use_lstm = False` → `True`, requires tf-nightly):

<img src="https://user-images.githubusercontent.com/37477845/102246817-8368b180-3f42-11eb-9851-23a7b12467aa.png" width="60%"/>

---

## 🔗 Reference

- [MediaPipe](https://mediapipe.dev/)

---

## 👩‍💻 Author

**Pujitha** — [GitHub](https://github.com/Pujitha1809)

---

## 🙏 Credits

| Role | Person |
|------|--------|
| Original Author | [Kazuhito Takahashi](https://twitter.com/KzhtTkhs) |
| English Translation | [Nikita Kiselov](https://github.com/kinivi) |

---

## 📄 License

This project is licensed under the [Apache v2 License](LICENSE).
