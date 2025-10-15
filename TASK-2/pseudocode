BAŞLA

  // --- 1. Kullanıcı Oturumu ve Sepet Yüklemesi ---
  
  EĞER (KULLANICI_GİRİŞ_YAPTI_MI İSE)
    // Kayıtlı kullanıcının sepet verisini veritabanından çek.
    SEPET = KULLANICI_SEPETINI_VERITABANINDAN_AL
  DEĞİLSE
    // Ziyaretçi için tarayıcı çerezinden veya oturum ID'sinden sepet verisini al.
    SEPET = ÇEREZDEN_VEYA_OTURUMDAN_SEPET_AL
  
  // --- 2. Ürün Ekleme ve Stok Kontrolü ---

  DÖNGÜ (KULLANICI_ALIŞVERİŞ_YAPIYOR_MU İSE)
    YAZ "Lütfen ürün seçin ve adedini girin:"
    OKU EKLENECEK_URUN, EKLENECEK_ADET
    
    // Stok kontrolü yap
    EĞER (STOK_DURUMU(EKLENECEK_URUN) >= EKLENECEK_ADET) İSE
      SEPETE_EKLE(EKLENECEK_URUN, EKLENECEK_ADET)
      YAZ "Ürün sepete eklendi."
    DEĞİLSE
      YAZ "Hata: Yetersiz stok. Lütfen daha az adet girin."
    
    YAZ "Alışverişe devam etmek istiyor musunuz? (EVET/HAYIR)"
    OKU DEVAM_CEVAP
    EĞER (DEVAM_CEVAP = "HAYIR" İSE)
      DÖNGÜDEN_ÇIK
  SON_DÖNGÜ
  
  // --- 3. Ödeme Aşaması ---
  
  BAŞLA_ÖDEME_AŞAMASI
    YAZ "Sepet toplamı: " + SEPET_TOPLAMI_HESAPLA()
    
    // İndirim kodu kontrolü
    YAZ "İndirim kodunuz varsa girin:"
    OKU İNDİRİM_KODU
    EĞER (İNDİRİM_KODU_GEÇERLİ_Mİ(İNDİRİM_KODU) İSE)
      İNDİRİM_TUTARI = İNDİRİM_HESAPLA(İNDİRİM_KODU)
      TOPLAM_TUTAR = SEPET_TOPLAMI_HESAPLA() - İNDİRİM_TUTARI
      YAZ "İndirim uygulandı. Yeni toplam: " + TOPLAM_TUTAR
    DEĞİLSE
      TOPLAM_TUTAR = SEPET_TOPLAMI_HESAPLA()
      YAZ "Geçersiz indirim kodu."

    // Kargo ve vergi hesaplama
    YAZ "Lütfen kargo adresinizi girin:"
    OKU KARGO_ADRESİ
    KARGO_ÜCRETİ = KARGO_HESAPLA(KARGO_ADRESİ, SEPET)
    TOPLAM_TUTAR = TOPLAM_TUTAR + KARGO_ÜCRETİ
    YAZ "Kargo ücreti eklendi. Son toplam: " + TOPLAM_TUTAR
    
    YAZ "Lütfen ödeme yöntemi seçin:"
    OKU ÖDEME_YÖNTEMİ

    // --- 4. Ödeme İşlemi ---
    
    YAZ "Ödeme bilgilerinizi girin."
    OKU ÖDEME_BİLGİLERİ

    EĞER (ÖDEME_SAĞLAYICIYA_GÖNDER(ÖDEME_BİLGİLERİ) BAŞARILI İSE)
      // Ödeme başarılı, siparişi veritabanına kaydet
      SİPARİŞİ_VERİTABANINA_KAYDET(SEPET, TOPLAM_TUTAR)
      // Stokları güncelle
      STOK_GÜNCELLE(SEPET)
      YAZ "Siparişiniz başarıyla alınmıştır. Teşekkürler!"
    DEĞİLSE
      YAZ "Ödeme başarısız. Lütfen tekrar deneyin."
  BİTİR_ÖDEME_AŞAMASI

BİTİR
