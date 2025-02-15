# Driver Drowsiness Detection System

## Overview

This project implements a real-time driver drowsiness detection system using computer vision techniques. The system analyzes video input from a webcam to identify signs of drowsiness, such as closed eyes and frequent yawning. If drowsiness is detected (specifically, if the driver closes their eyes for a span of 15 seconds continuously or yawns 5 or more times frequently), an alarm is triggered to alert the driver. This system aims to prevent accidents caused by driver fatigue.

## Features

*   **Real-time drowsiness detection:** Continuously monitors the driver's face for signs of fatigue.
*   **Eye closure detection:** Utilizes a Convolutional Neural Network (CNN) to detect closed eyes.
*   **Yawning detection:** Calculates lip distance to identify yawning.
*   **Alarm system:** Triggers an audible alarm when drowsiness is detected. The alarm sounds if the driver's eyes are closed long enough to accumulate a drowsiness score exceeding 15 *or* if the driver yawns more than 4 times. The yawn counter is reset after the alarm.
*   **Score-based drowsiness level:** Accumulates a score based on the frequency and duration of drowsiness indicators.
*   **Clear visual alerts:** Displays visual warnings on the video feed when drowsiness is detected.

## Dependencies

Before running the code, ensure you have the following libraries installed. You can install them using pip:

pip install opencv-python
pip install pygame
pip install dlib
pip install keras
pip install tensorflow # or theano, depending on your Keras backend
pip install numpy


*   **opencv-python:**  For image and video processing.
*   **pygame:** For playing the alarm sound.
*   **dlib:** For facial landmark detection.
*   **keras:** For the deep learning model.
*   **tensorflow:** Deep learning framework
*   **numpy:** For numerical computations.

## Installation

1.  **Clone the repository:**

    ```
    git clone [https://github.com/premtheganesh/drowsinessdetection]
    cd [repository directory]
    ```

2.  **Download the required pre-trained models and data:**

    *   **haarcascade\_frontalface\_alt.xml:** (Haar cascade for face detection)
    *   **haarcascade\_lefteye\_2splits.xml:** (Haar cascade for left eye detection)
    *   **haarcascade\_righteye\_2splits.xml:** (Haar cascade for right eye detection)
    *   **shape\_predictor\_68\_face\_landmarks.dat:** (Facial landmark predictor)
    *   **cnncat2.h5:** (Trained CNN model for eye state classification)
    *   **alarm.wav:** (Sound file for the alarm)

    Create the following directory structure and place the files accordingly:

    ```
    driver-drowsiness-detection/
    ├── driverdrowsinessdetection.py
    ├── haar cascade files/
    │   ├── haarcascade_frontalface_alt.xml
    │   ├── haarcascade_lefteye_2splits.xml
    │   └── haarcascade_righteye_2splits.xml
    ├── models/
    │   └── cnncat2.h5
    ├── alarm.wav
    └── shape_predictor_68_face_landmarks.dat
    ```

    *Note:* You can find the Haar cascade files online (e.g., in the OpenCV GitHub repository). The `shape_predictor_68_face_landmarks.dat` file can be downloaded from the [dlib website](http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2).  The `cnncat2.h5` model should be placed in a folder named `models`.

3.  **Ensure correct file paths:**
    *   Modify the file paths in the `driverdrowsinessdetection.py` script to match the location of your downloaded files.  Specifically, check these lines:

        ```
        face = cv2.CascadeClassifier('haar cascade files\haarcascade_frontalface_alt.xml')
        leye = cv2.CascadeClassifier('haar cascade files\haarcascade_lefteye_2splits.xml')
        reye = cv2.CascadeClassifier('haar cascade files\haarcascade_righteye_2splits.xml')
        PREDICTOR_PATH = "shape_predictor_68_face_landmarks.dat"
        model = load_model('models/cnncat2.h5')
        sound = mixer.Sound('alarm.wav')
        ```

## Usage

1.  **Run the script:**

    ```
    python driverdrowsinessdetection.py
    ```

2.  **Ensure your webcam is active and properly connected.** The script will open a window displaying the video feed from your webcam.

3.  **The system will detect your face, eyes, and mouth.** It will then analyze these features to determine if you are showing signs of drowsiness.

4.  **If drowsiness is detected (high score or frequent yawning), the alarm will sound, and a visual warning will appear on the screen.**

5.  **To exit the program, press 'q' in the video window.**

## Code Explanation

*   **`driverdrowsinessdetection.py`:** The main script that captures video from the webcam, detects faces, eyes, and yawns, and triggers the alarm if drowsiness is detected.
*   **Haar cascades:**  XML files containing pre-trained classifiers for face and eye detection.  These are used for fast object detection.
*   **Facial Landmark Predictor:**  `shape_predictor_68_face_landmarks.dat` is a pre-trained model that identifies 68 specific points on the face.  This is used to locate the eyes and mouth accurately.
*   **CNN Model (`cnncat2.h5`):** A Convolutional Neural Network (CNN) trained to classify eye states (open or closed).  The model is loaded using Keras.
*   **`alarm.wav`:**  The audio file played when drowsiness is detected.
*   **Key Functions:**
    *   `get_landmarks(im)`: Detects facial landmarks using dlib.
    *   `mouth_open(image)`: Calculates the distance between the upper and lower lips to detect yawning.

## How it Works

1.  **Face Detection:** The code uses Haar cascades to detect faces in the video frame.
2.  **Eye Detection:** Haar cascades are used to detect the left and right eyes within the detected face region.
3.  **Eye State Classification:** The detected eye regions are fed into a pre-trained CNN model (`cnncat2.h5`) to classify whether the eyes are open or closed.
4.  **Yawning Detection:** Facial landmarks are detected using dlib, and the distance between the upper and lower lips is calculated. A threshold is used to determine if the driver is yawning.
5.  **Drowsiness Scoring:** A score is incremented when the eyes are closed and decremented when the eyes are open. The yawn count also contributes to the drowsiness detection.
6.  **Alarm Trigger:** If the drowsiness score exceeds 15 *or* the yawn count exceeds 4, an alarm is triggered to alert the driver. The yawn counter is reset after the alarm.

## Acknowledgements

*   This project utilizes pre-trained models and techniques from various open-source resources.
