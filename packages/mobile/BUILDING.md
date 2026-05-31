# P2P Drop — Mobile Build Guide

نفس تطبيق الويب تمامًا، مغلّف داخل تطبيق أندرويد وiOS أصلي باستخدام Capacitor.

---

## المتطلبات

| الأداة | الاستخدام | رابط التنزيل |
|--------|-----------|--------------|
| Node.js 18+ | بناء الـ web | nodejs.org |
| Android Studio | بناء APK/AAB لـ Play Store | developer.android.com/studio |
| Xcode 15+ (Mac فقط) | بناء IPA لـ App Store | Mac App Store |
| Java 17 | مطلوب لـ Android Studio | adoptium.net |

---

## خطوات البناء

### 1. استنسخ المشروع على جهازك المحلي

```bash
git clone <your-repo-url>
cd p2p-drop
npm install
```

### 2. عيّن رابط سيرفر الاقتران الإنتاجي

```bash
# أنشئ هذا الملف في packages/web/
echo "VITE_SIGNALING_URL=wss://YOUR_DEPLOYED_URL/signaling?room=public" > packages/web/.env.production
```

> استبدل `YOUR_DEPLOYED_URL` برابط تطبيقك المنشور على Replit.

### 3. ابنِ تطبيق الموبايل

```bash
npm run build:mobile
```

هذا الأمر يقوم بـ:
- بناء `packages/core`
- بناء `packages/web` وإنتاج ملفات `dist`
- نسخ الملفات داخل Android وiOS عبر `cap sync`

---

## أندرويد — Google Play Store

### فتح المشروع في Android Studio

```bash
npm run open:android
```

أو يدويًا:
1. افتح Android Studio
2. اختر **Open** → `packages/mobile/android`

### توقيع التطبيق (مطلوب للنشر)

1. في Android Studio: **Build → Generate Signed Bundle/APK**
2. اختر **Android App Bundle (.aab)** (مطلوب للـ Play Store)
3. أنشئ Keystore جديد أو استخدم واحدًا موجودًا
4. احفظ بيانات الـ Keystore في مكان آمن — لا يمكن استعادتها

### رفع إلى Google Play

1. افتح [Google Play Console](https://play.google.com/console)
2. أنشئ تطبيقًا جديدًا بالـ App ID: `com.p2pdrop.app`
3. أرفع ملف الـ `.aab`
4. أكمل بيانات المتجر (وصف، صور، تصنيف)
5. أرسل للمراجعة

---

## iOS — Apple App Store

> **ملاحظة:** بناء iOS يتطلب جهاز Mac مع Xcode.

### فتح المشروع في Xcode

```bash
npm run open:ios
```

أو يدويًا:
1. افتح `packages/mobile/ios/App/App.xcworkspace` في Xcode

### إعداد التوقيع

1. في Xcode: اختر **App target → Signing & Capabilities**
2. سجّل دخولك بحساب Apple Developer
3. Xcode سيضبط التوقيع تلقائيًا (Automatic Signing)
4. Bundle ID: `com.p2pdrop.app`

### بناء ورفع

1. **Product → Archive**
2. في Organizer: **Distribute App → App Store Connect**
3. في [App Store Connect](https://appstoreconnect.apple.com): أنشئ تطبيقًا جديدًا
4. أكمل البيانات وأرسل للمراجعة

---

## بعد تحديث التطبيق

في كل مرة تُعدّل الكود:

```bash
npm run build:mobile
# ثم أعد البناء من Android Studio أو Xcode
```

---

## ملاحظات مهمة

- **App ID:** `com.p2pdrop.app` — لا تغيّره بعد النشر
- **التوقيع:** احتفظ بملف الـ Keystore (Android) وبيانات المطور (iOS) في مكان آمن
- **الأذونات المُدرجة:**
  - الإنترنت والشبكة (اكتشاف الأجهزة)
  - الكاميرا (مسح QR ومكالمات الفيديو)
  - الميكروفون (المكالمات الصوتية)
  - التخزين (إرسال واستقبال الملفات)
  - الإشعارات
