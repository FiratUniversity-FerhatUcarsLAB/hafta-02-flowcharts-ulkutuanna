BAŞLA

  // --- 1. Kimlik Doğrulama ---
  YAZ "TC Kimlik numaranızı ve şifrenizi girin."
  OKU TC_KIMLIK_NO, SIFRE
  
  EĞER (KIMLIK_DOGRULAMA(TC_KIMLIK_NO, SIFRE) BAŞARILI İSE)
    YAZ "Giriş başarılı. Hoş geldiniz."
  DEĞİLSE
    YAZ "Hatalı kimlik bilgileri. Lütfen tekrar deneyin."
    BITIR
  SON_EĞER
  
  // --- 2. Poliklinik ve Doktor Seçimi ---
  YAZ "Lütfen bir poliklinik seçin:"
  GOSTER POLIKLINIK_LISTESI
  OKU SECILEN_POLIKLINIK
  
  YAZ "Lütfen bir doktor seçin:"
  GOSTER DOKTOR_LISTESI(SECILEN_POLIKLINIK)
  OKU SECILEN_DOKTOR
  
  // --- 3. Uygun Saatleri Görüntüleme ---
  YAZ "Lütfen bir tarih seçin."
  OKU SECILEN_TARIH
  
  YAZ "Seçilen doktorun uygun saatleri:"
  GOSTER UYGUN_SAATLER(SECILEN_DOKTOR, SECILEN_TARIH)
  OKU SECILEN_SAAT
  
  // --- 4. Randevu Onaylama ---
  YAZ "Randevu bilgileriniz:"
  YAZ "Doktor: " + SECILEN_DOKTOR
  YAZ "Tarih: " + SECILEN_TARIH
  YAZ "Saat: " + SECILEN_SAAT
  
  YAZ "Randevuyu onaylamak için 'EVET' yazın."
  OKU ONAY_CEVABI
  
  EĞER (ONAY_CEVABI = "EVET" İSE)
    // Randevu veritabanına kaydedilir
    RANDEVU_OLUSTUR(KULLANICI_ID, SECILEN_DOKTOR, SECILEN_TARIH, SECILEN_SAAT)
    
    // SMS gönderme
    YAZ "Randevunuz başarıyla oluşturulmuştur."
    SMS_GONDER(KULLANICI_TELEFON, "Randevunuz " + SECILEN_TARIH + " tarihinde onaylanmıştır.")
  DEĞİLSE
    YAZ "Randevu oluşturma işlemi iptal edildi."
  SON_EĞER

BİTİR

BAŞLA

  // --- 1. Kimlik Doğrulama ---
  YAZ "Lütfen kimlik numaranızı ve şifrenizi girin:"
  OKU KIMLIK_NO, SIFRE
  
  EĞER (KULLANICI_DOGRULAMA(KIMLIK_NO, SIFRE) BAŞARILI İSE)
    YAZ "Giriş başarılı. Lütfen tahlil listenize göz atın."
  DEĞİLSE
    YAZ "Hatalı kimlik bilgileri. Lütfen tekrar deneyin."
    BITIR // Sistemden çıkış yap
  SON_EĞER
  
  // --- 2. Tahlil Varlığı ve Sonuç Durumu Kontrolü ---
  
  YAZ "Lütfen görüntülemek istediğiniz tahlil dosyasını seçin:"
  GOSTER KULLANICI_TAHLIL_LISTESI(KIMLIK_NO)
  OKU SECILEN_TAHLIL_ID
  
  EĞER (TAHLIL_MEVCUT_MU(SECILEN_TAHLIL_ID) İSE)
    // Tahlil mevcut, şimdi sonucun hazır olup olmadığını kontrol et
    EĞER (TAHLIL_SONUCU_HAZIR_MI(SECILEN_TAHLIL_ID) İSE)
      // Sonuç hazırsa görüntüleme ve indirme seçeneği sun
      YAZ "Tahlil sonucunuz görüntülendi."
      GOSTER TAHLIL_SONUCU(SECILEN_TAHLIL_ID)
      YAZ "Sonucu PDF olarak indirmek için 'INDIR' yazın."
      OKU INDIRME_KOMUTU
      EĞER (INDIRME_KOMUTU = "INDIR" İSE)
        PDF_DOSYASINI_INDIR(SECILEN_TAHLIL_ID)
        YAZ "Tahliliniz PDF olarak indiriliyor."
      DEĞİLSE
        YAZ "İndirme işlemi iptal edildi."
      SON_EĞER
    DEĞİLSE
      // Sonuç henüz hazır değilse bekleme mesajı ver
      YAZ "Tahlil sonucunuz henüz hazırlanmadı. Lütfen daha sonra tekrar deneyin."
    SON_EĞER
  DEĞİLSE
    YAZ "Hata: Seçilen tahlil dosyası bulunamadı."
  SON_EĞER

BİTİR

