# Attendance
# Mobile Barcode Attendance Tracker

A lightweight, web-based application designed to streamline student attendance tracking using 1D barcode scanning. This project utilizes a device's camera to scan student IDs, instantly logging their presence into a real-time cloud database. 

## 🚀 Features

* **Live Barcode Scanning:** Uses the device's rear camera to quickly read 1D barcodes from student ID cards.
* **Real-Time Database:** Integrates with Firebase Firestore to pull a live student roster and instantly log attendance data.
* **Timed Sessions:** Features a built-in 6-minute countdown timer for structured attendance taking.
* **Responsive UI:** Clean, mobile-first design with built-in Light and Dark modes.
* **Instant Feedback:** Visual and text-based toast notifications confirm when a student is successfully marked present.
* **Session Summaries:** Automatically generates a final roster summary (Present vs. Absent) when the timer expires or the session is ended manually.

## 🛠️ Technology Stack

* **Frontend:** HTML5, CSS3, Vanilla JavaScript
* **Backend / Database:** [Firebase Cloud Firestore](https://firebase.google.com/products/firestore) (Compat SDK)
* **Barcode Decoding:** [Quagga2](https://github.com/ericblade/quagga2) (JavaScript barcode scanner library)

## 📋 Prerequisites & Setup

To run this project locally or deploy it via GitHub Pages, you need to configure your own Firebase project.

### 1. Firebase Configuration
1. Go to the [Firebase Console](https://console.firebase.google.com/) and create a new project.
2. Add a Web App to your project to generate a Firebase Config object.
3. Open `index.html` and locate the `FIREBASE_CONFIG` block at the top of the `<script>` section.
4. Replace the placeholder values with your actual Firebase API keys.

### 2. Database Structure (Firestore)
Ensure your Firestore Database is initialized and set up with the following collections:

* **`Students` (Collection):**
  * Document ID: The student's barcode string (e.g., `STU-12345` or `987654321`).
  * Field: `name` (Type: string, Value: e.g., `Jane Doe`).
* **`sessions` (Collection):**
  * You do not need to manually create documents here; the app will auto-generate them when a session starts. 

### 3. Permissions
* Set your Firestore Rules to allow read and write access for your testing environment. 
* *Note: If using `allow read, write: if true;`, remember to secure your rules before using the app in a production environment.*

## 📱 Usage

1. Open the application on a mobile device or a browser with a webcam.
2. **Grant Camera Permissions** when prompted by the browser.
3. Tap **"Begin 6-minute session"** to start the timer and activate the camera.
4. Hold a student ID barcode steady within the on-screen reticle. 
5. The app will log the scan, update the live roster, and push the timestamp to Firebase.
6. When the session ends, review the final attendance summary.

## ⚠️ Troubleshooting

* **Camera Not Working:** Ensure you are accessing the site via `HTTPS` (required for camera access) and that your browser permissions allow camera use.
* **Not Reading Barcodes:** Ensure the barcode is well-lit and held steady. The app is configured to read standard 1D barcodes (Code 128, Code 39, UPC, etc.), not QR codes.
* **Data Not Saving:** Double-check that your `FIREBASE_CONFIG` is correct and that your Firestore rules allow write access.
