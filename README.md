## JIIT StudySync

JIIT StudySync is an Android application built in Kotlin for JIIT students to access previous year question papers (PYQs), curated notes, and other study resources across semesters. It centralizes PYQs and verified study material, making exam preparation faster and more structured.

### Features

- **PYQ browsing**
  - View previous year question papers by subject, year, and exam type (T1/T2/T3).
  - See front and back pages of question papers.
  - Subject list and PYQ metadata are loaded from Firebase Realtime Database and Firebase Storage.

- **Notes and study material**
  - Browse subjects and open notes (PDFs) stored in Firebase Storage.
  - In–app PDF viewing using `android-pdf-viewer`.
  - Intended to host only **verified** notes to reduce noise and redundancy.

- **Uploads**
  - Upload PYQs (images) with subject code/name, year, and exam type.
  - Upload notes (PDFs) linked to a subject.

- **Analysis & forum (placeholders)**
  - `COAnalysisPage` and `ForumPage` screens exist as static, image–based placeholders for:
    - CO / outcome–oriented analysis of PYQs.
    - A basic discussion forum for doubt–clearing and resource sharing.
  - These can be extended later to implement actual analytics, predictions, and conversation.

- **Authentication & user profiles**
  - Email/password authentication using Firebase Auth.
  - Email verification and password reset flow.
  - User profile data (name, enrollment, branch, etc.) stored in Realtime Database under `Users/{uid}`.

### Tech stack

- **Language**: Kotlin
- **UI**: XML layouts with View Binding (no Jetpack Compose)
- **Architecture**: Activity–driven navigation with logic in activities (no formal MVVM/MVI layer yet)
- **Backend**: Firebase
  - Firebase Authentication
  - Firebase Realtime Database
  - Firebase Storage
- **Libraries**
  - AndroidX core, appcompat, material components, constraintlayout
  - Navigation fragment/UI KTX (used minimally; main flow is via activities/intents)
  - `com.github.barteksc:android-pdf-viewer:2.8.2` for PDF viewing
  - `com.github.bumptech.glide:glide:4.16.0` for image loading

### Project structure

- **Root project**: `JIITStudySync`
- **Main module**: `app`
  - `app/src/main/AndroidManifest.xml` – activities and launcher configuration.
  - `SplashScreen.kt` – launcher activity.
  - `LoginPage.kt`, `SignupPage.kt`, `ResetPassword.kt` – auth flow.
  - `UserDashboard.kt` – main hub with bottom navigation for PYQs, Notes, and Forum.
  - `PYQActivity.kt`, `SubjectsPYQ.kt`, `PYQFrontPageView.kt`, `PYQBackPageView.kt` – PYQ browsing.
  - `NotesActivity.kt`, `SubjectNotes.kt`, `NotesViewerActivity.kt` – notes browsing & viewing.
  - `UploadPYQ.kt`, `UploadNotes.kt` – upload flows.
  - `SubjectDetails.kt`, `UserDetails.kt`, `NotesFile.kt` – data models.

### Build & environment

- **Gradle Android plugin**: 8.3.2 (via version catalog)
- **Kotlin**: 1.9.0
- **compileSdk**: 34
- **targetSdk**: 34
- **minSdk**: 26
- **Application ID**: `com.minorproject.jiitstudysync`

### Firebase setup

1. **Create/attach a Firebase project**
   - Enable **Authentication (Email/Password)**.
   - Enable **Realtime Database**.
   - Enable **Storage**.

2. **`google-services.json`**
   - Place your Firebase config file at:
     - `JIITStudySync/app/google-services.json`
   - The repo already contains a `google-services.json`; replace it if you want to use your own Firebase project.

3. **Database structure (expected)**
   - `Users/{uid}` – user profile data.
   - `Subjects/{code}` – subject metadata, including:
     - `name`, `code`
     - optional `notes` list (array of `NotesFile` objects with `downloadUrl`, `fileName`).

4. **Storage structure (expected)**
   - PYQ uploads: `Uploaded_PYQs/{subjectCode}/{fileName}`
   - PYQ static store (read side): `Stored_PYQs/{subjectCode}/{imageName}.jpg`
   - Notes PDFs: `Uploaded_Notes/{subjectCode}/{fileName}`

   In production, you may want to unify or automate movement between `Uploaded_PYQs` and `Stored_PYQs`.

### Running the app

1. **Clone the repository**

   ```bash
   git clone <this-repo-url>
   cd JIIT-StudySync-main/JIITStudySync
   ```

2. **Open in Android Studio**
   - Use **Android Studio Flamingo or newer** (compatible with AGP 8.3+).
   - Let Gradle sync finish.

3. **Configure Firebase**
   - Ensure `google-services.json` is present under `app/`.
   - Verify that your Firebase project has Auth, Realtime Database, and Storage enabled.

4. **Run**
   - Select the `app` configuration.
   - Run on a device or emulator with **API 26+**.

### Future improvements

- Implement real **PYQ analysis and outcome prediction** (e.g., most frequently asked COs/topics, predicted weightage).
- Turn `ForumPage` into an actual **discussion forum** with threads and comments.
- Introduce a clearer architecture (e.g., **MVVM** with ViewModels and a repository layer).
- Add offline caching and better error handling for network/storage failures.

### License

Add your preferred license here (e.g., MIT, Apache 2.0) if you intend to open source the project.

