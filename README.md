# PARKIN — GitHub Pages Manual Deployment Bundle

This folder contains the complete, pre-compiled static web build of **PARKIN** ready to upload and publish to GitHub Pages.

---

## What is Inside This Folder

* **`index.html`**: The main entry page configured for GitHub Pages.
* **`.nojekyll`**: Tells GitHub Pages to bypass Jekyll processing (required for Flutter web assets).
* **`404.html`**: Route fallback handler to prevent blank screen 404 errors when refreshing.
* **`main.dart.js`**: The compiled Flutter application logic.
* **`flutter.js` & `flutter_bootstrap.js`**: Flutter web engine loaders.
* **`assets/` & `canvaskit/`**: WebAssembly engine, icons, and fonts.

---
> **PARKIN** is a modern, cross-platform smart parking management system engineered for college campuses. It delivers real-time bay occupancy monitoring, 1-tap smart reservations for students and faculty, and a high-tech gate security command center for campus guards — running with **100% \$0 serverless architecture on the Firebase Spark (free) tier**.

---

## 🌟 Highlights & Architecture

- ⚡ **Zero Cloud Cost (\$0 Forever):** Engineered specifically for the **Firebase Spark Free Tier** — zero Cloud Functions, zero paid third-party SDKs, and zero server hosting costs.
- 🔒 **Atomic Concurrency Control:** Employs client-orchestrated Firestore transactions (`runTransaction`) to guarantee slot allocation idempotency and eliminate double-booking race conditions during peak morning campus rushes.
- 🛡️ **Role-Based Security Rules:** Strict server-side Firestore security rules enforce role privileges (`student`, `faculty`, `guard`, `admin`), preventing privilege self-escalation and securing global occupancy counters.
- 📱 **Dual-Portal Interface:**
  1. **Student & Faculty Mobile App:** Instant 1-tap booking, live occupancy meter, ticket-style digital parking passes, and booking management.
  2. **Security Guard Web/Desktop Dashboard:** High-contrast gate command center with instant license plate search, booking validation, and 1-tap vehicle check-in / check-out.
- 🚀 **Multi-Platform Ready:** Compiles seamlessly to **Web (GitHub Pages)**, **iOS**, and **Android** from a single Flutter codebase.

---

## 📸 Core Interfaces & Capabilities

### 1. 🎓 Student & Faculty Portal
- **Live Campus Occupancy Meter:** High-visibility gradient telemetry card displaying total bays, physically occupied bays, and live available bays.
- **1-Tap Smart Reservation:** Eliminates cumbersome manual slot hunting. Automatically assigns the nearest optimal bay (North Bay / South Bay) with custom duration selection (1h, 2h, 4h, full day).
- **Digital Parking Pass:** Ticket-style pass displaying designated Slot ID, reserved window, vehicle license plate, and real-time status (`Booked`, `Active`, `Completed`, `Cancelled`).
- **Role Distinction:** Dynamic visual branding distinguishing student memberships from faculty priority passes.

### 2. 🛡️ Guard & Gate Command Center
- **High-Tech Command Console:** Purpose-built dark-mode layout for campus entry/exit kiosks and tablet devices.
- **Real-Time Verification:** Instant lookup by vehicle license plate number or Student/Faculty ID.
- **Idempotent Entry & Exit:** 1-tap vehicle check-in triggers atomic slot occupation; check-out atomically frees the bay and marks the session completed.
- **Occupancy Sync:** Automatically synchronizes physical gate transitions with the public campus counter.

---

## 🏗️ System Architecture

```
                      ┌────────────────────────────────────────┐
                      │            Firebase Auth               │
                      │       (Email / Password Auth)          │
                      └──────────────────┬─────────────────────┘
                                         │
                 ┌───────────────────────┴───────────────────────┐
                 ▼                                               ▼
   ┌───────────────────────────┐                   ┌───────────────────────────┐
   │  Student / Faculty App    │                   │   Guard Command Center    │
   │   (iOS / Android / Web)   │                   │    (Web / Tablet Kiosk)   │
   ├───────────────────────────┤                   ├───────────────────────────┤
   │ • Live Occupancy Telemetry│                   │ • Gate Occupancy Monitor  │
   │ • 1-Tap Auto-Allocation   │                   │ • Plate / College ID Search│
   │ • Digital Parking Pass    │                   │ • Atomic Vehicle Entry    │
   │ • Booking Management      │                   │ • Atomic Vehicle Exit     │
   └─────────────┬─────────────┘                   └─────────────┬─────────────┘
                 │                                               │
                 │      Firestore Atomic Transactions            │
                 │      & Real-time Snapshot Streams             │
                 ▼                                               ▼
   ┌───────────────────────────────────────────────────────────────────────────┐
   │                          Cloud Firestore (Spark)                          │
   │  • /users/{uid}             • /slots/{slotId}                             │
   │  • /bookings/{bookingId}    • /metadata/occupancy                         │
   └───────────────────────────────────────────────────────────────────────────┘
```

---

## 📁 Project Structure

