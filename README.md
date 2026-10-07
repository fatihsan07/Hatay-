# Hatayİş

Hatay için Flutter Android/iOS/web hizmet pazarı; TypeScript/Vite yönetim paneli ve Supabase (PostgreSQL, Auth, Storage, Realtime) altyapısı. Sürüm: **1.5.0+6**. Kaynak kod MIT lisanslıdır; üçüncü taraf bileşenler kendi lisanslarını korur. Mağaza yayını ve iOS cihaz doğrulaması henüz tamamlanmadı.

## Kurulum

Flutter (Dart 3.12.1 veya uyumlu sürüm), Node.js 22+, Android SDK/JDK 17+; iOS için macOS ve Xcode gerekir. Kesin bağımlılık sürümleri lockfile dosyalarındadır.

1. `.env.example` dosyasını `.env` olarak kopyalayın. Kendi Supabase proje URL ve **publishable/anon** anahtarınızı girin. Service role/secret anahtarını istemciye yazmayın.
2. Yeni yerel backend için Supabase CLI ve Docker ile `supabase start` çalıştırın. `supabase db reset` yalnızca boş/test yerel veritabanında migration ve seed yüklemek içindir; mevcut veriyi siler. Canlı veritabanında çalıştırmayın.
3. Auth izin listesine mobil için `hatayis://auth-callback`, web için kendi origin adresinizi ekleyin. SMTP ve e-posta doğrulamasını yapılandırın.
4. `flutter pub get` ve `flutter run --dart-define-from-file=.env` çalıştırın. Ayarsız çalıştırma yalnızca katalog gösterir; kullanıcı veya iş işlemleri taklit edilmez.

```sh
flutter analyze
flutter test
npm ci --prefix tools/database
npm test --prefix tools/database
npm ci --prefix admin
npm run build --prefix admin
```

Admin için `admin/.env.example` dosyasını `admin/.env` olarak kopyalayın. Admin yetkisini güvenilir sunucu yöneticisi atar; kayıt metadata'sı yetki vermez.

## Mobil paketler

Android kimliği `com.hatayis.hatay_is`; iOS kimliği `com.hatayis.hatayIs`, minimum iOS 15. Kendi uygulamanızı dağıtırken mağaza hesabınıza uygun benzersiz kimlikler seçin.

Android release, `android/key.properties` ile dışarıdan sağlanan imzalama anahtarını gerektirir; debug anahtarıyla release üretmez. Anahtarları güvenli yedekleyin ve kaynak koduna koymayın. `key.properties` dosyasında `storeFile`, `storePassword`, `keyAlias`, `keyPassword` alanları gerekir.

```sh
flutter build apk --release --dart-define-from-file=.env
flutter build appbundle --release --dart-define-from-file=.env
```

Canlı yayına hazırlık için PowerShell: `scripts/build-live.ps1 -Target all -WebOrigin https://SIZIN-ADRESINIZ`. Script gerçek Auth/katalog/arama erişimini doğrular ve yalnızca public istemci ayarlarını derlemeye alır. macOS kontrolü: `bash scripts/check-ios.sh .env`. İmzalı IPA için Xcode takım/provisioning ayarlarını tamamlayıp `flutter build ipa --release --dart-define-from-file=.env` çalıştırın.

## Özellikler ve sınırlar

Müşteri/usta girişleri, e-posta doğrulama/şifre sıfırlama, sistem/açık/koyu tema, ilçe/hizmet arama, iş talebi, teklifler, randevular, mesajlar, yorumlar, favoriler, moderasyon ve usta istatistikleri bulunur. İşlem yetkileri veritabanında RLS ve kontrollü RPC ile uygulanır. 2026 çekim yılı doğrulanmış ilçe fotoğrafı bulunmadığı için fotoğraf arka planları kapalıdır.

Paket/fiyat yönetimi hazırlanmıştır; ödeme sağlayıcısı bağlı değildir ve satış kapalıdır. Uygulama içi bildirimler bulunur; cihaz push teslimatı ayrı Firebase/APNs/worker kurulumu ve gerçek cihaz testi gerektirir. Sosyal giriş sunulmaz. Hesap silme ve yayınlanacak gizlilik politikası, işletmeci/saklama kararları tamamlanana kadar mağaza engelidir.

Testlerdeki sahte kullanıcılar yalnızca izole test ortamı içindir. PostgreSQL testleri PGlite kullanır; gerçek SMTP/Storage/Realtime ve eşzamanlı cihaz testlerinin yerine geçmez. Kaynak paketinde canlı ayarlar, kullanıcı verileri, imzalar, eski fotoğraflar ve build önbellekleri bulunmaz.

Yayın denetimi: [docs/PRODUCTION-AUDIT.md](docs/PRODUCTION-AUDIT.md). Üçüncü taraf lisansları: [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md). Kaynak lisansı: [LICENSE](LICENSE).
