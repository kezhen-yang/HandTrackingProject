# HandTrackingProject

Real-time hand tracking demos using OpenCV and MediaPipe with a reusable detector module for building gesture-based applications.

## Overview
This repository contains hand tracking examples and a reusable hand detector class built on MediaPipe Hands. It captures webcam frames, detects hand landmarks in real-time, and exposes 21 landmark coordinates per hand for downstream use (e.g., gesture controls, games, virtual interfaces).

## Features
- ✅ Real-time hand detection and tracking
- ✅ Track up to 2 hands simultaneously
- ✅ 21 landmark points per hand (fingertips, knuckles, palm, wrist)
- ✅ FPS counter for performance monitoring
- ✅ Reusable module for easy integration
- ✅ Clean exit with 'q' key

## Files
- **`HandTrackingMin.py`** — Minimal single-file example that draws landmarks and highlights the thumb tip
- **`HandTrackingModule.py`** — Reusable `handDetector` class with configurable parameters
- **`MyNewGameHandTracking.py`** — Example application that imports the module and prints thumb coordinates
- **`README.md`** — This file

## Requirements

### System
- macOS, Windows, or Linux with webcam
- Python 3.10 or higher (required for MediaPipe compatibility)

### Dependencies
```bash
pip install opencv-python mediapipe
```

**Important**: Python 3.10+ is required due to MediaPipe dependencies. If you're on Python 3.9 or earlier, upgrade first:
```bash
# macOS with Homebrew
brew install python@3.10

# Then use python3.10 explicitly
python3.10 -m pip install opencv-python mediapipe
```

## Quick Start

### 1. Run the minimal example:
```bash
python3.10 HandTrackingMin.py
```
This displays webcam feed with hand landmarks drawn and thumb tip highlighted.

### 2. Run the module-based example:
```bash
python3.10 MyNewGameHandTracking.py
```
This uses the reusable module and prints thumb tip coordinates to console.

### 3. Exit the application:
Press **'q'** key to cleanly exit (or Ctrl+C in terminal).

## Using the Hand Detector Module

### Basic Usage
```python
import cv2
from HandTrackingModule import handDetector

# Initialize detector
detector = handDetector(
    mode=False,        # False for video stream, True for static images
    maxHands=2,        # Maximum number of hands to detect
    detectionCon=0.5,  # Minimum detection confidence (0.0-1.0)
    trackCon=0.5       # Minimum tracking confidence (0.0-1.0)
)

cap = cv2.VideoCapture(0)

while True:
    success, img = cap.read()
    
    # Draw hand landmarks on image
    img = detector.findHands(img, draw=True)
    
    # Get landmark positions for first hand
    lmList = detector.findPosition(img, handNo=0, draw=True)
    
    if len(lmList) != 0:
        # Access specific landmark (e.g., thumb tip is index 4)
        thumb_tip = lmList[4]  # Returns [id, x, y]
        print(f"Thumb tip at: x={thumb_tip[1]}, y={thumb_tip[2]}")
    
    cv2.imshow("Hand Tracking", img)
    
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
```

### Constructor Parameters
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `mode` | bool | False | Static image mode (False for video stream) |
| `maxHands` | int | 2 | Maximum number of hands to detect (1-2) |
| `detectionCon` | float | 0.5 | Minimum detection confidence (0.0-1.0) |
| `trackCon` | float | 0.5 | Minimum tracking confidence (0.0-1.0) |

### Methods

#### `findHands(img, draw=True)`
Detects hands in the image and optionally draws landmarks.
- **Parameters:**
  - `img`: BGR image from OpenCV
  - `draw`: Whether to draw landmarks (default: True)
- **Returns:** Image with landmarks drawn (if draw=True)

#### `findPosition(img, handNo=0, draw=True)`
Gets landmark positions for a specific hand.
- **Parameters:**
  - `img`: BGR image from OpenCV
  - `handNo`: Hand index (0 for first hand, 1 for second)
  - `draw`: Whether to draw circles on landmarks (default: True)
- **Returns:** List of `[id, x, y]` for all 21 landmarks

## MediaPipe Hand Landmarks

The 21 landmark indices:
```
0: Wrist
1-4: Thumb (base to tip)
5-8: Index finger (base to tip)
9-12: Middle finger (base to tip)
13-16: Ring finger (base to tip)
17-20: Pinky finger (base to tip)
```

**Common landmarks:**
- `lmList[4]` — Thumb tip
- `lmList[8]` — Index finger tip
- `lmList[12]` — Middle finger tip
- `lmList[0]` — Wrist

## Applications & Use Cases

This hand tracking module can be used to build:

### 🎮 Gesture Controls
- Virtual mouse/touchpad
- Gaming controls (rock-paper-scissors, finger counting)
- Presentation controls (next/previous slide)

### 🎨 Creative Tools
- Air drawing/painting
- Virtual instruments (air piano, drums)
- 3D modeling gestures

### ♿ Accessibility
- Hands-free computer control
- Sign language recognition foundation
- Alternative input methods

### 🏋️ Health & Fitness
- Finger exercise counting (physical therapy)
- Hand movement rehabilitation tracking
- Gesture-based workout controls

### 🏠 Smart Home
- Touchless device control
- IoT gesture commands
- AR/VR hand interfaces

## Troubleshooting

### Camera not opening
```python
# Try different camera indices
cap = cv2.VideoCapture(1)  # or 2, 3, etc.
```

### "No module named 'mediapipe'" error
Make sure you're using Python 3.10+:
```bash
python3 --version  # Check version
python3.10 -m pip install mediapipe opencv-python
python3.10 MyNewGameHandTracking.py
```

### Low FPS / Lag
- Close other applications using the webcam
- Reduce `model_complexity` in MediaPipe (advanced)
- Use `maxHands=1` if you only need one hand
- Ensure good lighting for better detection

### Hand detection not working
- Ensure adequate lighting
- Keep hands within camera view
- Avoid busy backgrounds
- Try adjusting `detectionCon` to 0.3 for easier detection

## Performance

Typical FPS on different hardware:
- **MacBook Pro M1**: 30-60 FPS
- **Modern laptop (Intel i5+)**: 20-40 FPS  
- **Raspberry Pi 4**: 5-15 FPS

## Next Steps

Want to build something more advanced? Check out these ideas:
1. **Finger counter** - Detect how many fingers are up
2. **Virtual mouse** - Control cursor with index finger
3. **Gesture recognition** - Recognize custom hand gestures
4. **Air painter** - Draw in the air with your finger
5. **Volume control** - Pinch gesture for system volume

## Credits

Built with:
- [MediaPipe](https://google.github.io/mediapipe/) by Google
- [OpenCV](https://opencv.org/) for computer vision

## License

Use and adapt freely. No explicit license included; add one if required for your project.

---

**Questions or issues?** Feel free to open an issue or contribute improvements!