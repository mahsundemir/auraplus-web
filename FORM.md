# Oura "New Application" formu — doldurma kılavuzu

## Sayfalar yayında

Üç zorunlu URL yayında ve doğrulandı:

| Sayfa | Adres | Durum |
|---|---|---|
| Website | `https://mahsundemir.github.io/auraplus-web/` | 200 ✓ |
| Privacy Policy | `https://mahsundemir.github.io/auraplus-web/privacy.html` | 200 ✓ |
| Terms of Service | `https://mahsundemir.github.io/auraplus-web/terms.html` | 200 ✓ |
| OAuth köprüsü | `https://mahsundemir.github.io/auraplus-web/oauth/callback.html` | 200 ✓ |

Kaynak: `github.com/mahsundemir/auraplus-web` (public).
Sayfaları değiştirmek için bu klasörde düzenleyip `git push` yeterli.

---

## Form alanları

| Alan | Değer |
|---|---|
| **Display Name** | `Aura+` |
| **Description** | `Native macOS and iOS companion for Oura Ring data. Presents sleep architecture, personal baselines, lagged correlations and long-term trends, with desktop and Home Screen widgets. Local-first: no backend, data stays on the user's device.` |
| **Contact Email** | `febilgiteknolojileri@gmail.com` |
| **Website** | `https://mahsundemir.github.io/auraplus-web/` |
| **Privacy Policy** | `https://mahsundemir.github.io/auraplus-web/privacy.html` |
| **Terms of Service** | `https://mahsundemir.github.io/auraplus-web/terms.html` |

### Redirect URIs — ikisini birden ekle

`+ Add URI` ile iki tane gir:

```
auraplus://oauth/callback
https://mahsundemir.github.io/auraplus-web/oauth/callback.html
```

**Neden ikisi birden:** Redirect URI'lar birebir eşleşen bir beyaz liste. Oura'nın
dokümanı yalnızca `https://` örnekleri gösteriyor; custom scheme'i kabul edip
etmediği belgelenmemiş. İkisini de kaydedersen hangi yolu seçersek seçelim
formu tekrar düzenlemek zorunda kalmazsın — sonradan URI eklemek uygulamayı
yeniden yapılandırmayı gerektirebilir.

`auraplus://` doğrudan `ASWebAuthenticationSession` ile çalışır (hem macOS hem
iOS). HTTPS olan ise yedek: tarayıcıda açılıp custom scheme'e yönlendirir.

---

## Scope'lar

**Email dışındaki hepsini işaretle.** Form varsayılan olarak hepsini işaretli
getiriyor; sadece Email'in tikini kaldır.

| Scope | Durum | Neden |
|---|---|---|
| Email | ☐ **kaldır** | Uygulama e-posta adresini hiç kullanmıyor |
| Personal | ☑ | Yaş/boy/kilo → BMI, damar yaşı karşılaştırması |
| Daily | ☑ | Uyku/hazırlık/aktivite skorları — çekirdek |
| Heartrate | ☑ | Gece nabız eğrisi |
| Tag | ☑ | Korelasyon motorunun girdisi |
| Workout | ☑ | Antrenman yükü (ACWR) |
| Session | ☑ | Meditasyon/nefes seansları |
| SpO2 | ☑ | Kandaki oksijen + solunum bozukluğu indeksi |
| Ring Configuration | ☑ | Yüzük modeli, firmware, pil göstergesi |
| Stress | ☑ | Günlük stres/toparlanma |
| Heart Health | ☑ | Damar yaşı ve VO₂ max (muhtemelen bu scope'a bağlı) |

Email'i çıkarmak gizlilik politikasında yazdığımızla da tutarlı: orada
«`email` scope'u istenmiyor» diyoruz.

---

## Formu gönderdikten sonra

Elinde `client_id` ve `client_secret` olacak.

- **`client_secret`'ı hiçbir yere yapıştırma** — repoya, Slack'e, buraya değil.
  Uygulamaya OAuth eklendiğinde Keychain'e veya Worker'a konacak.
- `client_id` gizli değil, sorun değil.
- Bu ikisi elinde dursun; **şu an uygulamada hiçbir şey değişmiyor** —
  Aura+ PAT ile çalışmaya devam ediyor. OAuth Faz 9'da devreye girecek.

## Oura API Agreement

Onay kutusunu işaretlemeden önce sözleşmeyi bir okumakta fayda var —
özellikle ticari kullanım ve veri saklama maddelerini. Kişisel kullanımda
sorun çıkarması beklenmez ama ileride dağıtım düşünürsen bağlayıcı olan o metin.

---

**Not:** `privacy.html` ve `terms.html` makul birer başlangıç metni; hukuki
danışmanlık değil. App Store'a çıkmaya karar verirsen bir gözden geçirme
isteyebilirsin. İçerik uygulamanın gerçekte yaptığı şeye göre yazıldı —
yerel depolama, backend yok, AI katmanı opt-in.
