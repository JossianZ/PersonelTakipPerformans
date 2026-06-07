# PERSONEL TAKİP SİSTEMİ - PROJE RAPORU

---

## 1. PROJE HAKKINDA

### 1.1 Genel Bilgiler
- **Proje Adı:** Kurumsal Personel Takip Sistemi (KPTS)
- **Versiyon:** 1.0
- **Geliştirme Dili:** PHP 8.x
- **Veritabanı:** MySQL/MariaDB
- **Arayüz:** TailwindCSS, HTML5

### 1.2 Proje Amacı
Şirketlerin personel bilgilerini, departman yapılarını ve izin süreçlerini dijital ortamda yönetmelerini sağlayan web tabanlı bir uygulamadır. Excel veya kağıt tabanlı takip yerine merkezi, güvenli ve hızlı bir sistem sunar.

---

## 2. SİSTEM ÖZELLİKLERİ

### 2.1 Temel Modüller
1. **Kullanıcı Girişi** - Güvenli kimlik doğrulama
2. **Dashboard** - İstatistikler ve genel bakış
3. **Personel Yönetimi** - Çalışan bilgileri CRUD işlemleri
4. **Departman Yönetimi** - Organizasyon yapısı
5. **İzin Takibi** - İzin talepleri ve onay süreçleri

### 2.2 Temel Fonksiyonlar
- ✅ Personel ekleme, düzenleme, silme, listeleme
- ✅ Departman yönetimi
- ✅ İzin talebi oluşturma ve onaylama
- ✅ Gerçek zamanlı istatistikler
- ✅ Responsive tasarım (mobil uyumlu)
- ✅ Güvenli veritabanı işlemleri

---

## 3. TEKNİK ALTYAPI

### 3.1 Kullanılan Teknolojiler
| Katman | Teknoloji |
|--------|-----------|
| Backend | PHP 8.x, PDO |
| Database | MySQL/MariaDB |
| Frontend | HTML5, TailwindCSS (CDN) |
| Icons | Font Awesome 6.0 |
| Güvenlik | bcrypt, Session, Prepared Statements |

### 3.2 Sistem Gereksinimleri
- PHP 8.0 veya üzeri
- MySQL 5.7+ veya MariaDB 10.4+
- Apache/Nginx web sunucu
- PDO extension

---

## 4. VERİTABANI YAPISI

### 4.1 Tablolar ve İlişkiler

**users** - Sistem kullanıcıları
- id, username, password (hashed), role, created_at

**departments** - Departmanlar
- id, name, description, created_at

**personnel** - Personel bilgileri
- id, first_name, last_name, email (unique), phone
- department_id (FK), position, hire_date, status, created_at

**leaves** - İzin talepleri
- id, personnel_id (FK), start_date, end_date
- type (enum), status (enum), reason, created_at

### 4.2 İlişkiler
- `personnel.department_id` → `departments.id` (ON DELETE SET NULL)
- `leaves.personnel_id` → `personnel.id` (ON DELETE CASCADE)

---

## 5. DOSYA YAPISI

```
PersonelTakip/
├── config/
│   └── database.php          # Veritabanı bağlantı ayarları
├── includes/
│   ├── auth.php              # Kimlik doğrulama
│   ├── header.php            # Sayfa üst kısmı ve menü
│   └── footer.php            # Sayfa alt kısmı
├── database.sql              # Veritabanı kurulum scripti
├── login.php                 # Giriş sayfası
├── logout.php                # Çıkış işlemi
├── index.php                 # Dashboard
├── personnel.php             # Personel yönetimi
├── departments.php           # Departman yönetimi
└── leaves.php                # İzin yönetimi
```

---

## 6. KURULUM

### 6.1 Adım Adım Kurulum
1. Projeyi web sunucunun root dizinine kopyalayın
2. `database.sql` dosyasını MySQL'e import edin
3. `config/database.php` dosyasında veritabanı bilgilerini düzenleyin
4. Tarayıcıda `login.php` sayfasını açın
5. Giriş yapın (Kullanıcı: `admin`, Şifre: `admin123`)

### 6.2 Veritabanı Ayarları
```php
$host = 'localhost';
$db_name = 'personel_takip';
$username = 'root';
$password = '';  // XAMPP için boş
```

---

## 7. KULLANIM

### 7.1 Giriş ve Dashboard
- Sistem girişi `admin/admin123` ile yapılır
- Dashboard'da personel, departman ve bekleyen izin sayıları görüntülenir
- Sol menüden tüm modüllere erişim sağlanır

