BAŞLA

  // --- 1. Kullanıcı Girişi ve Yetkilendirme ---
  YAZ "Lütfen öğrenci numaranızı ve şifrenizi girin."
  OKU OGRENCI_NUMARASI, SIFRE
  
  EĞER (KIMLIK_DOGRULAMA(OGRENCI_NUMARASI, SIFRE) BAŞARILI İSE)
    YAZ "Giriş başarılı. Ders kayıt ekranına yönlendiriliyorsunuz."
  DEĞİLSE
    YAZ "Hatalı öğrenci numarası veya şifre."
    BITIR
  SON_EĞER
  
  EĞER (DERS_KAYIT_DONEMI_ACIK_MI İSE)
    // Öğrenci ders kaydı yapmaya yetkili
    DERS_LISTESI = BOŞ_LISTE
    
    // --- 2. Ders Seçimi ve Kontroller Döngüsü ---
    DÖNGÜ (KULLANICI_DERS_SEÇİMİ_YAPIYOR_MU İSE)
      YAZ "Almak istediğiniz dersin kodunu girin (Çıkış için 'BITIR' yazın):"
      OKU SECILEN_DERS_KODU
      
      EĞER (SECILEN_DERS_KODU = "BITIR") İSE
        DÖNGÜDEN_ÇIK
      SON_EĞER

      EĞER (DERS_MEVCUT_MU(SECILEN_DERS_KODU) İSE)
        // --- Kontenjan Kontrolü ---
        EĞER (DERS_KONTENJANI_DOLU_MU(SECILEN_DERS_KODU) İSE)
          YAZ "Hata: Dersin kontenjanı dolmuştur."
        DEĞİLSE
          // --- Ön Koşul Kontrolü ---
          EĞER (ON_KOSULLAR_SAGLANIYOR_MU(SECILEN_DERS_KODU, OGRENCI_NUMARASI) İSE)
            // --- Zaman Çakışması Kontrolü ---
            EĞER (ZAMAN_CAKISMASI_VAR_MI(SECILEN_DERS_KODU, DERS_LISTESI) İSE)
              YAZ "Hata: Seçtiğiniz dersin saati, mevcut bir dersinizle çakışıyor."
            DEĞİLSE
              // --- Kredi Limiti Kontrolü ---
              EĞER (KREDI_LIMITI_ASILDI_MI(SECILEN_DERS_KODU, DERS_LISTESI) İSE)
                YAZ "Hata: Kredi limitinizi aşıyorsunuz. Lütfen ders çıkarın."
              DEĞİLSE
                // Tüm kontroller başarılı, dersi listeye ekle
                DERS_LISTESI.EKLE(SECILEN_DERS_KODU)
                YAZ "Ders başarıyla eklendi."
              SON_EĞER
            SON_EĞER
          DEĞİLSE
            YAZ "Hata: Dersin ön koşulları sağlanmamıştır."
          SON_EĞER
        SON_EĞER
      DEĞİLSE
        YAZ "Hata: Geçersiz ders kodu."
      SON_EĞER
    SON_DÖNGÜ
    
    // --- 3. Danışman Onayı ve Kesin Kayıt ---
    YAZ "Ders seçimleriniz danışmanınızın onayına gönderiliyor."
    EĞER (DANISMAN_ONAYI_ALINDI_MI(OGRENCI_NUMARASI, DERS_LISTESI) İSE)
      DERS_KAYIT_KESINLESTIR(OGRENCI_NUMARASI, DERS_LISTESI)
      YAZ "Ders kaydınız başarıyla tamamlandı. Hayırlı olsun!"
    DEĞİLSE
      YAZ "Ders kaydınız danışmanınız tarafından reddedildi. Lütfen listeyi gözden geçirin."
    SON_EĞER
    
  DEĞİLSE
    YAZ "Ders kayıt dönemi şu anda kapalıdır."
  SON_EĞER

BITIR
