BAŞLA

  // --- Değişken Tanımlamaları ---
  
  GERCEK_PIN = "1234"
  PIN_DENEME_SAYISI = 0
  HESAP_BAKIYESI = 1500
  GUNLUK_CEKIM_LIMITI = 1000
  CEKILEN_TOPLAM = 0
  ISLEM_DEVAM_ETSIN_MI = "EVET"
  
  // --- PIN Doğrulama Döngüsü ---

  DÖNGÜ (PIN_DENEME_SAYISI < 3)
  
    YAZ "Lütfen PIN kodunuzu girin:"
    OKU GIRILEN_PIN
    
    EĞER (GIRILEN_PIN = GERCEK_PIN) İSE
      YAZ "PIN doğru. Hoş geldiniz."
      DÖNGÜDEN_ÇIK
    DEĞİLSE
      YAZ "Hatalı PIN. Lütfen tekrar deneyin."
      PIN_DENEME_SAYISI = PIN_DENEME_SAYISI + 1
    
  EĞER (PIN_DENEME_SAYISI = 3) İSE
    YAZ "PIN'inizi 3 kez hatalı girdiniz. Kart bloke edildi."
    SON
  
  // --- Ana İşlem Döngüsü ---
  
  DÖNGÜ (ISLEM_DEVAM_ETSIN_MI = "EVET")
  
    YAZ "Hesabınızdaki bakiye: " + HESAP_BAKIYESI + " TL"
    YAZ "Bugün çekebileceğiniz limit: " + (GUNLUK_CEKIM_LIMITI - CEKILEN_TOPLAM) + " TL"
    YAZ "Lütfen çekmek istediğiniz tutarı girin:"
    OKU CEKIM_TUTARI
    
    // --- Tutar Kontrolü (20 TL'nin katı mı?) ---
    
    EĞER (CEKIM_TUTARI % 20 != 0) İSE
      YAZ "Lütfen 20 TL ve katları şeklinde bir tutar girin."
    
    // --- Bakiye Kontrolü ---
    
    DEĞİLSE EĞER (CEKIM_TUTARI > HESAP_BAKIYESI) İSE
      YAZ "Yetersiz bakiye. Bu işlemi gerçekleştiremezsiniz."
    
    // --- Günlük Limit Kontrolü ---
    
    DEĞİLSE EĞER ((CEKIM_TUTARI + CEKILEN_TOPLAM) > GUNLUK_CEKIM_LIMITI) İSE
      YAZ "Günlük çekim limitinizi aşıyorsunuz. Lütfen daha düşük bir tutar girin."
    
    // --- Başarılı İşlem ---
    
    DEĞİLSE
      HESAP_BAKIYESI = HESAP_BAKIYESI - CEKIM_TUTARI
      CEKILEN_TOPLAM = CEKILEN_TOPLAM + CEKIM_TUTARI
      YAZ "İşlem başarılı. Lütfen paranızı alın."
      YAZ "Yeni bakiye: " + HESAP_BAKIYESI + " TL"
    
    // --- İşlem Tekrarı Seçeneği ---
    
    YAZ "Başka bir işlem yapmak istiyor musunuz? (EVET/HAYIR)"
    OKU ISLEM_DEVAM_ETSIN_MI
    ISLEM_DEVAM_ETSIN_MI = BÜYÜK_HARF(ISLEM_DEVAM_ETSIN_MI)
    
  SON_DÖNGÜ
  
  YAZ "İyi günler dileriz."
  
SON
