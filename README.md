# Akademik Makale Toplama Betiği: Elmas Tabanlı Kuantum Teknolojileri

Bu proje, Google Colab üzerinde çalışan bir Python betiğidir ve [arXiv](https://arxiv.org/) ile [Semantic Scholar](https://www.semanticscholar.org/) platformlarından "elmas tabanlı kuantum teknolojileri" üzerine odaklanan akademik makaleleri otomatik olarak toplamak için tasarlanmıştır. Betik, toplanan makalelerin başlık, özet, yazarlar, yayın tarihi, DOI ve PDF bağlantısı gibi temel metaverilerini çıkarır ve bu verileri Google Drive'a hem JSON hem de CSV formatlarında kaydeder.

## 🎯 Amaç

Bu projenin temel amacı, belirli araştırma alanlarındaki (bu durumda elmas tabanlı kuantum teknolojileri) güncel akademik literatürü sistematik bir şekilde toplamak, düzenlemek ve kolay erişilebilir bir formatta depolamaktır. Bu, literatür taraması ve veri analizi süreçlerini otomatikleştirmeye yardımcı olabilir.

## 🛠️ Kullanılan Teknolojiler ve Kütüphaneler

* **Programlama Dili:** Python
* **Çalışma Ortamı:** Google Colaboratory
* **Ana Kütüphaneler:**
    * `arxiv`: arXiv API'si ile etkileşim için.
    * `semanticscholar`: Semantic Scholar API'si ile etkileşim için.
    * `pandas`: Verileri CSV formatında işlemek ve kaydetmek için.
    * `json`: Verileri JSON formatında işlemek ve kaydetmek için.
    * `tqdm`: Veri toplama sürecinde ilerleme çubukları göstermek için.
    * `google.colab.drive`: Google Drive entegrasyonu için.
* **Veri Kaynakları:**
    * arXiv API
    * Semantic Scholar API

## ✨ Özellikler

* **Yapılandırılabilir Arama Terimleri:** Birden fazla arama terimi tanımlayarak geniş bir literatür taraması yapabilme.
* **Sonuç Sınırlandırma:** Her arama terimi için API'lerden çekilecek maksimum sonuç sayısını belirleyebilme.
* **Çoklu Çıktı Formatı:** Toplanan verileri hem JSON hem de CSV formatlarında kaydedebilme.
* **Google Drive Entegrasyonu:** Toplanan verilerin doğrudan kullanıcının Google Drive hesabına kaydedilmesi.
* **API Gecikme Yönetimi:** API hız limitlerine takılmamak için istekler arasına gecikme ekleyebilme.
* **Temel Veri Kalite Kontrolleri:** Toplanan veriler üzerinde benzersiz makale sayısı, özeti olmayan makaleler ve PDF bağlantısı olan makaleler gibi basit kontroller.
* **Hata Yönetimi:** API istekleri sırasında oluşabilecek temel hataları yakalama ve sürece devam etme.

## ⚙️ Kurulum ve Çalıştırma

Bu betik, Google Colab ortamında çalıştırılmak üzere tasarlanmıştır.

1.  **Google Colab Ortamı:**
    * Bir Google hesabınızla [Google Colab](https://colab.research.google.com/) açın.
    * Bu `Veri_kazima001.ipynb` dosyasını Colab'e yükleyin veya yeni bir notebook oluşturup kodu kopyalayın.

2.  **Kütüphane Kurulumu:**
    * Notebook'un ilk kod hücresi gerekli `arxiv` ve `semanticscholar` kütüphanelerini `!pip install` komutu ile kuracaktır.

3.  **Google Drive Bağlantısı:**
    * Notebook, Google Drive'ınıza erişim izni isteyecektir. Bu izni vermeniz, toplanan verilerin Drive'ınıza kaydedilebilmesi için gereklidir.

4.  **Yapılandırma (İsteğe Bağlı):**
    * Notebook içindeki `CONFIG` sözlüğünü düzenleyerek arama yapılacak terimleri (`search_terms`), terim başına maksimum sonuç sayısını (`max_results_per_term`), çıktı formatlarını (`output_formats`) ve kaydedilecek dosya yolunu (`output_path`) değiştirebilirsiniz.
    ```python
    CONFIG = {
        "search_terms": [
            "diamond based quantum computing",
            "NV centers in diamond",
            # ... diğer terimler
        ],
        "max_results_per_term": 150,
        "output_formats": ["json", "csv"],
        "output_path": "/content/drive/MyDrive/quantum_diamond_dataset",
        "api_delay": 1
    }
    ```

5.  **Çalıştırma:**
    * Tüm hücreleri sırayla çalıştırın. Betik, veri toplama sürecini başlatacak, ilerlemeyi gösterecek ve sonuçları Google Drive'daki belirttiğiniz yola kaydedecektir.

## 📝 İş Akışı

1.  Gerekli paketler kurulur ve Google Drive bağlanır.
2.  Kullanıcı tarafından tanımlanan arama terimleri ve diğer yapılandırma parametreleri yüklenir.
3.  **arXiv Veri Toplama:** Her bir arama terimi için arXiv API'si kullanılarak makaleler aranır ve ilgili metaveriler (ID, başlık, özet, yazarlar, yayın tarihi, DOI, PDF URL) toplanır.
4.  **Semantic Scholar Veri Toplama:** Her bir arama terimi için Semantic Scholar API'si kullanılarak makaleler aranır ve ilgili metaveriler toplanır. (*Not: Mevcut betikte Semantic Scholar API'sinin `limit` parametresi ile ilgili bir sorun bulunmaktadır, aşağıya bakınız.*)
5.  Toplanan veriler birleştirilir.
6.  Temel veri kalite kontrolleri yapılır (benzersiz makale sayısı, özeti olmayanlar, PDF linki olanlar).
7.  Sonuçlar, yapılandırmada belirtilen formatlarda (JSON ve/veya CSV) Google Drive'a kaydedilir.
8.  İşlem sonunda bir özet raporu sunulur.

## ⚠️ Bilinen Sorunlar ve İyileştirme Alanları

* **Semantic Scholar API Limiti:** Betiğin mevcut haliyle çalıştırılması durumunda, Semantic Scholar API'sinden veri çekilirken "The limit parameter must be between 1 and 100 inclusive" hatası alınmaktadır. `CONFIG` içindeki `max_results_per_term` değeri (150) bu limite uymamaktadır.
    * **Çözüm Önerisi:** `CONFIG["max_results_per_term"]` değerini Semantic Scholar için 100 veya daha düşük bir değere ayarlayın veya Semantic Scholar API'sinin sayfalama (pagination) özelliğini destekleyip desteklemediğini kontrol ederek daha fazla sonuç çekmek için döngüsel istekler gönderin.
* **arXiv Deprecation Uyarısı:** `arxiv` kütüphanesinin `Search.results` metodu kullanımdan kaldırılmıştır (`DeprecationWarning`). Bunun yerine `Client.results` kullanılması önerilmektedir. Kütüphane güncellenerek veya kodda ilgili değişiklik yapılarak bu uyarı giderilebilir.
* **Yinelenen Kayıtlar:** Betik, farklı arama terimleri veya farklı API'lerden aynı makalenin birden fazla kez çekilmesine neden olabilir. Toplam 700 kayıt toplanmasına rağmen benzersiz makale sayısının 344 olması buna işaret etmektedir. Kaydetmeden önce veya sonra daha kapsamlı bir yinelenen kayıt temizleme adımı eklenebilir (örneğin, DOI veya başlık+yazar kombinasyonuna göre).
* **Özet Uzunluğu Kontrolü:** Özeti olmayan makaleler için yapılan kontrol (`len(p['abstract'])<100`) bazı kısa ama geçerli özetleri de filtreleyebilir. Bu eşik değeri ihtiyaca göre ayarlanabilir veya daha sofistike bir boş özet kontrolü eklenebilir.
* **Hata Yönetimi:** API'lerden gelen hatalar temel `try-except` blokları ile yakalanmaktadır. Daha detaylı hata loglaması ve farklı hata türlerine göre özel aksiyonlar (örn: geçici API sorunlarında yeniden deneme mekanizması) eklenebilir.

## 📄 Çıktı Dosyaları

## 📞 İletişim

🐛 **Bug Report**: GitHub Issues kullanın  
💡 **Feature Request**: Discussions bölümünden önerinizi paylaşın  
📧 E-posta: [mehmetaksoy49@gmail.com]

- Pull Request ile katkıda bulunun
- Projeyi yıldızlamayı unutmayın! ⭐

---

**Not**: Bu proje eğitim amaçlı geliştirilmiştir ve akademik çalışmalarda referans olarak kullanılabilir.

Betik başarıyla tamamlandığında, Google Drive'ınızda `CONFIG["output_path"]` ile belirtilen konumda aşağıdaki dosyalar oluşturulur:

* `quantum_diamond_dataset.json`: Toplanan tüm makale verilerini içeren JSON dosyası.
* `quantum_diamond_dataset.csv`: Toplanan tüm makale verilerini içeren CSV dosyası (yazar listeleri noktalı virgülle ayrılmış string olarak kaydedilir).
