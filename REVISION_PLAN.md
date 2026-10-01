# Revizyon Planı — *On the Imperceptibility of Graph Backdoor Trojans in Spectral Domain*

Bu plan, `main.tex` dosyasının revizyon öncesi hali (commit `61f3c5a`) baştan sona okunarak hazırlandı. Revizyon bu plana göre bölüm bölüm yapıldı. Kalıcı kontrol listesi ve danışman maddelerinin durumu için `CHECKLIST.md` dosyasına bakın. Plandaki kurallar danışmanın 12 maddelik geri bildiriminden geliyor; madde numaraları köşeli parantez içinde verildi, örneğin [K2].

## 0. Mevcut durumun teşhisi

| Gözlem | Etkisi |
|---|---|
| Makale belirli bir dergiye bağlı değil. Dosya başlığındaki SPL notları, `\markboth` satırı ve metindeki "this letter" ifadeleri bir şablondan kalmış. | SPL'e özgü sayfa sınırı ve ifadeler kaldırılacak; "letter" yerine "paper" kullanılacak. Kesintiler sayfa bütçesine göre değil, yalnızca danışman kurallarına (katkıya hizmet edip etmeme) göre yapılacak. |
| Abstract boş. | Mevcut sonuçlardan yazılacak; yeni iddia eklenmeyecek. |
| Argüman zinciri: tetikleyici → spektral değişim → temiz varyasyonu aşıyor mu? (istatistiksel test) → tanımlayıcının tamamı grafları ayırıyor mu? (MLP) → reddetmek arka kapıyı bastırıyor mu? → sınırlar. Bu zincir dosya başındaki yorumda yazıyor, ancak metin bu sırayı izlemiyor. | Holm prosedürü, kullanıldığı detektör bölümünden iki bölüm önce, bağlamsız biçimde veriliyor. CA ve ASR iki kez tanımlanıyor (Definition'larda ve Setup'ta). PROTEINS'in neden zor olduğuna dair tek ipucu, kaynakçadan sonraki ekte kalıyor. |
| Sekiz `Definition` ortamı var: Holm, Backdoor Attack, CA, ASR, Normalized Laplacian Spectrum, Normalized Spectral Profile, iki detektör. | Biçimsel tanım gerektiren tek kavram backdoor saldırısı (tehdit modeli). Diğerleri ya standart [K2, K4] ya da yöntem anlatımı [K2]. |
| Üç bantlı (low/mid/high) analiz metinde yok. Örnekleme boyutu zaten genel `K` ile yazılmış, `100` yalnızca Setup'ta geçiyor. | [K5, K6] yöntem metninde karşılanmış durumda. Kontrol listesinde yeniden doğrulanacak. |

## (i) Bölüm yapısı: ilk gönderilen yapı aynen korunuyor

