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
