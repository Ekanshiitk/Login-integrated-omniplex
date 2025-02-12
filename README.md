# Omniplex

This document explains how to set up and run the project locally, the approach taken for implementation, the AI tools used, and the challenges faced along with solutions.

---

## **Project Setup & Local Development**

### **Prerequisites**
Ensure you have the following installed:

- **Node.js (18+)**  
- **npm (latest recommended version)**  
- **Google Cloud SDK** (`gcloud`)  
- **Firebase CLI** (`npm install -g firebase-tools`)  
- **A Google Cloud project with App Engine enabled**  
- **A Firebase project set up with Firestore and Authentication**  

---

### ** Steps to Set Up Locally**
#### ** Clone the Repository**
```bash
git clone https://github.com/your-repo/omniplex.git
cd omniplex
```

#### **② Install Dependencies**
```bash
npm install
```

#### **③ Set Up Environment Variables**
Create a `.env.local` file in the root directory and add:

```plaintext
NEXT_PUBLIC_FIREBASE_API_KEY=your_firebase_api_key
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
NEXT_PUBLIC_FIREBASE_PROJECT_ID=your_project_id
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=your_project.appspot.com
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
NEXT_PUBLIC_FIREBASE_APP_ID=your_app_id
NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID=your_measurement_id
NEXT_PUBLIC_RAPIDAPI_KEY=your_rapidapi_key
```

#### **④ Start the Development Server**
```bash
npm run dev
```
The project will be available at `http://localhost:3000`

---

## 📌 **Project Approach & AI Tools Used**
### **🔹 Authentication & Firestore**
- **Implemented Firebase Authentication** for Google Sign-In & Email/Password login.
- **Firestore used for user data storage**, ensuring seamless authentication state persistence.

### **🔹 AI-Powered Features**
- **Email Validation API:** Uses **RapidAPI (mailok-email-validation)** to verify email authenticity before account creation.
- **Chatbot & AI Features:** Integrated **OpenAI API** to power chatbot interactions.

### **🔹 Cloud Deployment**
- **Google App Engine** (GAE) is used for deployment.
- Firebase & Firestore integrated with **Google Cloud IAM** for authentication management.
- **App.yaml** configuration optimized for Next.js deployment on **Google Cloud Run**.

---

## 🛠 **Challenges Faced & Solutions**
### **❌ Issue: No Account Found Even After Signing Up**
**Cause:** Firestore query logic was incorrect, checking for `email` instead of `uid`.  
**Solution:** Fixed the query using `where("email", "==", email)` and implemented a **Firestore-based email existence check** before user authentication.

### **❌ Issue: Firebase Permission Errors**
**Cause:** Firestore rules restricted read access for authenticated users.  
**Solution:** Updated `firestore.rules`:
```plaintext
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### **❌ Issue: Google Cloud App Deployment Failing**
**Cause:** `app.yaml` was missing required dependencies and configurations.  
**Solution:** Created `app.yaml` with:
```yaml
runtime: nodejs18
service: default
handlers:
  - url: /.*
    script: auto
    secure: always
```

### **❌ Issue: Next.js Build Failing Due to Webpack Errors**
**Cause:** Conflicts with Webpack cache & TypeScript dependencies.  
**Solution:** Modified `next.config.js`:
```js
module.exports = {
  webpack: (config) => {
    config.cache = false;
    return config;
  },
};
```

---

## 🚀 **How to Deploy on Google Cloud**
1. **Build the Project:**
```bash
npm run build
```

2. **Authenticate & Set Project ID:**
```bash
gcloud auth login
gcloud config set project your-project-id
```

3. **Enable App Engine & Deploy:**
```bash
gcloud app create --region=us-central
gcloud app deploy
```

4. **View the Live Project:**
```bash
gcloud app browse
```
Your project will be available at `https://your-project-id.appspot.com`

---

## 💌 **Contact & Contributions**
If you have any issues, feel free to **open an issue** or contact **ekanshbajpai27@gmail.com**.

---
