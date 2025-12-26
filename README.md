# React App to Android APK Conversion Guide

## Complete Step-by-Step Tutorial for Converting React Web Apps to Android APK

---

## Prerequisites (One-Time Setup)

### Required Software
- **Node.js** - Download from nodejs.org (includes npm)
- **Android Studio** - Download from developer.android.com/studio
- **Yarn** (recommended) - Install via: `npm install -g yarn`
- **Git** (optional) - For cloning repositories

### System Requirements
- Minimum 4GB RAM (8GB recommended)
- 10GB free disk space for Android SDK
- Windows 10/11, macOS, or Linux

---

## Step 1: Clone and Navigate to Project

```bash
git clone <repository-url>
cd project-name/frontend
```

**Important:** Always work in the `frontend` folder where `package.json` is located.

---

## Step 2: Install Dependencies

### Option A: Using Yarn (Recommended)
```bash
yarn install
```

### Option B: Using npm with Legacy Peer Dependencies Flag
```bash
npm install --legacy-peer-deps
```

**Note:** Use `--legacy-peer-deps` to bypass peer dependency conflicts (very common in React projects).

### Verify Installation
```bash
dir
```
You should see:
- `package.json`
- `node_modules` folder
- `src` folder
- `public` folder

---

## Step 3: Install Capacitor

Capacitor is a framework that wraps your web app into native mobile applications.

```bash
npm install @capacitor/core @capacitor/cli
```

---

## Step 4: Initialize Capacitor

```bash
npx cap init
```

When prompted, enter:
- **App name:** `Your App Name` (e.g., "Pediatrics On The Go")
- **Package ID:** `com.yourcompany.appname` (e.g., "com.pediatrics.onthego")
  - Use lowercase letters only
  - No spaces or special characters
  - Reverse domain notation (com.company.app)
- **Web asset directory:** `build` (for Create React App) or `dist` (for Vite/Next.js)

### Result
Creates two files:
- `capacitor.config.json`
- `ionic.config.json`

---

## Step 5: Install Android Platform

```bash
npm install @capacitor/android --legacy-peer-deps
npx cap add android
```

This creates an `android` folder with the native Android project structure.

---

## Step 6: Build Your React App

### Using Yarn
```bash
yarn build
```

### Using npm
```bash
npm run build
```

**Expected Output:**
- "Creating an optimized production build..."
- Progress percentages
- "Compiled successfully!"
- File size summary

**Result:** Creates a `build` folder with your compiled app.

**Duration:** 2-5 minutes depending on project size.

---

## Step 7: Sync Web Assets to Android

```bash
npx cap sync
```

This copies your built React app into the Android project.

**Output:** "✔ Synced successfully"

---

## Step 8: Open Android Studio

```bash
npx cap open android
```

Android Studio will launch with your project loaded.

---

## Step 9: Build APK in Android Studio

### 9.1 Wait for Gradle Sync
- Look at the bottom of Android Studio
- You'll see "Syncing...", "Building...", or a progress bar
- **Wait for this to complete** (10-15 minutes first time, 1-2 minutes after)
- Status shows "Gradle sync finished" or "BUILD SUCCESSFUL"

### 9.2 Build the APK
1. Click **Build** in the top menu
2. Select **Build Bundle(s) / APK(s)**
3. Click **Build APK(s)**

### 9.3 Wait for Build
- Another progress bar appears at bottom
- Wait for "BUILD SUCCESSFUL" message
- A notification pops up: "APK(s) generated successfully"

### 9.4 Locate Your APK
- Click the **"locate"** link in the notification
- OR navigate manually to: `android/app/build/outputs/apk/debug/`
- **Your APK file:** `app-debug.apk`

---

## Using Your APK

### Test on Android Device
1. Enable "USB Debugging" on your phone (Settings → Developer Options)
2. Connect phone via USB cable
3. In Android Studio: **Run → Run 'app'** (or press Shift+F10)
4. Select your device

### Share APK with Others
1. Copy `app-debug.apk` from the output folder
2. Send via email, WhatsApp, Google Drive, etc.
3. Recipients install by opening the file on their Android device

### Upload to Google Play Store
1. Generate a **release APK** (not debug version)
2. Sign it with your keystore
3. Create developer account at play.google.com
4. Follow upload guidelines

---

## Common Issues & Solutions

### Error: "npm is not recognized"
**Problem:** Node.js not installed
**Solution:** 
- Download and install Node.js from nodejs.org
- Restart computer after installation

### Error: "PowerShell execution policy"
**Problem:** Windows PowerShell security restriction
**Solution:**
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```
OR switch to Command Prompt in VS Code

### Error: "ERESOLVE unable to resolve dependency tree"
**Problem:** Package version conflicts
**Solution:**
```bash
npm install --legacy-peer-deps
npm install @capacitor/android --legacy-peer-deps
```

### Error: "Cannot find module 'dotenv'" or other missing modules
**Problem:** Dependencies not fully installed
**Solution:**
```bash
yarn add dotenv
yarn add react-scripts
```
OR reinstall:
```bash
rm -r node_modules
yarn install
```

### Error: "Missing appId"
**Problem:** `capacitor.config.json` not configured
**Solution:** Check file has:
```json
{
  "appId": "com.yourcompany.appname",
  "appName": "Your App Name",
  "webDir": "build",
  "bundledWebRuntime": false
}
```

### Error: "Could not find web assets directory"
**Problem:** `build` folder doesn't exist or wrong path
**Solution:**
1. Run `yarn build` to create build folder
2. Verify `webDir` in `capacitor.config.json` matches (usually "build")

### Error: "Android platform has not been added yet"
**Problem:** Missing android folder
**Solution:**
```bash
npx cap add android
npx cap sync
```

### Wrong Directory Errors
**Problem:** Running commands from main project folder instead of `frontend`
**Solution:**
```bash
cd frontend
# Now run your commands
```

---

## Important Configuration Files

### capacitor.config.json
Located in `frontend` folder. Customizable settings:

```json
{
  "appId": "com.pediatrics.onthego",
  "appName": "Pediatrics On The Go",
  "webDir": "build",
  "bundledWebRuntime": false,
  "server": {
    "url": "http://your-backend-url.com",
    "cleartext": true
  }
}
```

### Backend API Configuration
If your app connects to a Python/Node backend:
1. Host backend on cloud service (Render, Railway, PythonAnywhere, etc.)
2. Update API URLs in React code to use hosted backend
3. Change from `http://localhost:5000` to `https://your-backend.com`

