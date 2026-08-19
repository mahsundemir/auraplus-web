# Oura "New Application" formu — doldurma kılavuzu

## Önce: üç URL'i yayına al

Formun zorunlu tuttuğu Website / Privacy Policy / Terms of Service sayfaları
`Web/` klasöründe hazır. GitHub Pages'te yayınlamak dört komut:

```bash
cd "/Users/mmd/Desktop/apps/Aura+/Web"
git init -b main && git add . && git commit -m "Aura+ web pages"
gh repo create auraplus-web --public --source=. --push
gh api -X POST repos/:owner/auraplus-web/pages -f "source[branch]=main" -f "source[path]=/"
```

Bir iki dakika sonra sayfalar şurada yayında olur:

```
https://<github-kullanıcı-adın>.github.io/auraplus-web/
```

Aşağıda bu adres `BASE` olarak geçiyor. Sayfaların açıldığını doğrulamadan
formu göndermemek daha güvenli.

---

## Form alanları

| Alan | Değer |
|---|---|
| **Display Name** | `Aura+` |
| **Description** | `Native macOS and iOS companion for Oura Ring data. Presents sleep architecture, personal baselines, lagged correlations and long-term trends, with desktop and Home Screen widgets. Local-first: no backend, data stays on the user's device.` |
| **Contact Email** | `febilgiteknolojileri@gmail.com` |
| **Website** | `BASE/` |
| **Privacy Policy** | `BASE/privacy.html` |
| **Terms of Service** | `BASE/terms.html` |

### Redirect URIs — ikisini birden ekle

`+ Add URI` ile iki tane gir:

```
auraplus://oauth/callback
BASE/oauth/callback.html
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
