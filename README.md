# UiPath Academy – Generate Yearly Report | Performer

## 📌 Proje Hakkında

Bu proje, **UiPath Academy Automation Developer Professional** sertifikasyon süreci kapsamında geliştirilen *Generate Yearly Report* otomasyonunun **Performer** aşamasıdır.

Projenin amacı, Dispatcher tarafından UiPath Orchestrator kuyruğuna aktarılan Work Item kayıtlarını işleyerek ACME System 1 uygulamasındaki aylık raporları indirmek, yıllık Excel raporuna dönüştürmek, sisteme yüklemek ve ilgili Work Item kayıtlarını güncellemektir.

Proje, **UiPath Studio kullanılarak tarafımdan geliştirilmiş** ve Robotic Enterprise Framework (REFramework) mimarisi üzerine kurulmuştur.

Geliştirme sürecinde UiPath'in güncellenen REFramework yapısına uyum sağlanmış ve önceki sürümlerde kullanılan `Config.xlsx` yerine **`Config.json`** ile çalışılmıştır.

## 🎯 Projenin Amacı

- UiPath Orchestrator üzerindeki `Acme4Queue` kuyruğundan işlem kayıtlarını almak.
- ACME System 1 uygulamasında ilgili Work Item detaylarına erişmek.
- Vendor Tax ID bilgilerini elde etmek.
- 2025 yılına ait mevcut aylık CSV raporlarını indirmek.
- İndirilen raporları tek bir yıllık Excel dosyasında birleştirmek.
- Oluşturulan yıllık raporu ACME System 1 uygulamasına yüklemek.
- Yükleme sonucunda oluşan Confirmation ID bilgisini almak.
- İlgili Work Item durumunu `Completed` olarak güncellemek.
- İşlem sonuçlarını Orchestrator üzerinden takip etmek.

## ⚙️ Kullanılan Teknolojiler

- UiPath Studio
- Robotic Enterprise Framework (REFramework)
- UiPath Orchestrator
- Config.json
- Web UI Automation
- Excel Automation
- CSV & DataTable Operations

## 🔄 Otomasyonun Çalışma Mantığı

### 1. Initialization

REFramework başlatılır ve `Config.json` dosyasındaki yapılandırma bilgileri okunur.

Orchestrator üzerinde tanımlanan `System1_Credential` Credential Asset kullanılarak ACME System 1 uygulamasına giriş yapılır.

### 2. Get Transaction Data

Dispatcher tarafından doldurulan `Acme4Queue` kuyruğundan sıradaki Queue Item alınır.

Alınan kayıt üzerinden ilgili Work Item ID bilgisine erişilir.

### 3. Process Transaction

Her Queue Item için aşağıdaki işlemler gerçekleştirilir:

**Work Item bilgilerinin alınması**

- İlgili Work Item detay sayfasına gidilir.
- Vendor Tax ID bilgisi elde edilir.

**Aylık raporların indirilmesi**

- Download Monthly Report sayfasına gidilir.
- Vendor Tax ID ve 2025 yılı seçilir.
- Ocak–Aralık ayları için mevcut raporlar kontrol edilir.
- Bulunan CSV raporları indirilir.
- Raporu bulunmayan aylar atlanarak işleme devam edilir.

**Yıllık Excel raporunun oluşturulması**

- İndirilen CSV dosyaları okunur.
- Veriler DataTable üzerinde birleştirilir.
- Birleştirilen veriler tek bir Excel dosyasına yazılır.

Oluşturulan dosyanın isimlendirme formatı:

`Yearly-Report-2025-{TaxID}.xlsx`

**Yıllık raporun yüklenmesi**

- Upload Yearly Report sayfasına gidilir.
- Vendor Tax ID ve yıl bilgileri girilir.
- Oluşturulan Excel dosyası seçilerek sisteme yüklenir.
- Başarılı yükleme sonrasında oluşan Confirmation ID bilgisi alınır.

**Work Item güncellemesi**

- İlgili Work Item güncelleme ekranına gidilir.
- Durum `Completed` olarak değiştirilir.
- Yorum alanına yükleme bilgisi eklenir:

`Uploaded with ID {ConfirmationID}`

### 4. Set Transaction Status

İşlem sonucuna göre Queue Item durumu REFramework üzerinden güncellenir.

Başarılı işlemler tamamlanırken, karşılaşılan hatalar framework'ün exception handling mekanizması kapsamında yönetilir.

## 🏗️ Proje Mimarisi

Bu proje, **Dispatcher–Performer (Producer–Consumer)** mimarisinin Performer bileşenidir.

