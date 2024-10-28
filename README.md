# Biometric-Authentication-System

# Biometric Authentication System

This is a simple biometric authentication system that uses facial recognition for user registration and login. The application captures images of users' faces for training and then authenticates users based on their facial features.

## Table of Contents
- [Features](#features)
- [Requirements](#requirements)
- [Setup](#setup)
- [How to Run](#how-to-run)
- [How It Works](#how-it-works)
- [License](#license)

## Features
- User registration through facial recognition.
- Live authentication using the webcam.
- Captures multiple images during registration for better accuracy.

## Requirements
- Python 3.x
- OpenCV
- face_recognition
- NumPy

You can install the required libraries using pip:

```bash
pip install opencv-python face_recognition numpy
```

## Setup

1. Clone the repository or download the code.
2. Navigate to the project directory.
3. Create the following directories to store registered users and models:
   * `registered_users/` - This folder will contain images of registered users.
   * `models/` - This folder will store the trained face recognition model.

## How to Run

1. Open a terminal and navigate to the project directory.
2. Run the main program:

   ```bash
   python main.py
   ```
3. Follow the prompts to either register a new user or log in with an existing user.
