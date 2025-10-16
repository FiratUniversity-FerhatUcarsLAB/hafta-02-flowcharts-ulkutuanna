BAŞLA

  // Sistem 7/24 sürekli çalışır.
  DÖNGÜ (DOĞRU) // Bu, sonsuz bir döngüdür.
  
    // --- 1. Sensörleri Oku ve Tehdit Algıla ---
    OKU T_SENSORU = SICAKLIK_SENSORUNDAN_GELEN_VERI()
    OKU H_SENSORU = HAREKET_SENSORUNDAN_GELEN_VERI()
    OKU K_SENSORU = KAPI_SENSORUNDAN_GELEN_DURUM()
    
    // --- 2. Tehdit Seviyesine Göre Aksiyon Al ---
    
    // Tehlike Seviyesi 1: Hafif Tehdit (Sadece bildirim gönder)
    EĞER (H_SENSORU = "HAREKET_ALGILANDI") İSE
      YAZ "Koridorda hareket algılandı. Bildirim gönderiliyor."
      BILDIRIM_GONDER("Koridorda hareket algılandı.")
    SON_EĞER
    
    // Tehlike Seviyesi 2: Orta Tehdit (Bildirim ve hafif uyarı)
    EĞER (T_SENSORU > 50) İSE
      YAZ "Yüksek sıcaklık algılandı. Yangın alarmı gönderiliyor."
      YUKSEK_SESLE_UYARI("Yüksek sıcaklık!")
      BILDIRIM_GONDER("Evde sıcaklık yükseliyor.")
    SON_EĞER
    
    // Tehlike Seviyesi 3: Ciddi Tehdit (Alarmı çalıştır ve bildirim gönder)
    EĞER (K_SENSORU = "KAPI_ACIK") İSE
      YAZ "KAPI AÇIK! ALARM VERİLİYOR!"
      ALARM_CALISTIR()
      BILDIRIM_GONDER("Dış kapı açıldı! ACİL DURUM!")
      
      // Alarm sıfırlama komutu bekliyor.
      DÖNGÜ (ALARM_SIFIRLAMA_KOMUTU_GELDI_MI = YANLIŞ)
        BEKLE_BIRAZ() // CPU kullanımını azaltmak için bekle
      SON_DÖNGÜ
      ALARM_DURDUR()
      YAZ "Alarm sıfırlandı."
    SON_EĞER
    
  SON_DÖNGÜ // Döngü tekrar başlar
  
BİTİR
