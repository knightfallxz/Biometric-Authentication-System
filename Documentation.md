# Face Recognition System

## Overview

This project provides a facial recognition-based authentication system, where users can register their faces and later authenticate by capturing a live image via webcam. The system uses facial feature encoding to recognize users and match them against previously registered images.

## Features

- **User Registration**: Users can register by capturing multiple images of their face, which are then processed and stored.
- **Model Training**: A face recognition model is trained using the registered user's face images, and a serialized model is saved for future authentication.
- **User Authentication**: The system captures a live image of the user during login and compares it against the stored model to authenticate the user.
- **Image Processing**: Images are preprocessed, augmented, and aligned for better recognition accuracy.
  
## System Requirements

- **Python**: Version 3.7 or higher
- **Libraries**: OpenCV, face-recognition, numpy, pickle

## Project Structure

```
.
├── face_authentication.py       # Handles face authentication
├── face_registration.py         # Handles face registration
├── face_training.py             # Trains face recognition model with registered users
├── LICENSE                      # License file
├── main.py                      # Entry point for the application
├── models                       # Directory for storing trained face models
│   └── face_recognition_model.pkl # Pickled model for face recognition
├── README.md                    # Project description
├── requirements.txt             # List of dependencies
└── utils.py                     # Utility functions for image processing and alignment
```

## Installation

### Prerequisites

1. Ensure you have Python 3.7 or higher installed on your system. You can check this by running:

   ```bash
   python --version
   ```

2. Install the necessary libraries listed in `requirements.txt`:

   ```bash
   pip install -r requirements.txt
   ```

### Libraries Used

- **OpenCV**: `opencv-python` for image processing and webcam capture.
- **Face Recognition**: `face-recognition` for detecting, encoding, and comparing facial features.
- **Numpy**: `numpy` for handling arrays and numerical computations.
- **Pickle**: `pickle` for serializing and deserializing the face recognition model.

## How to Run

To run the system, follow these steps:

1. **Clone the Repository** (if applicable):

   ```bash
   git clone https://github.com/your-repo/face-recognition-system.git
   cd face-recognition-system
   ```

2. **Install Dependencies**:

   Install the required libraries using the provided `requirements.txt` file:

   ```bash
   pip install -r requirements.txt
   ```

3. **Run the Application**:

   Start the application by executing the `main.py` file:

   ```bash
   python main.py
   ```

   You will be presented with the following menu:

   ```
   1. Register
   2. Login
   Enter 'q' to quit
   ```

### Registration Process

1. **Choose Option 1 (Register)**:
   
   When registering, you will be prompted to enter a username. The system will capture multiple images of your face using your webcam.

2. **Model Training**:
   
   After registration, the face recognition model will be trained in the background using the captured images. This model will be saved to `models/face_recognition_model.pkl`.

   - Example prompt during registration:
   
     ```
     Enter a username for registration: john_doe
     Capturing images... 
     User john_doe registered successfully. Model is being trained in the background...
     ```

### Authentication Process

1. **Choose Option 2 (Login)**:

   During login, you will be prompted to enter your username. The system will capture a live image from your webcam and attempt to match it with the registered face encodings.

2. **Successful or Failed Authentication**:

   - If a match is found, the system will authenticate the user and display a success message.
   - If the match fails, it will notify you of the failure.

   - Example output during authentication:
   
     ```
     Enter your username for login: john_doe
     Looking for a registered face. Please look at the camera.
     Authentication successful. Welcome, john_doe!
     ```

## Modules

### main.py
- **Purpose**: The main entry point for the application, providing an interactive menu for user registration, login, or exit.
  
### face_registration.py
- **Purpose**: Handles the process of registering a new user by capturing images of the user's face and saving them for model training.

### face_training.py
- **Purpose**: Trains a facial recognition model using the images captured during registration. The trained model is stored as a `.pkl` file.

### face_authentication.py
- **Purpose**: Handles user authentication by comparing a live image against the registered user's face encodings stored in the trained model.

### utils.py
- **Purpose**: Contains helper functions for image processing, such as preprocessing images, augmenting data, and aligning faces.

## Model Details

- The face recognition model is saved as `face_recognition_model.pkl` inside the `models/` directory.
- This model stores face encodings for each registered user. During authentication, these encodings are compared with the live image.

To expand on the existing documentation, you can include more detailed explanations about each module, covering the key functions and the flow of data between the different parts of the system. Here's a more in-depth breakdown of each module and the key functionalities within:

## Module Breakdown

### 1. **`main.py`**
   - **Purpose**: This is the main entry point for the system, providing a menu for user interaction (Register/Login). It orchestrates the flow by calling functions from other modules.
   
   - **Key Functions**:
     - **`main()`**: Handles the main program loop. It prompts users to register, log in, or quit the program. Based on the user's input, it calls:
       - **`register_face()`** from `face_registration.py` for new user registration.
       - **`authenticate_face()`** from `face_authentication.py` to perform face-based login.
       - **`train_face_model()`** from `face_training.py` to train the model with newly registered faces.

### 2. **`face_registration.py`**
   - **Purpose**: Handles the registration of a new user by capturing multiple images of their face using the webcam. These images are stored in a directory structure based on the username.
   
   - **Key Functions**:
     - **`register_face(username)`**: Captures a series of images of the user's face using the webcam. These images are saved in a directory named after the username. This function ensures that the user's facial data is properly captured and stored for later model training.

     - **Flow**:
       1. The function initializes webcam capture.
       2. It captures a predefined number of images (e.g., 5-10 images) for better recognition accuracy.
       3. Each image is saved in the `registered_users/` directory under a folder named after the username.

     - **Error Handling**: If no face is detected during the capture process, it alerts the user and retries.