> **Bağlayıcı kural (kullanıcı):** İlk gönderilen haldeki section yapısı bozulmayacak. Bölüm, alt bölüm, alt-alt bölüm ve `\paragraph` başlıkları, sıraları ve iki ek aynen kalıyor. Revizyon yalnızca bu başlıkların **içeriğinde** yapılıyor. Eski planda önerilen bölüm taşımaları geri alındı. (Kontrol: `git show 61f3c5a:main.tex` ile yeni `main.tex`'in başlık listeleri `diff` ile karşılaştırıldı ve aynı çıktı.)

```
Abstract                                   (boştu → mevcut sonuçlardan yazıldı)
I.   Introduction                          soru → neden spektrum → üç adım → katkılar → bulgu özeti → yol haritası
II.  Related Work                          yapısal saldırılar → öznitelik saldırıları (kapsam dışı) → savunmalar (erişim ekseni) → konumlandırma
III. Preliminaries and Problem Formulation
     A. Graph Classification               notasyon (G, A, D, X, 𝒢, Δ^{C-1}, h_θ)
     B. Holm Procedure                     Definition ve formüller yok; düzyazı ile ne yaptığı, neden gerektiği, "en küçük p ≤ α/m" eşdeğerliği
     C. Backdoor Trojan
        1) Basics of a Backdoor Trojan     tek Definition (backdoor saldırısı); CA/ASR'nin deneysel karşılıklarına köprü
        2) Backdoor Insertion Framework
           ¶ Trigger-Insertion Policy Design   kısıtlı problem (tek denklem) + ceza terimli gevşetme (düzyazı)
           ¶ Data Poisoning                    zehirli veri kümesi (tek denklem)
           ¶ Model Poisoning                   düzyazı (standart ERM denklemi kaldırıldı)
     D. Trigger Insertion in Graphs        kenar kümesi modeli; düğüm sayısı korunuyor
        1) Static Triggers                 SBA, Motif; E_cut = E[V_g], E_g = ι(E_Γ)
        2) Dynamic Triggers                GTA; spektrumun yalnızca kenarları gördüğü köprüsü
     E. Graphs in Spectral Domain          L; ikinci dereceden form ile özdeğerin anlamı; düşük/yüksek frekans; tanımlayıcı s(G) (genel K)
IV.  Fingerprint of Backdoor Trojan        parmak izi; çıkarım zamanı filtre (yeniden eğitim yok); iki detektörün rolü
     A. Statistical Detection of Backdoor Trojan   p-değeri; Holm'a III-B üzerinden atıf
     B. Model-Based Backdoor Detection            MLP, eşik, β, LR referansı, saldırıya özel eğitim sınırı
V.   Numerical Results
     A. Simulation Setup                   Tablo I (yalın yapılandırma), havuz/detektör kurulumu, K=100, metrikler
        1) Datasets
        2) Backdoor Trojan Attacks
     B. Simulation Results                 Tablo II (üç soruya göre gruplanmış) + savunmasız saldırı bağlamı
        1) Statistical Detection
        2) MLP Detection Performance
        3) Analyzing Overall Defense Capability
VI.  Conclusion
Appendix A. Training Settings              eğitim, saldırı varsayılanları ve uygulama ayrıntıları (10⁻⁶, yalıtılmış düğüm, ikilileştirme, örnekleme)
Appendix B. Benchmark Structure            Tablo III (veri kümeleri)
```

## (ii) Her bölümde bulunması gereken içerik

| Bölüm | Bulunması gerekenler | Bulunmaması gerekenler |
|---|---|---|
| Abstract | Problem, soru, iki detektör, temel bulgular, denetimli öğrenme sınırı | Tabloda olmayan sayı ya da iddia |
| I | Arka kapı problemi; algılanamazlık sorusu; tetikleyicinin özdeğerleri neden değiştirebileceği; üç adım; katkılar; bulgu özeti | Deney ayrıntısı |
| II | Saldırı taksonomisi; savunmaların erişim gereksinimleri; bu çalışmanın konumu | Kopuk listeleme |
| III-A, III-B | Notasyon; Holm'un amacı ve kullanımı | Holm Definition'ı ve iki formülü [K2, K4] |
| III-C | Tek Definition; üç aşama; algılanamazlık kısıtı | J(φ), gradyan güncellemesi, ERM denklemi; CA/ASR Definition'ları [K2–K4] |
| III-D | Kenar kümesi modeli; statik ve dinamik tetikleyiciler | Örnekleme/sıralama/ikilileştirme ayrıntıları (Ek A'ya) [K3] |
| III-E | Özdeğerin yapısal anlamı; düşük/yüksek frekans; tanımlayıcı (genel K) | Definition ortamları; yalıtılmış düğüm kuralı (Ek A'ya) [K2, K3, K7] |
| IV | Parmak izi; savunmanın çıkarım zamanında uygulandığı; iki detektör | z-skoru/ortalama formülleri; detektör Definition'ları [K2, K4] |
| V-A | Tablo I, K=100 (gerekçe uydurulmadan), metrikler, savunma sonrası ASR'nin paydası | 10⁻⁶ korumaları (Ek A'ya) [K3, K5] |
| V-B | Tablo II'nin her sütun grubu bir alt-alt bölüme bağlı | Sayıların gereksiz tekrarı |
| VI | Soruya cevap; sınırlar; açık sorular | Yeni sonuç |

## (iii) Kaldırılan ve taşınan kısımlar

**Kaldırılanlar:** Holm Definition'ı ve iki formülü; CA ve ASR Definition'ları (tanımları V-A'da bir kez veriliyor); J(φ), gradyan güncellemesi ve ERM denklemi; genel tetikleyici bileşimindeki düğüm sayısı ve düğüm kümesi denklemleri; `Normalized Laplacian Spectrum`, `Normalized Spectral Profile` ve iki detektör Definition'ı (içerikleri düzyazıda); kullanılmayan makrolar; SPL'e özgü notlar ve "letter" ifadeleri.

**Ek A'ya (Training Settings) taşınanlar:** SBA'da düzgün örnekleme, Motif'te logit sıralaması, GTA'da ikilileştirme ve maskeleme; yalıtılmış düğüm kuralı; 10⁻⁶ korumaları ve tohum (seed) değerleri.

**Eklenenler (mevcut bulgulardan, yeni iddia yok):** Abstract; giriş sonunda bulgu özeti ve yol haritası; özdeğerin ikinci dereceden form ile açıklanması; savunmanın çıkarım zamanında uygulandığının ve yeniden eğitim yapılmadığının açıkça yazılması; savunma sonrası ASR'nin paydasının açıkça yazılması; PROTEINS tartışmasından Tablo III'e atıf.

## (iv) Tablolar [K9]

| Tablo | Değişiklik | Desteklediği iddia |
|---|---|---|
| Tablo I (Setup) | Yerinde kaldı; 10⁻⁶ satırları Ek A'ya taşındı | Deney yapılandırması |
| Tablo II (Results) | Sütunlar üç soruya göre gruplandı: Poisoned model (CA, ASR) / Separability (AUC: Stat, LR, MLP) / FPR (Stat, MLP) / TPR (Stat, MLP) / ASR after rejection (Stat, MLP). İstatistiksel detektörün FPR'si saldırıdan bağımsız olduğu için veri kümesi başına bir kez yazıldı. Sayılar değişmedi. | V-B-1, V-B-2 ve V-B-3 alt-alt bölümlerinin her biri bir sütun grubunu okuyor |
| Tablo III (Ek B) | Yerinde kaldı; PROTEINS tartışmasından atıf eklendi | PROTEINS'in yapısal farkı |

## (v) Anlatım ve notasyon sorunları

**Akış ve anlatım [K1, K7, K10, K11]**
* Giriş bölümünde "detectability is thus the converse of imperceptibility" ifadesi soruyu bulandırıyor. Soru doğrudan sorulmalı.
* İlgili çalışmalar bölümündeki A2GBD cümlesi bağlantısız duruyor. Savunma paragrafı erişim gereksinimi ekseninde bağlanmalı.
* "Bounded independently of graph size, which is what allows graphs to be compared without aligning their nodes" ifadesinde mantık hatası var. Düğümleri hizalama ihtiyacını ortadan kaldıran şey permütasyon değişmezliği; sınırlılık ise ortak bir değer aralığı sağlıyor.
* Özdeğerin anlamı "scaled by the square root of the node degree" gibi açıklanmayan bir ifadeyle anlatılıyor. Bunun yerine ikinci dereceden form verilip yorumlanacak.
* Statik ve dinamik tetikleyici alt bölümleri, uygulama ayrıntıları yüzünden argümanı (tetikleyici yalnızca kenarları değiştirir → spektrum) gölgeliyor.
* Sonuçlar bölümü sayıları cümle cümle tekrarlıyor. Her paragraf bir soruyla açılıp bir sonuçla kapanmalı.
* ASR iki kez tanımlanıyor; ikinci tanım ilkini "koşullandırma olayına ikinci bir şart ekleyerek" düzeltiyor.
* "Fingerprint" kavramı başlıkta geçiyor ama metinde tanımlanmıyor. Bölüm IV'ün ilk cümlesinde tanımlandı (başlık korundu).

**Notasyon [K8]**
* Graf uzayı `\mathcal{X}` ile, öznitelik matrisi `\mathbf{X}` ile gösteriliyor ve karışabiliyor. Graf uzayı için `\mathcal{G}` kullanılacak.
* Eğitim kaybı için `\mathcal{L}` kullanılıyor. Chung'ın kitabında normalize Laplasyen de 𝓛 ile gösterildiğinden karışıklık riski var. Kayıp sembolü metinden çıkacak (denklemler kaldırılınca gerek kalmıyor).
* Tetiklenmiş graf bir yerde `T(G,y_t)`, bir yerde demet, bir yerde `\tilde{\bA}` ile yazılıyor. `\tilde G = T(G, y_t)`, `\tilde{\calE}`, `\tilde{\bA}` ve `\tilde{\bX}` biçiminde tutarlı hale getirilecek.
* Yerleştirme haritası `ι_π`'deki π hiçbir yerde kullanılmıyor; `ι` olarak sadeleştirilecek.
* `m` (test edilen koordinat sayısı) ile `K` arasındaki fark açıklanmıyor. "R üzerinde sabit olan koordinatlar test edilmez, m ≤ K" ifadesi eklenecek.
* `Δ^{C-1}` (olasılık simpleksi) açıklanmıyor.
* Vektör ve matrisler kalın (`\bA`, `\bD`, `\bX`, `\bL`, `\bs`, `\boldsymbol\lambda`), skalerler normal yazılacak (`λ_i`, `s_j`, `μ_j`, `σ_j`, `K`, `m`, `α`, `β`, `τ`, `ρ`, `k`). `\mathbf{x}_v` satır vektörü kalın kalacak.

## Kontrol listesi: revizyon sonunda tarandı [K12]
- [x] Yalnızca biçimsel tanım gerektiren kavramlarda `Definition` var.
- [x] Standart istatistik ya da kavram için gereksiz formül yok (ortalama, varyans, z-skoru, Holm).
- [x] Yöntem metninde uygulama ayrıntısı yok (10⁻⁶, ikilileştirme, örnekleme, kırpma).
- [x] Yöntem `K` ile yazılmış; `100` yalnızca deney ayarlarında geçiyor ve gerekçesi uydurulmamış.
- [x] Üç bantlı analiz yok; düşük/yüksek frekans yalnızca kavramsal düzeyde geçiyor.
- [x] Her sembol ilk kullanıldığı yerde tanımlanmış; aynı nicelik için tek sembol kullanılıyor.
- [x] Her tablonun hangi soruyu desteklediği metinde ve başlıkta açık.
- [x] Paragraflar arası geçişler kopuk değil (her bölüm sonu bir sonrakine bağlanıyor).
- [x] Tüm sayılar Tablo II ile tutarlı.
- [x] Derleme hatası, tanımsız referans ya da atıf yok.
- [x] SPL'e ya da "letter" biçimine özgü ifade kalmamış.
- [x] Section yapısı ilk gönderilen halle aynı (`diff` ile doğrulandı).
