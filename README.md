# -Object-Detection-in-Images-and-Videos-with-YOLOv10-
Object Detection in Images and Videos with YOLOv10 in Google Colab

# YOLOv10 Setup and Object Detection

This repository provides a step-by-step guide to setting up YOLOv10 and performing object detection on a video file.

## Prerequisites

- Python 3.6 or higher
- `pip` package manager

## Installation

1. **Clone the YOLOv10 repository:**

    ```bash
    !git clone https://github.com/THU-MIG/yolov10.git
    ```

2. **Change the directory to the cloned repository:**

    ```bash
    %cd yolov10
    ```

3. **Install the YOLOv10 package:**

    ```bash
    !pip install -q .
    ```

### Download Pre-trained Weights

The script downloads several pre-trained weights required for object detection.

```python
import os
import urllib.request

# Create a directory for the weights in the current working directory
weights_dir = os.path.join(os.getcwd(), 'weights')
os.makedirs(weights_dir, exist_ok=True)

# URLs of the weight files to be downloaded
urls = [
    "https://github.com/THU-MIG/yolov10/releases/download/v1.1/yolov10n.pt",
    "https://github.com/THU-MIG/yolov10/releases/download/v1.1/yolov10s.pt",
    "https://github.com/THU-MIG/yolov10/releases/download/v1.1/yolov10m.pt",
    "https://github.com/THU-MIG/yolov10/releases/download/v1.1/yolov10b.pt",
    "https://github.com/THU-MIG/yolov10/releases/download/v1.1/yolov10l.pt",
    "https://github.com/THU-MIG/yolov10/releases/download/v1.1/yolov10x.pt"
]

# Download each weight file from the URLs
for url in urls:
    filename = os.path.basename(url)
    filepath = os.path.join(weights_dir, filename)
    urllib.request.urlretrieve(url, filepath)
    print(f"Downloaded: {filepath}")


## Additional Downloads
Use gdown to download additional necessary files from Google Drive.

!gdown "https://drive.google.com/uc?id=1aiWk3ugOq6mIcb2JecCIHJXcUV40ReRo&confirm=t"
!gdown "https://drive.google.com/uc?id=1U0QqPZJjjZZ2wLGuxID2iSru6FLPW807&confirm=t"

## Display an Image
Display an image from the local file system to verify successful setup.

from IPython.display import Image
Image(filename='/content/yolov10/yolov10/Screenshot 2024-07-24 154159.png')

## Run YOLOv10 for Object Detection
Perform object detection on a video file and save the results.

!yolo task=detect mode=predict save=True model=weights/yolov10x.pt source="demo.mp4"

## Compress and Display the Output Video
Compress the output video using ffmpeg and display it.

from IPython.display import HTML
from base64 import b64encode
import os

# Input video path
save_path = '/content/yolov10/yolov10/runs/detect/predict3/demo.avi'

# Compressed video path
compressed_path = "/content/yolov10/yolov10/runs/detect/predict3/result_compressed.mp4"

# Compress the video using ffmpeg
os.system(f"ffmpeg -i {save_path} -vcodec libx264 {compressed_path}")

# Check if the compressed video file was created
if os.path.isfile(compressed_path):
    # Read and encode the compressed video
    mp4 = open(compressed_path, 'rb').read()
    data_url = "data:video/mp4;base64," + b64encode(mp4).decode()

    # Display the video
    HTML(f"""
    <video width="640" height="480" controls>
      <source src="{data_url}" type="video/mp4">
      Your browser does not support the video tag.
    </video>
    """)
else:
    print(f"File not found: {compressed_path}")



# Acknowledgements
YOLOv10
gdown
