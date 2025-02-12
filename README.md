# Omniplex
This project enhances and deploys the Omniplex application by updating the login page UI to match Claude.ai’s interface design, integrating third-party APIs, and deployment. The primary focus is on UI simplicity, responsiveness, and user-friendly aesthetics.
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
git clone https://github.com/Ekanshiitk/Login-integrated-omniplex.git
cd Login-integrated-omniplex
```

#### ** Install Dependencies**
```bash
npm install
```

#### ** Set Up Environment Variables**
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

#### ** Start the Development Server**
```bash
npm run dev
```
The project will be available at `http://localhost:3000`



---

## **Project Approach & AI Tools Used**

## Approach to the Tasks

### UI Enhancement
The login page is designed to replicate Claude.ai’s authentication flow. When a user clicks "Sign in" in the sidebar, a centered modal appears with options for Google and email authentication. The page follows a dark-themed UI with minimalist typography, ensuring a seamless user experience. Email validation is mandatory before signing in, preventing incorrect submissions. The UI remains highly responsive, adapting to both mobile and desktop layouts effortlessly.

- Designed the login page to closely resemble Claude.ai’s UI.
- The login page appears as a centered modal when the "Sign in" button is clicked from the sidebar.
- The design is minimalistic, featuring a clean black-and-white theme with a focus on accessibility.
- The Google and email authentication buttons are prominent, ensuring easy navigation.
- Used Tailwind CSS to maintain responsiveness across different screen sizes.
- Integrated a validation step where the email must be verified before proceeding with authentication.

### API Integration
- Integrated a public API from RapidAPI to validate user email before proceeding with authentication.
- The "Validate Email" button triggers an API request to check the legitimacy of the entered email.
- Upon successful validation, the "Continue with Email" button is enabled.

### ** Authentication & Firestore**
- **Implemented Firebase Authentication** for Google Sign-In & Email/Password login.
- **Firestore used for user data storage**, ensuring seamless authentication state persistence.

### ** AI-Powered Features**
- **Chatbot & AI Features:** Integrated **OpenAI API** to power chatbot interactions.

### ** Cloud Deployment**
- **Google App Engine** (GAE) is used for deployment.
- Firebase & Firestore integrated with **Google Cloud IAM** for authentication management.
- **App.yaml** configuration optimized for Next.js deployment on **Google Cloud Run**.
- Enabled necessary Google Cloud APIs (`cloudbilling.googleapis.com`, `appengine.googleapis.com`).
- Could not successfully deploy due to some build issues in other parts of the code.
  
---

## AI Tools Used
- ChatGPT: Assisted in troubleshooting Next.js issues and improving Firebase authentication.
- Claude.ai: Used for UI design references and layout structuring.
- ESLint & Prettier: Ensured code quality and maintained consistent styling.
  
---

## **Challenges Faced & Solutions**
### ** Issue: No Account Found Even After Signing Up**
**Cause:** Firestore query logic was incorrect, checking for `email` instead of `uid`.  
**Solution:** Fixed the query using `where("email", "==", email)` and implemented a **Firestore-based email existence check** before user authentication.

### ** Issue: Firebase Permission Errors**
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

### ** Issue: Google Cloud App Deployment Failing**

### ** Issue: Next.js Build Failing Due to Webpack Errors**
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
