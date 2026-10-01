# Kalıcı Kontrol Listesi ve Danışman Maddelerinin Durumu

Bu dosya iki işe yarar:
1. Her yeni sürümde aynı yazım ve notasyon hatalarının tekrarlanmasını önlemek (bölüm A).
2. Danışman toplantısında çıkan maddelerin makalede karşılanıp karşılanmadığını izlemek (bölüm B).

Durum işaretleri: ✅ makalede karşılandı · 🟡 kısmen karşılandı · ❌ karşılanmadı (veri, kod ya da karar gerekiyor)

---

## A. Her sürümde uygulanacak yazım kontrolleri

- [ ] **Section yapısı değişmedi.** Kontrol komutu:
  `diff <(git show 61f3c5a:main.tex | grep -E '\\(section|subsection|subsubsection|paragraph)\{|\\appendices' | sed 's/\\label.*//; s/ }/}/') <(grep -E '\\(section|subsection|subsubsection|paragraph)\{|\\appendices' main.tex | sed 's/\\label.*//')`
- [ ] `Definition` yalnızca gerçekten biçimsel tanım gereken yerde kullanılıyor (şu an yalnızca backdoor saldırısı).
- [ ] Standart kavramlar (ortalama, varyans, z-skoru, Holm) için formül tekrarı yok; yalnızca çalışmaya özgü kullanım anlatılıyor.
- [ ] Yöntem anlatımında kod ayrıntısı yok: `clip`, 10⁻⁶ eklemeleri, ikilileştirme, örnekleme ve tohum (seed) değerleri Ek A'da duruyor.
- [ ] Yöntem genel `K` ile yazılmış; `100` yalnızca deney ayarlarında geçiyor ve teorik gerekçesi varmış gibi sunulmuyor.
- [ ] Üç bantlı (low/mid/high) analiz yok; düşük ve yüksek frekans yalnızca kavramsal düzeyde geçiyor.
- [ ] Özdeğerler ve frekanslar okuyucunun takip edebileceği biçimde açıklanmış; açıklanmayan jargon yok.
- [ ] Notasyon tutarlı: matris ve vektörler kalın (`\bA`, `\bD`, `\bX`, `\bL`, `\bs`, `\blam`, `\bu`), skalerler italik, kümeler kaligrafik; her nicelik için tek sembol; her sembol ilk kullanıldığı yerde tanımlı.
- [ ] Her tablonun hangi soruyu desteklediği hem başlıkta hem metinde açık.
- [ ] Her paragraf bir öncekine bağlanıyor; her alt bölüm bir sonrakine geçiş cümlesiyle bitiyor.
- [ ] Metindeki tüm sayılar tablolarla tutarlı.
- [ ] Derleme temiz: hata, `undefined` referans ya da `Overfull` uyarısı yok.
- [ ] Yeni bilimsel iddia, uydurma gerekçe ya da doldurulmuş sonuç yok.

---

## B. Danışman maddeleri: makalede görünüyor mu?

### 1. Deneyler ve değerlendirme

