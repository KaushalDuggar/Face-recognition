# Face-recognition
This project builds upon the research paper "Pose-Invariant Face Recognition via Facial Landmark Based Ensemble Learning" (Lin & Otoya, 2023), which proposes a novel method for face recognition that is robust to pose variations.
The original work utilizes 68 facial landmarks to compute a Face Angle Vector (FAV) for head pose classification and uses local descriptors like SIFT, HOG, and LBP around specific facial landmarks to perform ensemble-based 1:1 face verification.

This project extends the original method by:

Replacing the traditional 68 landmarks with a 468-point 3D facial mesh generated using Google's MediaPipe framework.

Improving pose estimation accuracy by leveraging the denser landmark set.

Enhancing local feature extraction around landmarks, leading to more discriminative and robust face descriptors.

Training a multi-model ensemble (SVM, Naive Bayes, GMM) using local features for identity verification.

Motivation
Pose variation is a major challenge in face recognition.
Traditional 68-landmark models can struggle with extreme poses or occlusions.
By moving to a 468-landmark dense mesh, we can better capture fine-grained face geometry and improve:

Head pose classification

Local feature extraction

Overall face verification and recognition accuracy, especially under extreme angles (up to ±90° yaw).

Methodology
1. Face and Landmark Detection
Original: 68 landmarks detected using a standard model.

Extended: 468 landmarks detected using MediaPipe Face Mesh, providing a more detailed and stable facial representation.

2. Face Pose Estimation
Face Angle Vector (FAV): Computed based on angular relationships between the eye centers and mouth landmarks.

Classifier: A Linear SVM is trained to classify the pose into discrete categories (e.g., Npose = 3 or Npose = 5 classes based on yaw angle ranges).

Improvement: The dense mesh allows for more accurate computation of FAV, leading to better pose classification.

3. Feature Extraction
Around each selected landmark (or its region), local descriptors are extracted:

SIFT (Scale-Invariant Feature Transform)

HOG (Histogram of Oriented Gradients)

LBP (Local Binary Patterns)

Patch-based extraction: Small regions around landmarks are cropped and features are extracted.

4. Base Learner Training
Each facial landmark has a corresponding base learner:

SVM, Naive Bayes, or GMM trained using the local descriptors.

Each subject's face model is an ensemble of base learners corresponding to selected facial landmarks.

5. Base Learner Selection (BLS)
Pose-aware selection: Depending on the estimated face pose, only a subset of landmarks and corresponding base learners are used.

Goal: Reduce computational load while maintaining high recognition performance.

6. Ensemble Decision
Each base learner outputs a match score.

Scores are aggregated (mean or trimmed-mean rules) to form a final identity decision.

7. Evaluation
Dataset: CMU-PIE database (poses ±90°, ambient lighting, neutral expressions).

Metrics:

Face Verification: TAR@FAR

Face Identification: Rank-N accuracy

Results showed state-of-the-art performance, achieving 100% accuracy under ±90° pose conditions with the extension.