```
parkin/
├── .github/workflows/       # Automated CI/CD workflows
├── android/                 # Native Android project configuration
├── ios/                     # Native iOS project & Runner workspace
├── web/                     # Web entry point & metadata
├── docs/                    # Compiled production build for GitHub Pages
├── firestore.rules          # Strict Spark-tier role-based access rules
├── firebase.json            # Firebase CLI platform mappings
├── lib/
│   ├── firebase_options.dart# Platform-specific Firebase credentials
│   ├── main.dart            # Application entry point & theme configuration
│   ├── models/
│   │   ├── booking_model.dart    # Booking lifecycle & serialization
│   │   ├── occupancy_model.dart  # Campus capacity & availability counters
│   │   ├── parking_slot.dart     # Physical slot & zone representation
│   │   └── user_model.dart       # User profiles & role enum (student, faculty, guard)
│   ├── screens/
│   │   ├── auth/
│   │   │   ├── auth_gate.dart    # Real-time auth state & role router
│   │   │   └── auth_screen.dart  # Ambient glow sign-in & registration UI
│   │   ├── guard/
│   │   │   └── guard_dashboard_screen.dart # Security command center
│   │   └── student/
│   │       ├── booking_dialog.dart         # 1-tap slot reservation modal
│   │       └── student_home_screen.dart    # Student & faculty dashboard
│   ├── services/
│   │   ├── auth_service.dart     # Auth provider & profile stream listener
│   │   └── parking_service.dart  # Atomic transactions & parking engine
│   └── widgets/
│       └── occupancy_card.dart   # Live telemetry dashboard card
└── test/
    ├── concurrency_idempotency_test.dart # Transaction race-condition tests
    ├── models_test.dart                  # Model serialization tests
    ├── parking_service_test.dart         # Core booking logic tests
    └── widget_test.dart                  # UI smoke tests
```

---

## 🔐 Security & Free-Plan Guarantee

| Service | Free Tier Allowance | How PARKIN Stays Within Limits |
| :--- | :--- | :--- |
| **Cloud Firestore** | 50,000 reads/day, 20,000 writes/day | Uses single-document `/metadata/occupancy` for live capacity counters; bounds all list queries with strict `.limit(20)`. |
| **Firestore Transactions** | Included in Spark plan | All state transitions (slot reservations, check-ins, check-outs) run through atomic client transactions without external servers. |
| **Firebase Auth** | Unlimited email/password accounts | Handles user credential lifecycle entirely client-side. |
| **Cloud Functions** | \$0 (Excluded / Blocked) | Zero server functions used — avoids Blaze upgrade completely. |
| **Hosting** | GitHub Pages (100% Free) | Web build hosted on GitHub Pages via the `/docs` directory. |

---

## 🚀 Getting Started

### Prerequisites

Ensure you have the following installed on your machine:
- [Flutter SDK](https://docs.flutter.dev/get-started/install) (`^3.13.0` or higher)
- [Dart SDK](https://dart.dev/get-dart)
- [Git](https://git-scm.com)
- [Xcode](https://developer.apple.com/xcode/) (for iOS simulator/device)
- [Android Studio](https://developer.android.com/studio) (for Android emulator/device)

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/cyber-meowfia/parkin.git
   cd parkin
   ```

2. **Install Flutter dependencies:**
   ```bash
   flutter pub get
   ```

3. **Configure Firebase:**
   PARKIN includes pre-configured `lib/firebase_options.dart`, `android/app/google-services.json`, and `ios/Runner/GoogleService-Info.plist`.
   
   To use your own Firebase project:
   ```bash
   flutterfire configure
   ```

4. **Deploy Firestore Security Rules:**
   Deploy the included [`firestore.rules`](firestore.rules) to your Firebase project:
   ```bash
   firebase deploy --only firestore:rules
   ```

---

## 💻 Running the Application

### Launch on Web (Chrome)
```bash
flutter run -d chrome
```

### Launch on iOS Simulator
```bash
# Boot an iOS Simulator
open -a Simulator

# Run the app
flutter run -d ios
```

### Launch on Android Emulator
```bash
flutter run -d android
```

---

## 🧪 Testing Suite

PARKIN comes with automated test suites covering models, booking logic, and transaction idempotency:

```bash
# Run all unit and integration tests
flutter test
```

### Test Coverage Highlights:
- **`test/concurrency_idempotency_test.dart`**: Validates that repeated gate check-ins or simultaneous bookings do not lead to corrupted occupancy counts.
- **`test/models_test.dart`**: Ensures JSON serialization and deserialization integrity across all domain models.
- **`test/parking_service_test.dart`**: Verifies automated bay allocation, cancellation eligibility, and role permissions.

---

## 👥 User Roles & Permissions

| Role | Interface | Permissions |
| :--- | :--- | :--- |
| **Student** | Student Portal | View occupancy, reserve slots, view active passes, cancel own bookings. |
| **Faculty** | Student Portal (Priority) | All student permissions with dedicated faculty branding and priority allocation. |
| **Guard** | Gate Command Center | View live gate telemetry, verify reservations, check in vehicles, check out vehicles. |
| **Admin** | Command Center & Management | Full administrative access, modify bay configurations, manage user roles. |

> *Note: For security, user roles cannot be self-assigned through the registration UI. Roles default to `student`/`faculty` and can be elevated to `guard` or `admin` directly via the Firebase Console.*

---

## 🌐 Deploying to GitHub Pages

The web application is structured to deploy directly via GitHub Pages:

1. Build the web release targeting the `docs/` output directory:
   ```bash
   flutter build web --release --base-href "/parkin/" --output docs
   ```
2. Commit and push the `docs/` folder:
   ```bash
   git add docs/
   git commit -m "Deploy latest web release to GitHub Pages"
   git push origin main
   ```
3. In your GitHub repository:
   - Go to **Settings** > **Pages**
   - Under **Build and deployment** > **Source**, choose **Deploy from a branch**
   - Select branch **`main`** and folder **`/docs`**, then click **Save**.

Your live app will be accessible at:  
👉 **`https://<your-username>.github.io/parkin/`**

---

## 📄 License

This project is open-source under the [MIT License](LICENSE).

---

<p align="center">
  Built with ❤️ using <b>Flutter</b> and <b>Firebase</b>
</p>