| Madde | Durum | Makaledeki karşılığı / eksik olan |
|---|---|---|
| Savunmanın hangi aşamada uygulandığı | 🟡 | Metinde açıkça yazıldı (Bölüm IV girişi): savunma **çıkarım zamanında** çalışan bir filtre. Eğitim verisi temizlenmiyor, model yeniden eğitilmiyor. **Kodla doğrulanmadı**, çünkü kod depoda yok. Metindeki provenance notları (CA = `clean_acc_before`; savunma sonrası ASR kabul edilen graflar üzerinde ölçülüyor) bu yorumla tutarlı. |
| Savunma sonrası ASR'nin nasıl hesaplandığı | ✅ | V-A metrik paragrafı: Bütün uygun (eligible) tetiklenmiş test grafları detektörden geçiyor. ASR, **tüm** uygun graflar üzerinden hesaplanıyor; reddedilen graf başarısız saldırı sayılıyor. Model aynı zehirli model. |
| İki ayrı tespit sonucu: bütün tetiklenmiş örnekler ve yalnızca saldırının başarılı olduğu örnekler | ❌ | Tablo II yalnızca bütün tetiklenmiş örnekler üzerinden TPR veriyor. Başarılı saldırılar üzerinden TPR için graf düzeyinde sonuç dosyası gerekiyor; bu dosya depoda yok. |
| Temiz veriye etkisi: yanlışlıkla elenen temiz örnekler ve savunma sonrası temiz doğruluk | 🟡 | Yanlışlıkla elenen temiz örnekler FPR sütunu olarak var. **Savunma sonrası temiz doğruluk yok.** Ayrıca tanımına karar verilmeli: reddedilen temiz graflar hata mı sayılacak, yoksa doğruluk yalnızca kabul edilenler üzerinden mi hesaplanacak? |
| Saldırıya özel MLP yerine farklı saldırılarla eğitim (veri kümesine özel eğitim kabul edilebilir) | ❌ | Makale hâlâ veri kümesi × saldırı başına ayrı detektör kullanıyor; bu durum yalnızca sınırlılık olarak belirtiliyor (IV-B, VI). Çok saldırılı eğitim deneyi gerekiyor. |
| Yalnızca başarılı saldırı örnekleriyle dedektör eğitimi (başarısız örnekleri "temiz" saymadan) | ❌ | Yeni deney gerekiyor. |
| Node classification deneyi | ❌ | Makalede ve git geçmişinde node classification içeriği yok; yalnızca III-D'de UGBA/DPGBA'ya kısa bir atıf var. Deneyin tarifi bu depodaki notlarda bulunmuyor. |

### 2. Anlatım ve yapı

| Madde | Durum | Not |
|---|---|---|
| Önce bölüm ve içerik planı | ✅ | `REVISION_PLAN.md` |
| Gereksiz Definition blokları | ✅ | 8 Definition yerine 1 tane kaldı. Spectral descriptor düzyazı ve tek denklemle anlatılıyor. |
| `clip`, `[0,1]` gibi kod ayrıntıları | ✅ | Metinde `clip` yok. `[0,1]` yalnızca göreli konumların tanım aralığı olarak geçiyor; bu bir kırpma değil, yöntemin parçası. 10⁻⁶ korumaları Ek A'da. |
| Bilinen istatistiklerin tekrar tanımlanması | ✅ | Holm formülleri ve CA/ASR Definition'ları kaldırıldı; ortalama ve standart sapma yalnızca adlarıyla geçiyor. |
| `100`'ün yöntemden ayrılması | ✅ | Yöntemde genel `K` kullanılıyor; `K=100` yalnızca Simulation Setup'ta ve Tablo I'de geçiyor. Metinde "sabit bir tasarım tercihi; etkisi incelenmedi" deniyor. |
| Üç bantlı analizin çıkarılması | ✅ | Metinde yok. |
| Spektral kavramların açıklanması | ✅ | III-E: özdeğer ikinci dereceden form üzerinden açıklanıyor; düşük frekans bağlantılılık ve darboğaz, yüksek frekans iki parçalılık ile ilişkilendiriliyor. |
| Notasyon tutarlılığı | ✅ | Graf uzayı `\calG`, tetiklenmiş graf `\tilde G`, kullanılmayan `π` ve `\mathcal{L}` kaldırıldı, `m ≤ K` açıklandı. |
| Sonuç tablolarının düzenlenmesi | ✅ | Tablo II üç soruya göre gruplandı. Not: madde 1'deki yeni sonuçlar gelince tablo genişletilmeli. |
| Okuyucu gözüyle kontrol | ✅ | Geçişler yeniden yazıldı; dış okuyucunun takılabileceği noktalar bölüm C'de. |
| Taslağın dış okuyucuya (Çağrı) okutulması | ❌ | İnsan eylemi gerekiyor. Bölüm C, okuyucuya verilecek soru listesi olarak kullanılabilir. |
| Kalıcı kontrol listesi | ✅ | Bu dosya (bölüm A). |

