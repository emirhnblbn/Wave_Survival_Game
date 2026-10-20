# Wave_Survival_Game
Wave Survival Oyunu

Teslim Tarihi: 23.10.2026 
Ege Üniversitesi — Tire Kutsan Meslek Yüksekokulu — Bilgisayar Programcılığı Proje Dersi Raporu

GitHub Repo URL:https://github.com/emirhnblbn/Wave_Survival_Game

Öğrenci Bilgileri:
60250000048 - Emirhan Balaban
60250000049 - Kenan Onay

A. Sistem Gereksinimleri
1. Projenin Amacı ve Kapsamı (Scope)

Amaç: Oyuncuya kısa seanslarda (10-20 dakika) oynanabilen, her oturumda farklı bir ilerleme deneyimi sunan, tekrar oynanabilirliği yüksek bir 2D hayatta kalma (wave survival) oyunu sunmak. Oyun, Vampire Survivors tarzı "otomatik saldırı + yoğun düşman dalgaları + hızlı karakter geliştirme" formülünü temel alır.

Çözdüğü Problem:
Bu oyun türünü seven ve bu oyun türünün diğer oyunlarını çoktan bitirmiş olan ve bu türde yeni bir oyun deneyimlemek isteyen kullanıcıların isteğini karşılar.
Sistem ne yapacak:

Oyuncunun oyun başında birden fazla karakter arasından seçim yapmasına izin verecek.
Düşmanları zamanla artan sayı ve güçte dalgalar halinde oyuncunun üzerine gönderecek.
Oyuncu düşman yendikçe deneyim puanı ve ödüller (yetenek, ekipman vb.) kazandıracak.
Seviye atlandığında oyuncuya yetenek/ekipman seçimi sunacak.
Dalgalar ilerledikçe zorluğu kademeli olarak artıracak.

Kapsam dışı (bu sürümde yapılmayacaklar):

Çok oyunculu (multiplayer) mod.
Mobil platform desteği (yalnızca PC/masaüstü hedeflenir).
Online skor tablosu / sunucu tabanlı kayıt sistemi.
Hikaye modu, cutscene veya diyalog sistemi.
Kalıcı (meta-progression) ilerleme — her oyun baştan başlar; bu, zaman kalırsa ileride eklenebilecek bir genişletme olarak not edilmiştir.
2. Kullanıcı Profilleri / Aktörler
Aktör	Açıklama	Yetki Seviyesi
Oyuncu	Oyunu başlatan, karakter seçen, oyunu oynayan tek insan aktör	Tüm oyun içi etkileşimleri (hareket, menü, duraklatma) yapabilir
Sistem (Wave Manager)	İnsan olmayan aktör; düşman dalgalarını üretir ve zorluk seviyesini otomatik ayarlar	Oyuncu müdahalesi olmadan arka planda çalışır

Not: Oyun tek kullanıcılı ve çevrimdışı olduğundan admin/misafir gibi ek roller bulunmamaktadır; tek insan aktör "Oyuncu"dur.

3. Fonksiyonel Gereksinimler (Functional Requirements)
Bir Oyuncu olarak, oyuna başlamadan önce farklı karakterler arasından seçim yapmak istiyorum, böylece oynanış tarzımı kendi tercihime göre belirleyebileyim.
Bir Oyuncu olarak, karakterimi klavye ile dört yönde hareket ettirmek istiyorum, böylece düşmanlardan kaçabileyim veya onlara yaklaşabileyim.
Bir Oyuncu olarak, karakterimin yakındaki düşmanlara otomatik olarak saldırmasını istiyorum, böylece hareket ve konumlanmaya odaklanabileyim.
Bir Oyuncu olarak, düşmanları yendikçe deneyim puanı kazanmak istiyorum, böylece karakterimi geliştirebileyim.
Bir Oyuncu olarak, seviye atladığımda sunulan birkaç yetenek/ekipman arasından seçim yapmak istiyorum, böylece kendi stratejime uygun bir build oluşturabileyim.
Bir Oyuncu olarak, ekranda kalan canımı, deneyim çubuğumu ve geçen süreyi görmek istiyorum, böylece oyundaki durumumu anlık takip edebileyim.
Bir Oyuncu olarak, oyunu istediğim an duraklatıp devam ettirebilmek istiyorum, böylece ihtiyaç halinde araya girebileyim.
Bir Oyuncu olarak, karakterim öldüğünde bir "Oyun Bitti" ekranı görüp oyunu yeniden başlatabilmek istiyorum, böylece tekrar deneyebileyim.
Bir Oyuncu olarak, ilerleyen dalgalarda düşman sayısının ve gücünün artmasını istiyorum, böylece oyun giderek zorlaşsın ve meydan okuma hissi sürsün.