**Dispatcher → Acme4Queue → Performer → Yearly Report → ACME System 1**

Dispatcher, işlenecek kayıtları kuyruğa eklerken Performer bu kayıtları bağımsız transaction'lar halinde işler.

Bu yapı sayesinde işlerin merkezi olarak yönetilmesi, işlem durumlarının takip edilmesi ve hata yönetiminin standartlaştırılması amaçlanmıştır.

## 📂 Temel Workflow'lar

| Workflow | Açıklama |
|---|---|
| `Main.xaml` | REFramework ana iş akışı |
| `Process.xaml` | Queue Item bazlı işlem adımları |
| `System1_Login.xaml` | ACME System 1 giriş işlemleri |
| `System1_Close.xaml` | Uygulamadan çıkış |
| `System1_NavigateTo.xaml` | Sayfalar arası navigasyon |
| `System1_GetClientDetails.xaml` | Vendor Tax ID bilgisinin alınması |
| `System1_DownloadMonthlyReports.xaml` | Aylık CSV raporlarının indirilmesi |
| `System1_MergeMonthlyReports.xaml` | CSV dosyalarının yıllık Excel raporunda birleştirilmesi |
| `System1_UploadYearlyReport.xaml` | Yıllık raporun sisteme yüklenmesi |
| `System1_UpdateWorkItems.xaml` | Work Item durumunun güncellenmesi |

## 🆕 Güncel REFramework ve Config.json Kullanımı

Projenin geliştirme sürecinde UiPath Studio ve REFramework yapısında yapılan güncellemeler nedeniyle yapılandırma yönetiminde `Config.json` kullanılmıştır.

Bu durum, eğitim materyallerinde yer alan `Config.xlsx` tabanlı örneklerle güncel proje yapısı arasındaki farklılıkların değerlendirilmesini gerektirmiştir.

Geliştirme sırasında:

- Güncel REFramework yapısı incelenmiştir.
- `Config.json` üzerinden yapılandırma değerlerinin yönetimi uygulanmıştır.
- Framework içerisindeki yapılandırma kullanımına uyum sağlanmıştır.
- Mevcut workflow'lar güncel proje yapısıyla entegre edilmiştir.

Bu süreç, farklı REFramework sürümlerine uyum sağlama ve mevcut eğitim dokümanlarını güncel geliştirme ortamına uyarlama konusunda deneyim kazandırmıştır.

## 🔗 Orchestrator Yapılandırması

| Bileşen | Değer |
|---|---|
| Folder | `UiPathAcademy` |
| Queue | `Acme4Queue` |
| Credential Asset | `System1_Credential` |
| Framework | REFramework |
| Transaction Type | `QueueItem` |
| Configuration | `Config.json` |

## 📤 Proje Çıktısı

Otomasyon sonucunda:

1. Vendor'a ait mevcut aylık CSV raporları indirilir.
2. Veriler yıllık Excel raporunda birleştirilir.
3. Yıllık rapor ACME System 1 uygulamasına yüklenir.
4. Confirmation ID bilgisi alınır.
5. İlgili Work Item `Completed` durumuna güncellenir.
6. Transaction sonucu Orchestrator üzerinden takip edilebilir.

## 💡 Kazanımlar

Bu proje kapsamında aşağıdaki konularda uygulamalı deneyim kazanılmıştır:

- REFramework ile uçtan uca otomasyon geliştirme.
- Queue tabanlı transaction yönetimi.
- UiPath Orchestrator entegrasyonu.
- Güncel REFramework yapısında `Config.json` kullanımı.
- Web uygulamalarında dinamik veri işleme.
- CSV ve Excel dosyalarıyla çalışma.
- DataTable işlemleri ve veri birleştirme.
- Modüler ve yeniden kullanılabilir workflow tasarımı.
- Dispatcher–Performer mimarisinin uygulanması.

## 🎓 Sertifikasyon

Bu proje, **UiPath Academy – Automation Developer Professional** sertifikasyon süreci kapsamında tamamlanmıştır.

Dispatcher projesinden farklı olarak Performer otomasyonunun geliştirme süreci tarafımdan yürütülmüş; analiz, workflow geliştirme, entegrasyon ve hata giderme aşamalarında uygulamalı deneyim kazanılmıştır.

---

**Not:** Bu repository eğitim ve portföy amaçlı hazırlanmıştır. Otomasyonun çalıştırılabilmesi için UiPath Studio, uygun Orchestrator yapılandırması, gerekli Queue ve Asset tanımları ile ACME System 1 test ortamına erişim gereklidir.
