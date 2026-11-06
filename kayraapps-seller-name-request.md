# Satıcı adını "Serdar Özdemir" → "KayraAPPS" yapma

Tarih: 2026-10-30 · Hesap team: App Store Connect API ile doğrulandı

## Durum tespiti (API'den)

- `/v1/users`: tek kullanıcı → **Serdar Özdemir**, roller `ACCOUNT_HOLDER, ADMIN`
- Yani hesap **Individual (bireysel)** üyelik. Ekip üyesi yok, organizasyon rolü yok.
- Hesapta 4 uygulama: Shelf, Tidy, Loghaven, Crowns (hepsi incelemede, henüz yayında değil).
- `itunes.apple.com/lookup` → yayında uygulama yok (resultCount 0), yani satıcı adı henüz mağazada görünmüyor.

## Apple kuralı (resmî)

- Bireysel üyelikte satıcı adı = **yasal ad**. "Set your developer name" sayfası:
  "If you're enrolled as an individual, this option isn't available to you and the developer
  name is the same as your legal name."
- Enrollment SSS: "Can I enroll as an individual and have my company appear as the seller?
  **No.** To have your company name appear as the seller, your company must be recognized as a
  legal entity and you must be enrolled as an organization. If you are a sole proprietor/single
  person business, you must join as an individual and your legal name will appear as the seller."
- Organizasyon hesabında: satıcı adı = tüzel kişilik unvanı; ayrıca uygulama başına
  **developer name** olarak tescilli ticari ad / DBA / fiktif işletme adı seçilebilir — ancak bu
  yalnızca **hesaba ilk uygulama eklenirken** seçilir, sonradan UI'dan değiştirilemez.

**Sonuç:** Bireysel hesapta "KayraAPPS" markasını satıcı adı yapmanın bir yolu yok.
Tek meşru yol: organizasyon üyeliğine geçiş (veya yeni organizasyon hesabı).

## Yol A — Individual → Organization geçişi (önerilen)

Form: https://developer.apple.com/contact/request/migrate-individual-account
(Account Holder ile giriş yapılmalı; form Türkçe değil, İngilizce doldurulur.)

Gerekenler:
1. **D-U-N-S numarası** (ücretsiz, D&B üzerinden; Apple asla DUNS ücreti almaz).
   Türkiye'de D&B'ye başvuru: https://developer.apple.com/support/D-U-N-S/
   Süre: genelde 5–30 gün. DUNS kaydındaki unvan + adres Apple'a verilecek belgelerle birebir aynı olmalı.
2. **Tüzel kişilik**: limited/anonim şirket en garantili yol (vergi levhası, ticaret sicil gazetesi,
   imza sirküleri, faaliyet belgesi). Apple'ın kendi metni şahıs şirketini organizasyon olarak
   kabul etmiyor ("sole proprietor → individual"), bu yüzden şahıs şirketiyle başvuru risklidir.
3. Hesabın **Account Holder**'ı olmak (sen öylesin) + kimlik doğrulama.
4. Mevcut 99 $/yıl üyelik devam eder, yeniden ödeme yok. Geçiş sonrası yıllık ücret tüzel kişiye kesilir.

Geçiş sonrası:
- Satıcı adı = şirket ticaret unvanı (örn. "Kayra Yazılım Ltd. Şti."). Kısa ad "KayraAPPS"
  görünsün istiyorsan ya şirket unvanını öyle kur ya da developer name'i tescilli ticari ad
  olarak ilk uygulama kaydında gir.
- **Uyarı (IDFV):** tüzel kişi adı değişince tüm uygulamalarda vendor adı değişir, kullanıcıların
  `identifierForVendor` değeri son güncellemede sıfırlanır; MMP/analitik varsa eski kullanıcılar
  yeni kurulum sayılabilir. **Senin uygulamaların henüz yayında olmadığı için bu risk şu an sıfır —
  değişikliği şimdi yapmak için en iyi zaman.**

### Form için İngilizce metin

```
Request: migrate Apple Developer Program membership from Individual to Organization.

Account holder: Yasar Serdar Ozdemir (Apple ID: <apple-id-mail>)
Current membership: Individual
Requested organization name (seller name): KayraAPPS
Legal entity: <tam ticaret unvanı>, <şehir>, Türkiye
D-U-N-S Number: <9 haneli>
Tax ID / VKN: <vergi no>

Reason: I am the founder and sole owner of the company. The apps currently in review
(Shelf, Tidy: Subscription Tracker, Loghaven: Home Records, Crowns) are to be published
under the company's trade name rather than my personal name, as required by App Store
Review guideline 1.2 (seller/company name must reflect the brand).

Attached: tax certificate (vergi levhası), trade registry gazette, signature circular,
company activity certificate, D-U-N-S confirmation.
```

## Yol B — Hesap zaten organizasyon olsaydı

Form: https://developer.apple.com/contact/request/update-company-information
(Sadece Account Holder. Organizasyon adı değişir → tüm uygulamalarda vendor adı değişir,
IDFV sıfırlanır, geri alınamaz. Belge doğrulaması istenir.)

## Yapılabilecekler / yapılamayacaklar özeti

| İstek | Mümkün mü |
|---|---|
| Bireysel hesapta satıcı adı = KayraAPPS | ❌ (Apple reddeder) |
| Apple Account'ta adı değiştirmek | ❌ satıcı adını değiştirmez |
| Organizasyona geçiş + KayraAPPS unvanı | ✅ (DUNS + tüzel kişilik şart) |
| Uygulama adı/markası "KayraAPPS" olarak görünsün (satıcı satırı hariç) | ✅ şimdi yapılabilir |
| Yayındaki uygulamayı yeni organizasyon hesabına transfer | ⚠️ app transfer yalnızca en az bir sürümü yayında olan uygulamalar için; henüz mümkün değil |

## Şimdi yapılacaklar

1. KayraAPPS bir ticari unvan mı olacak, yoksa sadece marka mı? Ticari unvan olacaksa şirket kur.
2. DUNS başvurusu yap (ücretsiz, unvan birebir aynı olmalı).
3. DUNS geldiğinde migrate-individual-account formunu yukarıdaki metinle doldur.
4. Apple onayından sonra ilk yeni uygulama kaydında developer name'i "KayraAPPS" gir.
