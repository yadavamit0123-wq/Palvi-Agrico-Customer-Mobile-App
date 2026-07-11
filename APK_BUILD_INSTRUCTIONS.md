# Release APK Build (GitHub Actions)

Ye project me `.github/workflows/build-apk.yml` add kar diya gaya hai. Isse GitHub Actions
apne aap ek release APK build karega — aapke local machine pe Flutter/Android SDK
install karne ki zaroorat nahi hai.

## Steps

1. Is folder ko GitHub par ek naye (ya existing) repository me push karein:
   ```
   git init
   git add .
   git commit -m "Add release APK build workflow"
   git branch -M main
   git remote add origin <aapka-repo-url>
   git push -u origin main
   ```
2. GitHub repo par jaakar **Actions** tab open karein.
3. "Build Release APK" workflow dikhega — ye push par khud chal jaata hai, ya
   "Run workflow" button se manually bhi trigger kar sakte hain.
4. Build complete hone ke baad, us run ke "Artifacts" section me `release-apk` milega —
   yahi aapki `app-release.apk` file hai. Download karke seedha device pe install kar sakte hain.

## Signing (important)

Abhi ye release build **debug signing key** se sign hota hai (`android/app/build.gradle.kts`
me `signingConfig = signingConfigs.getByName("debug")`). Ye APK install aur test karne
ke liye theek hai, lekin Google Play Store par upload karne ke liye aapko apni
**real keystore** se sign karna hoga.

Real keystore add karne ke liye:

1. Apna `.jks`/`.keystore` file base64 me convert karein:
   ```
   base64 -w0 your-release-key.jks > keystore-base64.txt
   ```
2. GitHub repo -> Settings -> Secrets and variables -> Actions me ye 4 secrets add karein:
   - `KEYSTORE_BASE64` (upar wali file ka content)
   - `KEYSTORE_PASSWORD`
   - `KEY_ALIAS`
   - `KEY_PASSWORD`
3. `.github/workflows/build-apk.yml` me "Decode signing keystore" step ko uncomment karein.
4. `android/app/build.gradle.kts` line 55 me:
   ```
   signingConfig = signingConfigs.getByName("debug")
   ```
   ko badal kar:
   ```
   signingConfig = signingConfigs.getByName("release")
   ```
5. Commit + push karein — agla build asli keystore se signed hoga.
