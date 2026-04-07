# Face Recognition System – AI

A real-time face recognition attendance system built with Python, OpenCV, and TensorFlow. The system detects and recognises faces through a webcam, logs attendance automatically to a CSV file, and displays the recognised name as an overlay on the live video feed.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Project Structure](#project-structure)
- [How It Works](#how-it-works)
- [Requirements](#requirements)
- [Installation](#installation)
- [Dataset Setup](#dataset-setup)
- [Usage](#usage)
  - [1. Preprocess the Data](#1-preprocess-the-data)
  - [2. Train the Model](#2-train-the-model)
  - [3. Run Real-Time Recognition](#3-run-real-time-recognition)
- [Output](#output)
- [Model Architecture](#model-architecture)
- [Configuration](#configuration)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

This project implements an end-to-end face recognition pipeline:

1. **Preprocessing** – Images are loaded from a structured dataset, resized to 128 × 128 pixels, and normalised.
2. **Training** – A Convolutional Neural Network (CNN) is trained on the preprocessed images using TensorFlow/Keras.
3. **Real-Time Recognition** – The trained model runs inference on live webcam frames, identifies the person, and records their attendance.

---

## Features

- Real-time face recognition via webcam using OpenCV.
- Custom CNN trained from scratch on your own dataset.
- Dynamic label mapping – supports any number of student/person classes.
- Automatic attendance logging to `results/attendance.csv` (each person is logged only once per session).
- Clean separation of concerns across three focused scripts.

---

## Project Structure

```
Face-Recognition-System-AI/
│
├── preprocess_data.py.txt      # Data loading and preprocessing pipeline
├── train_model.py.txt          # CNN model definition and training
├── real_time_recognition.py.txt# Webcam inference and attendance logging
│
└── results/                    # Created automatically at runtime
    ├── face_recognition_model.h5   # Saved trained model
    └── attendance.csv              # Attendance log
```

> **Note:** The source files currently use the `.py.txt` extension. Rename them to `.py` before running.

---

## How It Works

```
Dataset (train / valid / test)
        │
        ▼
preprocess_data.py
  └─ Reads images, resizes to 128×128, normalises [0,1], builds label map
        │
        ▼
train_model.py
  └─ Builds CNN → trains on train split → validates on valid split → saves model
        │
        ▼
real_time_recognition.py
  └─ Opens webcam → preprocesses each frame → runs model inference
     → overlays predicted name → logs attendance to CSV
```

---

## Requirements

| Package       | Purpose                          |
|---------------|----------------------------------|
| Python ≥ 3.8  | Runtime                          |
| TensorFlow ≥ 2.x | Model building and inference  |
| OpenCV (`cv2`) | Image loading and webcam access |
| NumPy         | Array operations                 |
| Pandas        | Attendance CSV management        |

---

## Installation

```bash
# 1. Clone the repository
git clone https://github.com/UnaizaMukhdoom/Face-Recognition-System-AI.git
cd Face-Recognition-System-AI

# 2. (Recommended) Create and activate a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install tensorflow opencv-python numpy pandas
```

---

## Dataset Setup

The pipeline expects a dataset directory with the following layout:

```
<dataset_root>/
├── train/
│   ├── student1_img1.jpg
│   └── ...
├── valid/
│   └── ...
└── test/
    └── ...
```

Update the `DATASET_PATH` constant in both `preprocess_data.py` and `real_time_recognition.py` to point to your dataset root:

```python
DATASET_PATH = Path("/path/to/your/dataset")
```

---

## Usage

> Rename the `.py.txt` files to `.py` first:
> ```bash
> for f in *.py.txt; do mv "$f" "${f%.txt}"; done
> ```

### 1. Preprocess the Data

```bash
python preprocess_data.py
```

Loads images from the dataset, resizes and normalises them, and prints a summary of each split.

### 2. Train the Model

```bash
python train_model.py
```

Builds the CNN, trains it for 15 epochs with a batch size of 32, and saves the trained model to `results/face_recognition_model.h5`.

### 3. Run Real-Time Recognition

```bash
python real_time_recognition.py
```

Opens the default webcam, performs real-time face recognition, and logs attendance. Press **`q`** to quit.

---

## Output

| File | Description |
|------|-------------|
| `results/face_recognition_model.h5` | Trained Keras model |
| `results/attendance.csv` | Attendance log with columns `Name` and `Time` |

Sample `attendance.csv`:

```
Name,Time
Student 0,2024-05-01 09:03:22
Student 1,2024-05-01 09:04:45
```

---

## Model Architecture

```
Input: (128, 128, 3)
   │
Conv2D(32, 3×3, relu)
MaxPooling2D(2×2)
   │
Conv2D(64, 3×3, relu)
MaxPooling2D(2×2)
   │
Conv2D(128, 3×3, relu)
Flatten
   │
Dense(128, relu)
Dense(num_classes, softmax)
```

- **Optimizer:** Adam  
- **Loss:** Sparse Categorical Cross-Entropy  
- **Metric:** Accuracy

---

## Configuration

Key constants that can be adjusted:

| Constant | File | Default | Description |
|----------|------|---------|-------------|
| `IMG_SIZE` | all scripts | `128` | Image resize dimension (px) |
| `DATASET_PATH` | `preprocess_data.py`, `real_time_recognition.py` | *(local path)* | Path to dataset root |
| `MODEL_PATH` | `train_model.py`, `real_time_recognition.py` | `../results/face_recognition_model.h5` | Model save/load path |
| `ATTENDANCE_FILE` | `real_time_recognition.py` | `../results/attendance.csv` | Attendance log path |
| Epochs / batch size | `train_model.py` | `15` / `32` | Training hyperparameters |

---

## Contributing

Contributions are welcome! Please open an issue to discuss proposed changes before submitting a pull request.

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/your-feature`.
3. Commit your changes: `git commit -m "Add your feature"`.
4. Push to your fork and open a pull request.

---

## License

This project is open-source. See the repository for licensing details.

