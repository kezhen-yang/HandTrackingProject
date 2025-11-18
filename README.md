# HandTrackingProject

Minimal hand-tracking demos using OpenCV + MediaPipe and a small reusable detector module.

## Overview
This repository contains a lightweight example and a reusable hand detector class built on MediaPipe Hands. It captures webcam frames, detects hand landmarks, and exposes landmark coordinates for downstream use (e.g., games, gestures).

## Files
- `HandTrackingMin.py` — minimal single-file example that draws landmarks and highlights the thumb tip.
- `HandTrackingModule.py` — reusable `handDetector` class with `findHands(img, draw=True)` and `findPosition(img, handNo=0, draw=True)` methods.
- `MyNewGameHandTracking.py` — example that imports the module and prints the thumb landmark coordinates.
- `README.md` — this file.

## Requirements
- macOS (tested) or other OS with webcam.
- Python 3.7+
- Install dependencies:
  ```sh
  python3 -m pip install opencv-python mediapipe
  ```

## Quick start
Run the minimal example:
```sh
python3 HandTrackingMin.py
```
Run the module example:
```sh
python3 MyNewGameHandTracking.py
```

## Using the hand detector module
Example:
```py
from HandTrackingModule import handDetector
detector = handDetector(mode=False, maxHands=2, detectionCon=0.5, trackCon=0.5)

# img is a BGR frame from cv2.VideoCapture
img = detector.findHands(img)                   # draws landmarks if draw=True
lmList = detector.findPosition(img, handNo=0)   # returns [[id, x, y], ...]
if lmList:
    # lmList[4] is thumb tip (MediaPipe index 4)
    print(lmList[4])
```

Constructor options:
- mode: static image mode (False for video)
- maxHands: maximum number of hands to detect
- detectionCon: detection confidence threshold (0.0–1.0)
- trackCon: tracking confidence threshold (0.0–1.0)

## Notes
- Camera index 0 is used by default. To use another camera, pass the index to `cv2.VideoCapture()`.
- Landmark coordinates returned by `findPosition` are integer pixel coordinates in the BGR image.
- Landmark indexing follows MediaPipe Hands (e.g., id 4 is the thumb tip).

## License
Use/adapt freely. No explicit license included; add one if required.