---

## Yarn vs npm Decision Tree

### Use Yarn If:
- Project has `yarn.lock` file
- `package.json` contains: `"packageManager": "yarn@..."`
- Team recommends yarn

### Use npm If:
- Project has `package-lock.json` file
- No `yarn.lock` present
- Default Node.js package manager

### How to Check
```bash
dir
# Look for either yarn.lock or package-lock.json
```

---

## Quick Command Reference

### Initial Setup
```bash
cd frontend
yarn install
npm install @capacitor/core @capacitor/cli
npx cap init
npm install @capacitor/android --legacy-peer-deps
npx cap add android
```

### Build and Deploy to Android Studio
```bash
yarn build
npx cap sync
npx cap open android
```

### After Code Changes
```bash
yarn build
npx cap sync
# Then rebuild APK in Android Studio
```

### Test on Connected Device
```bash
npx cap run android
```

### View Logs
```bash
npx cap run android -- --verbose
```

---

## Key Tips for Success

### 1. Patience During First Setup
- First Gradle sync: 10-15 minutes (it downloads SDK files)
- Subsequent builds: 1-2 minutes
- Don't close Android Studio during sync

### 2. Always Use Correct Directory
- All commands run from `frontend` folder
- Verify: `dir` should show `package.json`

### 3. Use Legacy Peer Dependencies
- Always add `--legacy-peer-deps` when installing packages
- Saves troubleshooting time for React projects

### 4. Backend Hosting
- Mobile app can't reach localhost
- Host Python backend on cloud service
- Update API endpoints in React code

### 5. Version Compatibility
- Keep Node.js updated
- Use Android Studio latest stable version
- Update Capacitor: `npm install @capacitor/core@latest`

### 6. Testing Strategy
1. Test web version first: `yarn start`
2. Build for Android: `yarn build`
3. Test debug APK on device
4. Build release version for distribution

### 7. Performance Optimization
- Remove unused dependencies: `npm prune`
- Check bundle size: `npm run build` shows file sizes
- Lazy load images and components

---

## Development Workflow for Future Updates

### When You Update Your React Code

```bash
# 1. Make code changes in VS Code
# 2. Test locally
yarn start

# 3. When ready to build APK
yarn build

# 4. Sync to Android
npx cap sync

# 5. Rebuild in Android Studio
npx cap open android
# Then: Build → Build APK(s)
```

### Shortcuts
- **Rebuild without reopening Android Studio:** Just run `yarn build && npx cap sync`
- **Make small changes:** Just sync and rebuild, don't need to reinitialize

---

## Additional Resources

### Official Documentation
- Capacitor: https://capacitorjs.com
- Android Studio: https://developer.android.com/studio
- React: https://react.dev

### Useful Tools
- **ngrok:** Expose local backend for testing
- **Firebase Hosting:** Free backend hosting option
- **Heroku/Railway:** Easy Python backend deployment

### Common Tasks

#### Change App Icon
1. Replace `android/app/src/main/ic_launcher-*.png` files
2. Use Android icon guidelines (108x108dp minimum)

#### Change App Name
Update `capacitor.config.json`:
```json
"appName": "New App Name"
```

#### Change Package ID (Before Building)
Update `capacitor.config.json`:
```json
"appId": "com.newcompany.newappname"
```

---

## Troubleshooting Checklist

- [ ] Node.js and npm installed (`node -v`, `npm -v`)
- [ ] Android Studio installed and SDKs downloaded
- [ ] Working in `frontend` folder (check with `dir`)
- [ ] All dependencies installed (`yarn install` or `npm install --legacy-peer-deps`)
- [ ] React app builds successfully (`yarn build`)
- [ ] `build` folder exists
- [ ] `capacitor.config.json` has correct `appId` and `webDir`
- [ ] Gradle sync completed (wait 10-15 minutes)
- [ ] Using latest Android Studio version
- [ ] USB debugging enabled on test device (if testing on phone)

---

## Version Information

**Last Updated:** December 26, 2025

**Tested With:**
- Node.js v24.12.0
- npm v11.6.2
- Yarn v1.22.22
- Android Studio 2024.1+
- Capacitor 6.0+
- React 19.0+

---

## Support & Next Steps

### If You Get Stuck
1. Read the error message carefully
2. Check the "Common Issues & Solutions" section
3. Search the error on GitHub issues
4. Check official Capacitor documentation

### For iOS Support
Once Android builds successfully, convert to iOS:
```bash
npm install @capacitor/ios
npx cap add ios
npx cap open ios
```

### Publishing to Google Play Store
1. Generate signed release APK
2. Create Google Play developer account
3. Follow app store guidelines
4. Submit for review

---

**Good luck with your app! 🚀**
