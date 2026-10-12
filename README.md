# Drowsiness Detection (Eye-Blink Based)

A real-time computer-vision system that watches a person's eyes through the webcam and **raises an audio alarm when they stay closed for too long**, a common sign of driver fatigue.

## How it works
1. **Face detection.** dlib's frontal-face detector finds the face in each webcam frame.
2. **Facial landmarks.** A 68-point landmark model locates the six points around each eye.
3. **Eye Aspect Ratio (EAR).** The ratio of the eye's height to its width is computed for both eyes and averaged:

   `EAR = (‖p2 − p6‖ + ‖p3 − p5‖) / (2 · ‖p1 − p4‖)`

   An open eye gives a roughly constant EAR, and it drops sharply when the eye closes.
4. **Alert logic.** If EAR stays **below 0.25 for 20 consecutive frames**, the frame is marked with an on-screen **ALERT** and an alarm sound plays (pygame). A normal blink is too short to trigger it.

Eye contours are drawn on the live video so you can see the tracking.

## Tech stack
Python · OpenCV · dlib · imutils · SciPy · pygame

## Setup
1. Install the dependencies:
   ```bash
   pip install opencv-python dlib imutils scipy pygame
   ```
   (dlib needs CMake and a C++ build toolchain on Windows.)
2. Download the landmark model `shape_predictor_68_face_landmarks.dat`, for example from [Kaggle](https://www.kaggle.com/datasets/sergiovirahonda/shape-predictor-68-face-landmarksdat).
3. In `EyeBlink.py`, set the paths to the `.dat` file and to an alarm sound (`.wav`).
4. Run it, and press **q** to quit:
   ```bash
   python EyeBlink.py
   ```

## Possible improvements
- Make the thresholds configurable and calibrate them per user
- Add yawn detection (mouth aspect ratio)
- Log drowsiness events with timestamps
