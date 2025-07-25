# AdoptiPaw1
Hayvan barınak uygulaması


# 🐾 Adoptipaw | Hayvan Sahiplendirme Sistemi

Merhaba!  
Bu proje, 2025 Bahar Dönemi'nde **Veri Tabanı Yönetim Sistemleri** dersi kapsamında geliştirdiğim bir **hayvan sahiplendirme platformudur**. İçinde hem sağlam bir **SQL Server veritabanı**, hem de **C# Windows Form** arayüzü yer alıyor. Proje boyunca veritabanı nesneleriyle bolca haşır neşir olduk: **Trigger, Stored Procedure, View** gibi kavramlar doğrudan uygulandı.  

---

## 📌 Proje Özeti

**Adoptipaw**, barınaklardaki hayvanların dijital ortamda listelenip, sahiplendirilmesini kolaylaştırmak için geliştirildi.  
**Firma sahipleri** sisteme giriş yaparak sadece kendi barınaklarındaki hayvanları yönetebilir. Silinen hayvanlar fiziksel olarak silinmez, durumları pasif yapılır.  
Kullanıcılar sisteme giriş yaparak hayvanları inceleyebilir, sahiplenebilir.  

---

## 🔐 Giriş Ekranı (Login Form - C# Windows Forms)

Proje içerisinde bir **kullanıcı girişi** (login) sayfası yer almakta. Girişler SQL Server’dan gelen verilerle doğrulanıyor.

```csharp
SqlConnection conn = new SqlConnection("Data Source=.;Initial Catalog=Adoptipaw;Integrated Security=True");
conn.Open();
SqlCommand cmd = new SqlCommand("SELECT COUNT(*) FROM FirmaSahipleri WHERE KullaniciAdi=@adi AND Sifre=@sifre", conn);
cmd.Parameters.AddWithValue("@adi", txtKullaniciAdi.Text);
cmd.Parameters.AddWithValue("@sifre", txtSifre.Text);

int result = (int)cmd.ExecuteScalar();
if (result > 0)
{
    MessageBox.Show("Giriş başarılı.");
    // Ana sayfaya geç
}
else
{
    MessageBox.Show("Kullanıcı adı veya şifre hatalı.");
}
conn.Close();
```

Giriş sonrası sadece ilgili firma sahibine ait barınaktaki hayvanlara erişim verilir. Herkes her şeyi göremez 🌿

---

## 🧠 Veritabanı Detayları

### 📁 Tablolar:
- `Hayvanlar`
- `Sahiplendirmeler`
- `Kullanıcılar`
- `Barınaklar`
- `FirmaSahipleri`

### 👁️ View:
`vw_AktifHayvanlar`:  
Aktif durumda olan hayvanları listelemek için bir görünüm.

```sql
CREATE VIEW vw_AktifHayvanlar AS
SELECT * FROM Hayvanlar WHERE Durum = 'Aktif';
```

### ⚙️ Stored Procedure:
Hayvan sahiplendirme işlemini tek prosedürle yapıyoruz 👇

```sql
CREATE PROCEDURE sp_HayvanSahiplendir
    @HayvanID INT,
    @KullaniciID INT,
    @Tarih DATE
AS
BEGIN
    INSERT INTO Sahiplendirmeler (HayvanID, KullaniciID, Tarih)
    VALUES (@HayvanID, @KullaniciID, @Tarih)
END
```

### 🚨 Trigger:
Hayvan silme işlemi fiziksel değil, mantıksaldır. Yani kayıt durumu sadece “Pasif” yapılır.

```sql
CREATE TRIGGER trg_HayvanSilme
ON Hayvanlar
INSTEAD OF DELETE
AS
BEGIN
    UPDATE Hayvanlar
    SET Durum = 'Pasif'
    WHERE HayvanID IN (SELECT HayvanID FROM deleted)
END
```

---

## 💡 Özellikler

- [x] Firma sahibi bazlı yetkilendirme
- [x] Hayvanları listeleme, durum yönetimi
- [x] Sahiplendirme işlemi prosedür ile
- [x] Fiziksel silme yerine mantıksal silme (trigger)
- [x] Giriş ekranı C# Forms ile oluşturulmuş arayüz

---

## 🛠️ Kullanılan Teknolojiler

| Teknoloji | Açıklama |
|----------|----------|
| C#       | Windows Form App |
| SQL Server | Veritabanı yönetimi |
| T-SQL    | View, Trigger, Procedure kullanımı |
| Git      | Versiyon kontrol |
| .NET Framework | Form uygulaması geliştirme |

---

## 🪄 Geliştirme Süreci

Bu projede hem yazılım geliştirme becerilerimi hem de veritabanı tasarım kabiliyetimi geliştirme fırsatım oldu. Giriş ekranından trigger yazımına kadar her aşamasında elimle işlediğim, tam anlamıyla "kasnak gibi" bir proje oldu 🧵🐾

---

## 👩‍💻 Geliştiren
Bu proje, Zümra tarafından geliştirilmiştir.  
> 2025 Bahar Dönemi – VTYS Projesi