### 3. Literatür ve kapsam

| Madde | Durum | Not |
|---|---|---|
| Graph classification saldırılarının yeniden taranması | ❌ | Yapılmadı. `main.bib` içinde metinde atıf yapılmayan saldırı kayıtları var: `eumc2025`, `abarc2025`, `baft2025`. Bunların graph classification kapsamında olup olmadığı kontrol edilmeli. |
| Karşılaştırılabilecek savunmalar ve deney gereksinimleri | 🟡 | Related Work savunmaları erişim gereksinimine göre grupluyor, ancak karşılaştırma deneyi ya da gereksinim tablosu yok. Atıf yapılmayan savunma kayıtları: `mad2025`, `tcf2025`, `losplit2025`. |
| Güncel devam çalışmaları | ❌ | Literatür taraması yapılmadı. |
| Daha kapsamlı dergi seçeneği / node içeriğini silmeme | 🟡 | SPL'e özgü kısıtlar kaldırıldı; metin artık belirli bir dergiye bağlı değil. Silinen node içeriği yok, çünkü zaten yoktu. |
| Clean-label genişlemesi | ❌ | Makalede yok. Henüz bir yöntem kararı olmadığı için yalnızca açık soru olarak eklenebilir. |

---

## C. Dış okuyucunun takılabileceği noktalar

1. **β neden istatistiksel detektörün FPR'sine bağlanıyor?** Metin bunun "aynı temiz maliyette karşılaştırma" için yapıldığını söylüyor. Yine de okuyucu MLP'nin kendi başına hangi FPR'de çalışacağını sorabilir.
2. **MLP aynı saldırının örnekleriyle eğitiliyor.** Bu sınırlılık açıkça yazıldı, ancak katkı (ii) okunurken gerçekçilik sorusu ilk akla gelen itiraz olacaktır. Madde 1'deki çok saldırılı deney bu soruyu yanıtlar.
3. **GTA neden en kolay tespit edilen saldırı?** Öznitelik değişimi spektruma yansımıyor ve GTA yalnızca kenar ekliyor. Metin bunun nedenini açıklamıyor, çünkü elimizde bunu destekleyen bir analiz yok.
4. **K=100 ve koordinatların bağımlılığı.** Grafların %93'ünden fazlasında n ≤ K olduğu için koordinatlar ara değerleme ile üretiliyor. Okuyucu "neden daha küçük bir K değil?" diye sorabilir; metin K'nın etkisinin incelenmediğini söylüyor.
5. **Gauss yaklaşımı.** α = 0.05 olmasına rağmen gözlenen temiz ret oranı %3.1–9.2. Metin bunu yaklaşıma bağlıyor; okuyucu bir kalibrasyon önerisi bekleyebilir.
6. **Sayı kontrolü.** PROTEINS için "2.5 to 3.9 times" ifadesi tablodaki yuvarlanmış değerlerden 11.8/3.1 = 3.8 veriyor. Ham sonuçlardan doğrulanmalı.
7. **Uygunluk (eligibility).** k'dan az düğümü olan ve tetikleyici uygulanmadan bırakılan grafların uygun kümeden çıkarılıp çıkarılmadığı kodla doğrulanmalı.
8. **Spektral olmayan bir referans yok.** Kenar sayısı gibi basit bir istatistikle karşılaştırma yapılmadı; okuyucu detektörün yalnızca kenar sayısındaki değişimi yakalayıp yakalamadığını sorabilir. Bu durum Conclusion'da sınırlılık olarak yazıyor.
9. **Savunma sonrası temiz doğruluk** raporlanmıyor (madde 1).
