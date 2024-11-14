# Full Stack E-Commerce Website

This is a Full Stack E-Commerce Website built with **Next.js 14**, **Tailwind CSS**, **Firebase**, **Stripe**, and **Algolia**.

## Features

- User authentication with Firebase
- Product browsing and searching with Algolia
- Shopping cart functionality
- Secure payment processing with Stripe
- Admin panel for managing products, orders, and users

## Technologies Used

- **Frontend**: Next.js 14, Tailwind CSS
- **Backend**: Firebase (Firestore, Authentication)
- **Payment**: Stripe
- **Search**: Algolia

## Installation
apiKey: "AIzaSyAago4NyQnSpMoWBo0J4IR9faCYT0MDvXI",
  authDomain: "purchasey-2c462.firebaseapp.com",
  projectId: "purchasey-2c462",
  storageBucket: "purchasey-2c462.firebasestorage.app",
  messagingSenderId: "288047381803",
  appId: "1:288047381803:web:48f75f15203174de76d96e",
  measurementId: "G-2B6THKBM52"


1. Clone the repository:
   ```bash
   git clone <repository-url>
   ```

2. Navigate to the project directory:
   ```bash
   cd <project-directory>
   ```

3. Install the dependencies:
   ```bash
   npm install
   ```

4. Create a `.env.local` file in the root directory and add the following:

   ```plaintext
   NEXT_PUBLIC_FIREBASE_API_KEY=""
   NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=""
   NEXT_PUBLIC_FIREBASE_PROJECT_ID=""
   NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=""
   NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=""
   NEXT_PUBLIC_FIREBASE_APP_ID=""
   NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID=""
   
   NEXT_PUBLIC_DOMAIN=""
   
   NEXT_PUBLIC_ALGOLIA_APP_ID=""
   NEXT_PUBLIC_ALGOLIA_APP_KEY=""
   
   NEXT_PUBLIC_FIREBASE_SERVICE_ACCOUNT_KEYS='{}'
   ```

5. Start the development server:
   ```bash
   npm run dev
   ```

## Firestore Security Rules

```plaintext
service cloud.firestore {
  match /databases/{database}/documents {
    
    function isAdmin() {
      return exists(/databases/$(database)/documents/admins/$(request.auth.token.email));
    }

    match /admins/{id} {
      allow read: if true;
      allow write: if isAdmin();
    }

    match /brands/{id} {
      allow read: if true;
      allow write: if isAdmin();
    }

    match /categories/{id} {
      allow read: if true;
      allow write: if isAdmin();
    }

    match /collections/{id} {
      allow read: if true;
      allow write: if isAdmin();
    }

    match /orders/{id} {
      allow read: if isAdmin() || request.auth.uid == resource.data.uid;
      allow write: if isAdmin();
    }

    match /products/{id} {
      allow read: if true;
      allow write: if isAdmin();

      match /reviews/{uid} {
        allow read: if true;
        allow write: if isAdmin() || request.auth.uid == uid;
      }
    }

    match /users/{uid} {
      allow read: if isAdmin() || request.auth.uid == uid;
      allow write: if isAdmin() || request.auth.uid == uid;

      match /checkout_sessions/{id} {
        allow read: if isAdmin() || request.auth.uid == uid;
        allow write: if isAdmin() || request.auth.uid == uid;
      }

      match /checkout_sessions_cod/{id} {
        allow read: if isAdmin() || request.auth.uid == uid;
        allow write: if isAdmin() || request.auth.uid == uid;
      }

      match /payments/{id} {
        allow read, write: if false;
      }
    }
  }
}
```

## Firestore Storage Security Rules

```plaintext
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read: if true;
      allow write: if request.auth.uid != null && firestore.exists(/databases/(default)/documents/admins/$(request.auth.token.email));
    }
  }
}
```

## Demo

You can explore the demo of the e-commerce website at [this link](https://docs-reader-store.vercel.app). 

To access the Admin Panel, use the following credentials:

- **Email**: admin@gmail.com
- **Password**: iloveyou

(Note: This demo admin account has read and write permissions disabled for security purposes.)

## Conclusion

This project provides a robust e-commerce solution leveraging modern technologies. Feel free to customize and extend its functionality as needed.
NEXT_PUBLIC_FIREBASE_API_KEY="AIzaSyCSdL70wtMVPsFTqfJC5zxA-ZR1GxL0p6M"
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN="e-commerce-d75b0.firebaseapp.com"
NEXT_PUBLIC_FIREBASE_PROJECT_ID="e-commerce-d75b0"
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET="e-commerce-d75b0.appspot.com"
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID="869034672196"
NEXT_PUBLIC_FIREBASE_APP_ID="1:869034672196:web:329dd71ec73fce4793fc11"
NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID="G-6DJ9TWMBPQ"
https://console.firebase.google.com/v1/r/project/e-commerce-d75b0/firestore/indexes?create_composite=Ck9wcm9qZWN0cy9lLWNvbW1lcmNlLWQ3NWIwL2RhdGFiYXNlcy8oZGVmYXVsdCkvY29sbGVjdGlvbkdyb3Vwcy9vcmRlcnMvaW5kZXhlcy9fEAEaBwoDdWlkEAEaEwoPdGltZXN0YW1wQ3JlYXRlEAIaDAoIX19uYW1lX18QAg

