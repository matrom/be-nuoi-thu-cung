# Bé Nuôi Thú Cưng - Android

Game chạy hoàn toàn offline: toàn bộ trang game nằm trong www/index.html và được đóng gói vào app.

## Cách 1: GitHub Actions (không cần cài gì)
1. Tạo repo GitHub mới, đẩy toàn bộ thư mục này lên.
2. Vào tab Actions > "Build Android APK" > Run workflow.
3. Tải file be-nuoi-thu-cung-apk (app-debug.apk) ở mục Artifacts, chép sang điện thoại và cài.

## Cách 2: Build trên máy (cần Android Studio hoặc JDK 17 + Android SDK)
    npm install
    npx cap sync android
    cd android && ./gradlew assembleDebug
APK nằm ở android/app/build/outputs/apk/debug/app-debug.apk. Hoặc chạy `npx cap open android` rồi Run trong Android Studio.

## Sửa game
Sửa www/index.html, rồi chạy `npx cap sync android` và build lại.

## Đưa lên Google Play (file AAB)
1. Tạo upload keystore (chỉ làm một lần, GIỮ KỸ file và mật khẩu):
       keytool -genkey -v -keystore upload.jks -keyalg RSA -keysize 2048 -validity 10000 -alias upload
2. Trong repo GitHub: Settings > Secrets and variables > Actions, thêm 3 secret:
   UPLOAD_KEYSTORE_BASE64 (chạy `base64 -w0 upload.jks`), UPLOAD_KEYSTORE_PASSWORD, UPLOAD_KEY_ALIAS (upload).
3. Actions > "Build signed AAB for Google Play" > Run workflow, tải app-release-signed.aab.
4. Đổi appId trong capacitor.config.json (và applicationId trong android/app/build.gradle) thành mã của riêng bạn TRƯỚC khi tải lên lần đầu, vì mã app không đổi được sau này.
5. Tăng versionCode trong android/app/build.gradle mỗi lần cập nhật.
