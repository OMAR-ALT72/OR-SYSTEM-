# OR System — تطبيق الطلبيات (Windows / Android / iOS)

تطبيق إدارة طلبيات المواد لمصانع الحديد والصلب (LISCO)، مبني بـ Capacitor + Electron
من كود HTML/JavaScript واحد. نفس ملف `www/index.html` يعمل على الثلاث منصات.

## بنية المشروع

```
OR-System-Capacitor/
├── www/index.html          ← التطبيق (الكود المعدّل)
├── electron/               ← غلاف ويندوز (Desktop)
├── .github/workflows/      ← بناء APK و EXE تلقائياً عبر GitHub Actions
├── capacitor.config.ts     ← إعدادات Capacitor
└── package.json
```

## المتطلبات

| المنصة | المتطلبات |
|---|---|
| الكل | Node.js 18 أو أحدث (https://nodejs.org) |
| أندرويد | Android Studio + SDK (minSdk 22 / Android 5.1+) |
| iOS | Mac + Xcode 15+ + CocoaPods + حساب Apple Developer ($99/سنة) |
| ويندوز | لا شيء إضافي |

## التثبيت الأول

```bash
npm install
npx cap add android     # مرة واحدة فقط
npx cap add ios         # مرة واحدة فقط (على Mac)
npx cap sync
```

## بناء نسخة أندرويد

**APK للموظفين (توزيع مباشر):**
```bash
npx cap sync android
npx cap open android
```
ثم من Android Studio: `Build > Build App Bundle(s)/APK(s) > Build APK(s)`
الملف: `android/app/build/outputs/apk/debug/app-debug.apk`

**AAB للنشر على Google Play:**
`Build > Generate Signed Bundle / APK` (تحتاج keystore — احتفظ به سراً ولا ترفعه على GitHub).

## بناء نسخة iOS (على Mac فقط)

```bash
npx cap sync ios
npx cap open ios
```
ثم من Xcode: اختر جهاز/محاكي iPhone → `Product > Run` للتجربة،
وللنشر: `Product > Archive` → Upload to App Store Connect.

## بناء نسخة ويندوز (EXE)

```bash
npm install
npm run dist:win
```
الملفات الناتجة في `dist/` — ملف تثبيت NSIS + نسخة Portable (تعمل بدون تثبيت، مثالية للموظفين).

## بعد أي تعديل على الكود

عدّل `www/index.html` ثم:
```bash
npx cap sync        # يكفي لتحديث أندرويد و iOS وويندوز معاً
```

## التخزين والطباعة على الهواتف

- **التخزين**: على الهاتف تُحفظ البيانات في Preferences (آمنة ولا تُمسح مع cache المتصفح).
- **التصدير/الطباعة**: تُحفظ الملفات (Word/JSON/صفحة طباعة) في مجلد Documents ثم تظهر
  قائمة المشاركة — اختر "طباعة" أو "حفظ في الملفات" أو إرسالها عبر واتساب/إيميل.
- على ويندوز والمتصفح يعمل كل شيء كما في النسخة الأصلية (تنزيل مباشر + نافذة طباعة).

## GitHub — البناء التلقائي

المشروع يتضمن Workflow جاهز:
- **Android APK**: Actions ← "Build Android APK" ← Run workflow → تنزيل APK من Artifacts.
- **Windows EXE**: Actions ← "Build Windows EXE" ← Run workflow → تنزيل من Artifacts.

للتوزيع على الموظفين: أنشئ **Release** جديد (مثلاً `v1.0.0`) وأرفق ملفات APK/EXE به —
هذا أفضل من إرسالها عبر واتساب لأنها تبقى منظمة وبإصدارات واضحة.

## ملاحظات مهمة

1. لا ترفع ملفات التوقيع (keystore) على GitHub أبداً.
2. لتغيير معرف التطبيق عدّل `appId` في `capacitor.config.ts` (يجب أن يكون فريداً للنشر على المتاجر).
3. عند رفع النسخة لـ Google Play / App Store ستحتاج أيقونة التطبيق ولقطات شاشة — يمكن توليدها لاحقاً.
4. نسخة iOS تتطلب مراجعة Apple (عادة أيام إلى أسبوع)؛ نسخة أندرويد تُنشر خلال ساعات.
