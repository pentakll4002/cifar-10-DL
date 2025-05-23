# 📦 CIFAR-10 Deep Learning Classifier
This project implements a Convolutional Neural Network (CNN) to classify images from the CIFAR-10 dataset using TensorFlow/Keras. CIFAR-10 is a well-known computer vision dataset consisting of 60,000 32x32 color images in 10 different classes, such as airplanes, cars, cats, dogs, etc.

## 🧰 Libraries Used
```python
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd

from tensorflow.keras.datasets import cifar10
from tensorflow.keras.utils import to_categorical
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Conv2D, MaxPooling2D, AveragePooling2D, Dense, Flatten, Dropout
from tensorflow.keras.optimizers import Adam
from tensorflow.keras.callbacks import EarlyStopping, TensorBoard
from tensorflow.keras.preprocessing.image import ImageDataGenerator
```

## 📥 Dataset
The CIFAR-10 dataset is directly available through Keras:
```python
(x_train, y_train), (x_test, y_test) = cifar10.load_data()
```

The labels are one-hot encoded:
```python
num_classes = 10
y_train = to_categorical(y_train, num_classes)
y_test = to_categorical(y_test, num_classes)
```

## 🔧 Data Augmentation
To improve generalization, we use ImageDataGenerator for real-time data augmentation:

```python
datagen = ImageDataGenerator(
    rotation_range=15,
    width_shift_range=0.1,
    height_shift_range=0.1,
    horizontal_flip=True,
)
datagen.fit(x_train)
```

## 🧠 Model Architecture
The model is built using the Keras Sequential API:
```python
model = Sequential([
    Conv2D(32, (3, 3), activation='relu', input_shape=(32, 32, 3)),
    MaxPooling2D((2, 2)),
    Dropout(0.25),
    
    Conv2D(64, (3, 3), activation='relu'),
    MaxPooling2D((2, 2)),
    Dropout(0.25),
    
    Flatten(),
    Dense(128, activation='relu'),
    Dropout(0.5),
    Dense(num_classes, activation='softmax')
])
```

## 🛠️ Compile the Model
```python
model.compile(
    optimizer=Adam(learning_rate=0.001),
    loss='categorical_crossentropy',
    metrics=['accuracy']
)
```

## ⏱️ Callbacks
```python
early_stop = EarlyStopping(monitor='val_loss', patience=5)
tensorboard = TensorBoard(log_dir='./logs')
```

## 🚀 Training
```python
history = model.fit(
    datagen.flow(x_train, y_train, batch_size=64),
    epochs=50,
    validation_data=(x_test, y_test),
    callbacks=[early_stop, tensorboard]
)
```

## 📊 Visualization
You can use matplotlib, seaborn, and pandas to visualize training history (accuracy/loss curves), confusion matrix, and classification reports.

## 📁 Project Structure
```bash
cifar10-dl/
│
├── cifar10_classifier.py       # Main script to train the model
├── README.md                   # Project overview
├── logs/                       # TensorBoard logs
└── saved_model/                # Optionally save trained model
```

✅ Requirements
- Python 3.7+

- TensorFlow 2.x

- NumPy

- Pandas

- Seaborn

- Matplotlib

Install requirements via:
```bash
pip install -r requirements.txt
```

## 🎯 Results
With this basic CNN architecture and light data augmentation, the model is expected to reach an accuracy of ~50–60% on the CIFAR-10 test set. Further improvements can be achieved through deeper models (e.g., ResNet, VGG), learning rate tuning, or regularization techniques.











