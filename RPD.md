Aşağıda, minimal ve şık bir MVP box interval timer uygulaması için Heroui tabanlı tasarım spec'ini ve RPD entegrasyonuna yönelik önerileri bulabilirsiniz.

---

## 1. Proje Genel Tanımı

- **Proje Adı:** Box Interval Timer (MVP)
- **Hedef:** Zamanlayıcı işlemlerini basit, anlaşılır ve estetik bir arayüz ile sunmak.
- **Platform:** Web tabanlı (responsive, mobil uyumlu) – Heroui UI kütüphanesi kullanılarak geliştirme.
- **Yaklaşım:** Minimalizm ve işlevselliği ön planda tutarak, kullanıcı dostu ve modern bir tasarım elde etmek.

---

## 2. Kullanıcı Akışı ve Fonksiyonel Gereksinimler

### 2.1 Ana Fonksiyonlar

- **Zamanlayıcı Başlat/Durdur:** Kullanıcı, interval timer’ı başlatıp durdurabilmeli.
- **Süre Ayarı:** Kullanıcı, önceden tanımlı veya özelleştirilebilir interval sürelerini (örneğin, 30 sn, 60 sn, vs.) seçebilmeli.
- **Reset/Restart:** Timer sıfırlama ve yeniden başlatma işlevleri.
- **Bildirimler:** Her interval sonunda ses veya görsel bildirim (örn. hafif animasyon/renk geçişi) ile kullanıcıya uyarı.

### 2.2 Ek Fonksiyonlar (MVP sonrası opsiyonel)

- **Kayıt ve geçmiş:** Önceki seansların kaydını gösterme.
- **Kişiselleştirme:** Renk paletleri veya tema seçenekleri.

---

## 3. UI Tasarım İlkeleri ve Estetik Vurgular

- **Minimalizm:** Gereksiz detaylardan kaçınılacak, sadece temel işlevler öne çıkarılacak.
- **Temiz Tipografi:** Okunaklı, modern fontlar (örn. Roboto, Open Sans) kullanılacak.
- **Boşluk Kullanımı:** Yeterli boşluklar (whitespace) ile arayüz ferah tutulacak.
- **Renk Paleti:** Nötr temel renkler (beyaz, gri tonları) ile, aksan olarak canlı ancak sofistike renkler (örneğin, mavi veya turuncu) kullanılacak.
- **Animasyon ve Geçişler:** Hafif animasyonlar (örn. düğme tıklamalarında, interval geçişlerinde) ile kullanıcı deneyimi zenginleştirilecek.
- **Hiyerarşi:** Bilgi hiyerarşisi net olarak belirlenip, en önemli öğeler (zaman gösterimi, kontroller) vurgulanacak.

---

## 4. Heroui ile Tasarım Uygulama Stratejisi

### 4.1 Bileşenler

- **Ana Zaman Gösterimi:**
  - Büyük, merkezi bir dijital ekran (örn. dijital saat stili).
  - Rakamlar yüksek kontrast ve okunabilirlik için yeterince büyük.
- **Kontrol Düğmeleri:**
  - Başlat/Durdur, Reset düğmeleri.
  - Düğmeler, Heroui’nin "Button" komponenti kullanılarak, yuvarlatılmış köşe ve hafif gölge efektiyle tasarlanacak.
- **Süre Seçici:**
  - Kullanıcıların belirli süreleri seçebileceği bir dropdown veya slider.
  - Heroui’nin "Select" veya "Slider" bileşeni entegre edilecek.
- **Bildirim Alanı:**
  - Timer tamamlandığında, ekranın üst kısmında/orta üst kısmında kısa bir bildirim alanı.
  - Özel stil ve geçiş animasyonları ile dikkat çekici fakat rahatsız etmeyecek biçimde yerleştirilecek.

### 4.2 Yerleşim Düzeni

- **Header Bölümü:**
  - Minimal logo veya uygulama ismi.
  - Basit ve sabit bir üst menü (gerekirse; MVP’de sadeleştirilebilir).
- **Ana İçerik:**
  - Zaman göstergesi ekranın merkezinde yer alacak.
  - Kontrol düğmeleri göstergenin alt kısmında, geniş dokunmatik alanlar sunacak.
- **Footer:**
  - Opsiyonel; haklar, kısa açıklama veya küçük ikonlarla sosyal medya bağlantıları.

---

## 5. RPD (Rapid Prototyping Design) Entegrasyonu

- **Wireframe & Prototip Araçları:** RPD kapsamında, hızlı prototipler oluşturulacak. Figma, Sketch veya Adobe XD gibi araçlarla temel akışlar görselleştirilebilir.
- **Kullanıcı Testleri:** Prototip üzerinden kullanıcı testleri yapılacak; feedback toplanarak hızlı iterasyon sağlanacak.
- **Dokümantasyon:** Her adımın UI bileşenleri, animasyon detayları ve renk şemaları detaylıca dokümante edilecek.

---

## 6. UI Detaylandırma ve Etkileşimler

### 6.1 Ana Zaman Gösterimi

- **Font & Renk:** Zamanın gösterildiği rakamlar, modern ve geometrik bir font ile, yüksek kontrast beyaz/metalik renk tonu.
- **Arka Plan:** Düz, yumuşak geçişli gradient veya düz renk; dinamik arka plan geçişleri interval değişimlerine eşlik edebilir.

### 6.2 Düğme Etkileşimleri

- **Başlat/Durdur:** 
  - Basıldığında, hafif renk geçişi (örn. ton değişikliği) ve minimal büyüme efekti.
  - Buton durumları (aktif, pasif, hover) net olarak belirgin olacak.
- **Reset:** 
  - Kullanıcı reset'e bastığında, hafif titreşim animasyonu ile geri bildirim sağlanacak.
  
### 6.3 Süre Seçici ve Bildirimler

- **Slider/Dropdown:** Kullanım kolaylığı için, süre seçicinin değeri canlı olarak gösterilecek. 
- **Bildirim:** Interval tamamlandığında, ekranda beliren kısa bir overlay mesaj (örneğin “Süre Tamamlandı!”) animasyonlu şekilde açılıp kapanacak.

### 6.4 Animasyon ve Geçişler

- **Transition Süreleri:** 200-300 ms arası yumuşak geçişler.
- **Gölge ve Derinlik:** Minimal gölge kullanımı ile 3D etki sağlanabilir.
- **Feedback Animasyonları:** Her kullanıcı etkileşimi, görsel olarak onaylanacak şekilde (örneğin buton basımında hafif renk değişimi, zaman güncellemede geçişli animasyon) tasarlanacak.

---

## 7. Özet

Bu spec, box interval timer MVP’sinin temel işlevlerini ve estetik tasarım prensiplerini özetlemektedir. Heroui’nin modern bileşenleri ve RPD yaklaşımı sayesinde kullanıcı dostu, minimal ama çok şık bir arayüz sunulacaktır. Tasarımda sade ama güçlü görsel hiyerarşi, animasyonlar ve dikkat çekici renk detayları ön plana çıkarılacaktır.

Bu doküman üzerinden, geliştirici ve tasarım ekibi, hem kullanıcı deneyimi hem de teknik uygulama açısından net bir yol haritasına sahip olacaktır.

--- 

Her adımda kullanılacak UI detayları ve prototip örnekleri, ek görsel referanslar ve iterasyon sonrası kullanıcı geri bildirimleriyle güncellenecektir.