### 7.2 Personel İşlemleri
- **Ekleme:** "Yeni Personel" → Form doldur → Kaydet
- **Düzenleme:** Listeden personeli seç → Düzenle → Güncelle
- **Silme:** Sil ikonuna tıkla → Onayla

### 7.3 İzin İşlemleri
- İzin talebi oluşturulur
- İzin türü seçilir (Yıllık, Mazeret, Hastalık, Ücretsiz)
- Onay durumu belirlenir (Bekliyor, Onaylandı, Reddedildi)

---

## 8. GÜVENLİK

### 8.1 Güvenlik Önlemleri
- ✅ **Şifreleme:** bcrypt ile password hashing
- ✅ **SQL Injection Koruması:** PDO Prepared Statements
- ✅ **XSS Koruması:** htmlspecialchars() ile output sanitization
- ✅ **Session Güvenliği:** PHP session ile oturum yönetimi
- ✅ **Erişim Kontrolü:** Her sayfada giriş kontrolü

### 8.2 Önerilen Ek Önlemler
- HTTPS kullanımı (SSL sertifikası)
- CSRF token sistemi
- Rate limiting (brute force koruması)
- Güçlü şifre politikası

---

## 9. MODÜL DETAYLARI

### 9.1 Dashboard (index.php)
- Toplam personel sayısı
- Toplam departman sayısı
- Bekleyen izin talepleri
- Son eklenen 5 personel listesi

### 9.2 Personel Yönetimi (personnel.php)
- Personel listesi (tablo görünümü)
- Yeni personel ekleme formu
- Personel düzenleme formu
- Silme işlemi
- Departman bazlı filtreleme

### 9.3 Departman Yönetimi (departments.php)
- Departman listesi
- Yeni departman ekleme
- Departman düzenleme
- Departman silme (personel bağlantıları korunur)

### 9.4 İzin Yönetimi (leaves.php)
- İzin talepleri listesi
- Yeni izin talebi oluşturma
- İzin türü seçimi (4 farklı tür)
- Onay durumu güncelleme
- Tarih aralığı belirleme

---

## 10. AVANTAJLAR VE FAYDALAR

### 10.1 İş Süreçlerine Katkı
- 📊 Merkezi veri yönetimi
- 🚀 Hızlı erişim ve sorgulama
- 📉 Kağıt işlerinin azalması
- ⏰ Zamandan tasarruf
- 🔍 Kolay raporlama altyapısı

### 10.2 Teknik Avantajlar
- 🎨 Modern ve kullanıcı dostu arayüz
- 📱 Mobil cihazlarda çalışabilme
- 🔒 Güvenli veri saklama
- ⚡ Hızlı performans
- 🔧 Kolay kurulum

---

## 11. GELİŞTİRME ÖNERİLERİ

### 11.1 Öncelikli İyileştirmeler
- [ ] Arama ve filtreleme özellikleri
- [ ] PDF/Excel rapor çıktıları
- [ ] E-posta bildirimleri
- [ ] Yetki bazlı kullanıcı rolleri
- [ ] İzin bakiyesi takibi

### 11.2 İleri Seviye Özellikler
- [ ] REST API geliştirme
- [ ] Mobil uygulama
- [ ] Performans değerlendirme modülü
- [ ] Mesai takip sistemi
- [ ] Dashboard grafikleri (Chart.js)

---

## 12. SONUÇ

Personel Takip Sistemi, küçük ve orta ölçekli işletmelerin insan kaynakları yönetimini kolaylaştıran, modern ve güvenli bir web uygulamasıdır. PHP ve MySQL teknolojileri kullanılarak geliştirilmiş olup, kolay kurulum ve kullanım sunmaktadır.

Sistem mevcut haliyle temel personel yönetimi ihtiyaçlarını karşılarken, modüler yapısı sayesinde gelecekte yeni özellikler eklenebilir ve farklı iş süreçlerine adapte edilebilir.


---

## 13. EKLER

### 13.1 Varsayılan Giriş Bilgileri
- **Kullanıcı Adı:** admin
- **Şifre:** admin123

### 13.2 Örnek Veriler
Sistem kurulumunda otomatik olarak eklenir:
- 3 Departman (İnsan Kaynakları, Bilgi İşlem, Muhasebe & Finans)
- 1 Örnek Personel (Ahmet Yılmaz)
- 1 Admin Kullanıcı

### 13.3 İletişim ve Destek
- **Proje Deposu:** https://github.com/horyusmoon-cloud/PersonelTakipProje.git
- **Lisans:** Açık Kaynak

---

**Rapor Tarihi:** 8 Haziran 2026  
**Versiyon:** 1.0  
**Hazırlayan:** Proje Ekibi
