# 🛌 Drowsiness Detection using YOLOv5 🚗💤

This project uses **YOLOv5** to detect whether a person is **awake** or **drowsy** from images. It can be extended to work with live video streams (e.g., in driver monitoring systems).

---

## 🔧 Setup

### 1. Clone the YOLOv5 Repo

```bash
git clone https://github.com/ultralytics/yolov5
cd yolov5
```

### 2. Install Requirements

Make sure you have Python 3.8+ (Anaconda recommended):

```bash
pip install -r requirements.txt
```

(Optional but recommended: Use a virtual environment)

---

## 🗃️ Dataset Structure

Your dataset should look like this:

```
data/
├── images/
│   ├── train/
│   │   ├── awake_1.jpg
│   │   └── drowsy_1.jpg
│   └── val/
│       ├── awake_2.jpg
│       └── drowsy_2.jpg
├── labels/
│   ├── train/
│   │   ├── awake_1.txt
│   │   └── drowsy_1.txt
│   └── val/
│       ├── awake_2.txt
│       └── drowsy_2.txt
```

Each `.txt` file should follow YOLO format:

```
<class_id> <x_center> <y_center> <width> <height>
```

---

## 📄 dataset.yaml

Make sure your `dataset.yaml` looks like this:

```yaml
path: data
train: images/train
val: images/val
names: ['awake', 'drowsy']
```

---

## 🏋️ Training

You can train the model using:

```bash
python train.py --img 320 --batch 8 --epochs 100 --data dataset.yaml --weights yolov5s.pt --device 0
```

Trained weights will be saved in:

```
runs/train/exp/weights/best.pt
```

---

## 🚀 Inference on Image

```python
import torch
import os

# Load model
model = torch.hub.load('ultralytics/yolov5', 'custom', path='runs/train/exp/weights/best.pt', force_reload=True)

# Run prediction
image_path = os.path.join('data', 'images', 'val', 'drowsy_1.jpg')
results = model(image_path)

# Show and print results
results.print()
results.show()
```

---

## 📦 Run Detection on Folder

```bash
python detect.py --weights runs/train/exp/weights/best.pt --img 320 --source data/images/test --device 0
```

Results will be saved to:

```
runs/detect/exp/
```

---

## 📈 Evaluation Metrics

After training, you'll get precision, recall, and mAP scores.

Example:

```
Class     Images  Instances   P      R    mAP50
awake         37         16  0.94   1.0   0.96
drowsy        37         19  0.97   0.95  0.97
```

---

## 🧠 Future Ideas

* Real-time drowsiness detection via webcam
* Deploy to edge devices like Jetson Nano
* Integrate alert system with audio or vibration

---

## 🙌 Credits

* Based on [YOLOv5 by Ultralytics](https://github.com/ultralytics/yolov5)
* Custom dataset annotated using [LabelImg](https://github.com/tzutalin/labelImg)

---

Let me know if you'd like a version tailored for **streamlit**, **Flask**, or **mobile deployment** too!
