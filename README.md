# Skin Cancer Classification
This code implements a Convolutional Neural Network (CNN) for skin cancer classification using the ISIC 2019 Challenge dataset.

## Importing Necessary Libraries

import tensorflow as tf
from tensorflow import keras
from tensorflow.keras import layers, models
import tensorflow_hub as hub
import os
import shutil
import pandas as pd
from sklearn.model_selection import train_test_split
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Conv2D, MaxPooling2D, Flatten, Dense
from tensorflow.keras.applications import NASNetLarge, NASNetMobile
import numpy

## Dataset Preparation
### Reading all the images
## This section explains how image paths and labels (benign or malignant) are obtained from a CSV file:

csv_file = pd.read_csv('ISIC_2019_Training_GroundTruth (2).csv')
image_path = []
benign_malign = csv_file.iloc[:, 1:2]
image_name = csv_file.iloc[:, 0]
dir = "ISIC_2019_Training_Input"
for names in image_name:
  fpath = os.path.join(dir, names + ".jpg")
  image_path.append(fpath)
benign = []
mel = []
index = 0
for values in benign_malign.iloc[:, 0]:
  if values == 0.0:
    benign.append(image_path[index])
  else:
    mel.append(image_path[index])
  index += 1
print("Length benign: ", len(benign))
print("Length malign: ", len(mel))
Use code with caution.
content_copy
Dividing the images into train, test, and validation sets
We split the data into separate sets for training, validating, and testing the model's performance:


# For benign set of images
ben_train_paths, ben_test_paths = train_test_split(benign, test_size=0.2, random_state=42)

ben_train_paths, ben_val_paths = train_test_split(ben_train_paths, test_size=0.2, random_state=42)

# Print the sizes of each set
print("Benign dataset")
print("Train set size:", len(ben_train_paths))
print("Validation set size:", len(ben_val_paths))
print("Test set size:", len(ben_test_paths))

# For images with melanoma
mel_train_paths, mel_test_paths = train_test_split(mel, test_size=0.2, random_state=42)

mel_train_paths, mel_val_paths = train_test_split(mel_train_paths, test_size=0.2, random_state=42)

# Print the sizes of each set
print("Melanoma Dataset")
print("Train set size:", len(mel_train_paths))
print("Validation set size:", len(mel_val_paths))
print("Test set size:", len(mel_test_paths))
Use code with caution.
content_copy
Folder Segregation
This section explains how the images are organized into folders for training, validation, and testing:

Python
os.makedirs("test", exist_ok=True)
os.makedirs("train", exist_ok=True)
os.makedirs("validation", exist_ok=True)

os.makedirs("test/" + "benign", exist_ok=True)
os.makedirs("test/" + "mel", exist_ok=True)
os.makedirs("train/" + "benign", exist_ok=True)
os.makedirs("train/" + "mel", exist_ok=True)
os.makedirs("validation/" + "benign", exist_ok=True)
os.makedirs("validation/" + "mel", exist_ok=True)

for file_path in ben_train_paths:
  shutil.copy(file_path, "train/benign/")
for file_path in ben_test_paths:
  shutil.copy(file_path, "test/benign/")
for file_path in ben_val_paths:
  shutil.copy(file_path, "validation/benign/")
for file_path in mel_train_paths:
  shutil.copy(file_path, "train/mel/")
for file_path in mel_test_paths:
  shutil.copy(file_path, "
