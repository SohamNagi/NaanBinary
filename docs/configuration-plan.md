# NaanBinary Project Configuration Plan

## Overview

This document explains what we'll do to configure your Android project before starting development. Think of this as setting up your `package.json`, installing dependencies, and configuring your build tools - but for Android.

## Current State vs Target State

### What You Have Now ✅

- Basic Android Studio project with Jetpack Compose
- Single module structure (perfect!)
- Basic Compose dependencies
- MainActivity with "Hello World"

### What We Need to Add 🎯

- Dependencies that make life easier (without heavy boilerplate)
- LLM-friendly networking + JSON for parsing workflows
- Light Gradle configuration
- Only required permissions per feature
- Application class for manual DI setup
- Simple, React-like folder structure

---

## Configuration Steps Explained

### Step 1: Update Gradle Version Catalog (`gradle/libs.versions.toml`)

**What is this?** Think of this as your `package.json` - it lists all dependencies and their versions.

**What we'll add (Core “easy” deps):**

- **Navigation Compose** - Routing (like React Router)
- **DataStore** - Key-value storage (prefs + small counters)
- **OkHttp** + **kotlinx.serialization** - Simple HTTP + JSON for LLM requests
- **Coil** - Image loading in Compose (materials, captures, previews)
- **Coroutines** - Async work (already in most templates)

**Feature-owned deps (add only if a feature needs it):**

- **CameraX + ML Kit** - Only if in-app scanning is implemented
- **Location Services** - Only for GPS reminders
- **Google Calendar API / OAuth** - Only if calendar sync is implemented
- **Room** - Only if JSON/DataStore becomes too limiting

**React equivalent:**

```json
// This is like adding to package.json:
{
  "dependencies": {
    "react-router-dom": "^6.0.0",
    "axios": "^1.0.0"
    // ... etc
  }
}
```

---

### Step 2: Update Build Configuration (`app/build.gradle.kts`)

**What is this?** Like your `webpack.config.js` or `vite.config.js` - configures how the app is built.

**What we'll add:**

- **buildConfig** - For storing API keys (like `.env` files)
- Dependencies from Step 1

**Only add KSP if Room is added later.**

**React equivalent:**

```javascript
// Like configuring webpack plugins:
module.exports = {
  plugins: [
    new HtmlWebpackPlugin(),
    new DefinePlugin({ API_KEY: process.env.API_KEY }),
  ],
};
```

---

### Step 3: Update Root Build File (`build.gradle.kts`)

**What is this?** Project-level configuration that applies to all modules.

**What we'll add:**

- No changes for MVP
- Register KSP only if Room is added later

**React equivalent:**

```javascript
// Like root-level configuration in a monorepo
```

---

### Step 4: Add Permissions (`AndroidManifest.xml`)

**What is this?** Declares what device features your app needs (camera, location, internet).

**What we'll add (core):**

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

**Feature-owned (only if needed):**

```xml
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.POST_NOTIFICATIONS" />
```

**React equivalent:**

```javascript
// Like requesting browser permissions:
navigator.permissions.query({ name: "camera" });
navigator.geolocation.getCurrentPosition();
```

---

### Step 5: Create Application Class (`NaanBinaryApp.kt`)

**What is this?** Entry point for app-wide setup. Runs once when app starts. We'll use it to initialize our manual DI container.

**What we'll create:**

```kotlin
import com.example.naanbinary.di.AppContainer

class NaanBinaryApp : Application() {
    override fun onCreate() {
        super.onCreate()
        // Initialize our manual dependency injection container
        AppContainer.init(this)
    }
}
```

**React equivalent:**

```javascript
// Like your index.js or App.jsx where you set up top-level providers
// or services.
initializeApp(); // conceptually similar

function App() {
  return (
    <AppProviders>
        {/* Your app */}
    </AppProviders>
  );
}
```

---

### Step 6: Create Basic Folder Structure

**What we'll create:**

```
app/src/main/java/com/example/naanbinary/
├── NaanBinaryApp.kt
├── ui/
│   ├── screens/
│   ├── components/
│   ├── navigation/
│   └── theme/
├── data/
│   ├── local/
│   │   ├── storage/          # DataStore + JSON file storage
│   │   └── seed/             # Seeded JSON data
│   ├── remote/               # LLM API client
│   └── repository/
├── di/
│   └── AppContainer.kt
└── util/
```

**React equivalent:**

```
src/
├── pages/
├── components/
├── hooks/
├── api/
├── context/
└── utils/
```

---

## What Happens After Configuration?

Once configuration is complete:

1. **Gradle Sync** - Android Studio downloads all dependencies (like `npm install`)
2. **Ready to Code** - You can start building screens and components

---

## Time Estimates

| Step                        | Time     | Complexity |
| --------------------------- | -------- | ---------- |
| Update libs.versions.toml   | 2 min    | Easy       |
| Update app/build.gradle.kts | 3 min    | Easy       |
| Add permissions             | 1 min    | Easy       |
| Create Application class    | 3 min    | Easy       |
| Create folder structure     | 5 min    | Easy       |
| **Gradle Sync**             | 5-10 min | Automatic  |
| **Total**                   | ~15-20 min | Easy      |

---

## Potential Issues & Solutions

### Issue 1: Gradle Sync Fails

**Cause:** Version conflicts or network issues
**Solution:** Check error message, update versions, or retry

### Issue 2: Build Takes Long Time

**Cause:** First-time dependency download
**Solution:** Wait patiently (like first `npm install`)

### Issue 3: KSP Errors

**Cause:** Missing annotations or incorrect setup for Room
**Solution:** Ensure Room's `@Entity`, `@Dao`, etc. annotations are correct

---

## What You DON'T Need to Worry About

❌ **Multi-module setup** - We're keeping it simple with single module
❌ **Complex build variants** - Just debug and release
❌ **ProGuard rules** - Not needed for development
❌ **Custom Gradle tasks** - Using defaults
❌ **Native code (NDK)** - Pure Kotlin/Java
❌ **Flavors** - Single flavor for now

---

## Comparison to React Setup

| React               | Android                | Purpose               |
| ------------------- | ---------------------- | --------------------- |
| `npm init`          | Android Studio project | Initialize project    |
| `package.json`      | `libs.versions.toml`   | Dependency management |
| `npm install`       | Gradle Sync            | Install dependencies  |
| `webpack.config.js` | `build.gradle.kts`     | Build configuration   |
| `.env`              | `buildConfig`          | Environment variables |
| `src/` folder       | `app/src/main/java/`   | Source code           |
| `public/` folder    | `app/src/main/res/`    | Static resources      |

---

## Next Steps After Configuration

1. **Create Navigation** - Set up bottom nav and routing
2. **Build First Screen** - Start with Home screen
3. **Add Dummy Data** - Test UI without backend
4. **Set Up Database** - Define Room entities
5. **Connect ViewModels** - Wire up state management
6. **Add Features** - Build out each screen