### 3. **`face_training.py`**
   - **Purpose**: Trains the facial recognition model based on the images captured during user registration. It extracts facial encodings from the images and stores them in a serialized model.
   
   - **Key Functions**:
     - **`train_face_model(user)`**: This function processes the images saved during registration, extracts the face encodings, and updates the face recognition model. It saves the model in the `models/` directory as `face_recognition_model.pkl`.
     
     - **Flow**:
       1. The function loads all the images associated with the registered user.
       2. Each image is processed and encoded using the `face_recognition` library to extract facial features.
       3. The encodings are stored in a dictionary format, where the key is the username and the value is a list of encodings.
       4. The model is saved in a pickle file, which is used for authentication later.

     - **Model Update**: When a new user is registered, the model gets updated with the new face encodings without overwriting existing data.

     - **Error Handling**: If no valid face encodings are found (e.g., poor image quality or no faces), the model is not saved, and the user is alerted.

### 4. **`face_authentication.py`**
   - **Purpose**: Authenticates users by comparing a live webcam image with the stored face encodings in the trained model.
   
   - **Key Functions**:
     - **`authenticate_face(username)`**: This function captures a live image of the user during login, extracts the face encoding from the live image, and compares it to the stored encodings in the model to verify the user’s identity.

     - **Flow**:
       1. It checks whether the model exists and whether the username provided by the user is present in the model.
       2. A live image is captured using the webcam.
       3. The face encoding is extracted from the live image.
       4. The live encoding is compared to the registered encodings for the given username using the `face_recognition.compare_faces()` method.
       5. If a match is found, the user is authenticated; otherwise, the authentication fails.

     - **Key Sub-functions**:
       - **`capture_live_image()`**: Captures the current frame from the webcam for authentication.
       - **`load_model(model_path)`**: Loads the trained face recognition model from the pickle file.

     - **Error Handling**:
       - If no face is detected in the live image, the user is asked to retry.
       - If the username is not found in the model, authentication is aborted with a message.
       - If the model file is missing or corrupted, it alerts the user to register first.

### 5. **`utils.py`**
   - **Purpose**: Provides utility functions for image processing, augmentation, and alignment, improving recognition accuracy.
   
   - **Key Functions**:
     - **`preprocess_and_save(image, save_path)`**: Preprocesses the input image (converts to grayscale, sharpens, resizes, applies noise reduction) and saves the processed image to the given path.
       - **Use case**: Ensures consistency in image quality before they are used for model training.

     - **`preprocess_image(image)`**: Similar to `preprocess_and_save()` but returns the processed image without saving it.
       - **Use case**: Used during authentication to preprocess the live-captured image.

     - **`augment_image(image)`**: Applies random augmentations to the image, such as slight rotations and brightness adjustments.
       - **Use case**: Can be used to artificially expand the dataset during training by introducing variations in the training images, which improves the model's robustness.

     - **`align_face(image)`**: Aligns the face based on the position of the eyes to standardize the orientation of the face across images.
       - **Use case**: This is important to improve the accuracy of facial recognition, as aligned faces make the encoding process more consistent.

## Detailed Workflow

1. **Registration**:
   - The user selects the registration option.
   - The system captures multiple images of the user's face and stores them.
   - The images are processed and saved in a structured format (`registered_users/username/`).
   - The system immediately trains a face recognition model using the newly captured images and updates the model.

2. **Model Training**:
   - The captured images are loaded, preprocessed, and facial features are extracted using the `face_recognition` library.
   - The extracted features are stored in a dictionary under the user’s name.
   - The dictionary is serialized (pickled) and saved to a file (`face_recognition_model.pkl`).

3. **Authentication**:
   - The user selects the login option and enters their username.
   - The system captures a live image from the webcam.
   - The live image is preprocessed, and facial features are extracted.
   - The extracted features are compared against the stored encodings in the model.
   - If a match is found, the user is authenticated.

## Image Processing Techniques

The system uses several image processing techniques to improve the reliability of face recognition:

1. **Grayscale Conversion**: Reduces computational complexity and focuses on important features.
2. **Image Sharpening**: Enhances the edges and details in the image, improving the quality of feature extraction.
3. **Noise Reduction**: Gaussian blur is applied to remove unnecessary noise and make the image more suitable for feature extraction.
4. **Image Augmentation**: During training, images may be slightly rotated or have their brightness adjusted to simulate real-world conditions and improve model robustness.
5. **Face Alignment**: By aligning the face based on the eyes' positions, the system ensures that the orientation of the face is consistent across different images, which enhances recognition accuracy.

## Model Persistence

- **Pickle Storage**: The face recognition model is stored in a pickled format, allowing easy loading and updating. The file `models/face_recognition_model.pkl` contains the serialized model, which is a dictionary mapping usernames to their face encodings.

## Troubleshooting

### Common Issues

1. **Model Not Found**:
   If you attempt to log in without having registered any user, the system will alert you that no model is found:
   
   ```
   Model not found. Please register at least one user.
   ```

2. **No Face Detected in Live Image**:
   If the system fails to detect a face during authentication, you will be prompted to try again:
   
   ```
   No face landmarks found in the live image. Please try again.
   ```

3. **Failed Authentication**:
   If the live image does not match the registered face data, authentication will fail:
   
   ```
   Authentication failed. Face does not match.
   ```

## Future Enhancements

- **Multi-user support**: Extend the model to handle multiple users with independent face encodings.
- **Security improvements**: Add encryption for model storage to enhance the security of facial data.
- **Improved UI**: Implement a graphical user interface (GUI) to improve user interaction.

This documentation provides clear instructions and explanations, making it easy to use and understand the system. Let me know if you need more details or adjustments