5. Fonksiyonel Olmayan Gereksinimler (Non-Functional Requirements)
Performans: Aynı anda ekranda onlarca/yüzlerce düşman olabileceğinden, obje havuzlama (object pooling) kullanılarak stabil bir kare hızı (ör. 60 FPS) hedeflenmelidir.
Kullanılabilirlik: Kontroller basit ve sezgisel olmalı (ör. WASD/yön tuşları ile hareket, saldırı otomatik).
Taşınabilirlik: Unity motoru sayesinde en azından Windows masaüstü için build alınabilmeli; mümkünse WebGL build'i de hedeflenebilir.
Ölçeklenebilirlik: Yeni karakter, düşman ve yetenek eklemek kolay olmalı; karakter/düşman verileri kod içine gömülmek yerine Unity ScriptableObject gibi veri yapılarıyla tutulmalıdır.
Güvenilirlik: Oyun, tek bir oturumda (20+ dakika) çökme yaşamadan çalışabilmelidir.

7. Harici Arayüz Gereksinimleri (External Interfaces)
Oyuncunun ulaştığı en yüksek seviye ve o seviyede oynadığı karakter, oyunun çalıştığı bilgisayarda yerel bir veritabanı dosyasında saklanır.

B. Dizayn Raporu
1. Kullanım Senaryoları (Use-Case Scenarios)


Senaryo 1 — Karakter Seçme

Aktör: Oyuncu
Ön Koşul: Oyuncu ana menüdedir, "Oyuna Başla" seçeneğine basmıştır.
Ana Akış: Oyuncu karakter seçim ekranına gelir → mevcut karakterler arasında gezinir → istediği karakteri seçer → "Başla" butonuna basar → oyun sahnesi yüklenir.

Senaryo 2 — Dalga ile Hayatta Kalma

Aktör: Oyuncu, Sistem
Ön Koşul: Oyun sahnesi yüklenmiş, oyuncu karakteri sahnededir.
Ana Akış: Sistem belirli aralıklarla düşman dalgası oluşturur → düşmanlar oyuncuya doğru hareket eder → oyuncu klavye ile konumunu değiştirir → karakter menzildeki düşmanlara otomatik saldırır → düşman ölünce deneyim nesnesi bırakır → oyuncu bu nesneyi toplar.

Senaryo 3 — Seviye Atlama ve Yetenek Seçme

Aktör: Oyuncu
Ön Koşul: Oyuncunun deneyim çubuğu dolmuştur.
Ana Akış: Oyun kısa süreliğine duraklar → ekranda 2-3 yetenek/ekipman kartı belirir → oyuncu kartlardan birini seçer → seçilen yetenek karaktere eklenir/güçlendirilir → oyun kaldığı yerden devam eder.

Senaryo 4 — Oyunun Bitmesi ve Yeniden Başlama

Aktör: Oyuncu
Ön Koşul: Oyuncunun canı sıfıra düşmüştür.
Ana Akış: Sistem "Oyun Bitti" ekranını gösterir, ulaşılan dalga/süre/skor bilgisini listeler → oyuncu "Tekrar Dene" veya "Ana Menü" seçeneklerinden birine basar → ilgili sahne yeniden yüklenir.


2. UML Sınıf Diyagramı




3. UI/UX Tasarımları
Ana Menü: Oyun logosu, "Oyuna Başla", "Ayarlar", "Çıkış" butonları.
Karakter Seçim Ekranı: Yatayda kaydırılabilir karakter kartları (portre, isim, kısa yetenek açıklaması), seçili karakteri onaylayan "Başla" butonu.
Oyun İçi HUD: Sol üstte can barı, üstte ortada deneyim (XP) çubuğu, sağ üstte geçen süre ve dalga numarası, ekranın köşesinde duraklat butonu.
Seviye Atlama / Yetenek Seçim Ekranı: Oyunun üzerine yarı saydam bir panel, yan yana 2-3 yetenek kartı (ikon + isim + kısa açıklama), oyuncu bir karta tıklar.
Duraklatma Menüsü: "Devam Et", "Ana Menüye Dön", "Çıkış" seçenekleri.
Oyun Bitti Ekranı: "Oyun Bitti" başlığı, ulaşılan dalga/süre/skor özeti, "Tekrar Dene" ve "Ana Menü" butonları.

4. Kullanılan Teknolojiler
Katman	Teknoloji	Neden
Oyun Motoru	Unity (2D, URP)	2D araçları olgun, C# ile kod yazmaya uygun, geniş öğretici kaynak
Programlama Dili	C#	Unity'nin yerel dili; grubun önceden bildiği bir dil olması öğrenme süresini kısaltır
Veri Yönetimi	Unity ScriptableObject	Karakter/düşman/yetenek verilerini koddan bağımsız, kolay genişletilebilir şekilde tutmak için
Sürüm Kontrolü	Git + GitHub	Grup içi iş bölümü, commit geçmişiyle bireysel katkının izlenebilmesi
UI	Unity UGUI (veya UI Toolkit)	Unity ile doğrudan entegre, HUD ve menüler için yeterli
Görsel Tasarım	2D sprite / pixel art	Tür için uygun, düşük performans maliyeti
