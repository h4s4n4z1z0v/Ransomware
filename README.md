# 🛑 AES-GCM & RSA Ransomware PoC

⚠️ **Diqqət!** Bu kod zərərli məqsədli ransomware (file encrypting malware) nümunəsidir. Yalnız **təhsil və tədqiqat məqsədilə** nəzərdə tutulub. Real həyatda istifadəsi qanun pozuntusudur və etik deyil.

---

## 📜 Ümumi Məlumat

Bu Python script, hədəf sistemdəki sənədləri AES-GCM alqoritmi ilə şifrələyir. AES şifrələmə açarını isə RSA public key vasitəsilə şifrələyərək hücumçunun əlində gizlətmək üçün nəzərdə tutulub.

Əsas addımlar:
✅ AES-128 açarı yarat  
✅ AES açarını RSA ilə şifrələ  
✅ İstifadəçinin `Documents`, `Pictures`, `Desktop` qovluqlarındakı bütün faylları şifrələ  
✅ Masaüstündə ransom note yaradaraq hücumçunun əlaqə məlumatlarını göstər  
✅ Özünü təkrar işə salmaq üçün Windows startup folder-ə kopyala  
✅ Virtual maşında işlədiyini aşkarlasa, dayandır

---

## ⚙️ Əsas Fəaliyyətlər

- **AES Şifrələmə**: Hər fayl üçün 12-byte nonce ilə AES-GCM istifadə edir.
- **RSA İnteqrasiyası**: AES açarını `ATTACKER_PUBLIC_KEY_PEM` dəyişənində saxlanılan RSA açarı ilə OAEP padding ilə şifrələyir.
- **Persistence**: Windows sistemlərində `Startup` qovluğuna kopyalanır.
- **Virtual Maşın Aşkarlanması**: VMware və VirtualBox-a qarşı deteksiya.
- **Ransom Note**: `Desktop` qovluğunda `README_DECRYPT.txt` faylı yaradır.

---

## 📁 Fayl Quruluşu

fileencrypter.py


---

## 🚨 Hüquqi Məlumat

Bu script yalnız **təhsil məqsədilə** paylaşılır:
- Etik hacking, POC (Proof of Concept) analizləri və sınaqlar üçün uyğundur.
- Hər hansı real şəbəkədə icazəsiz istifadə qanunsuzdur!
- Zərərli məqsədlər üçün istifadəsi etik və hüquqi baxımdan **məsuliyyət daşıyır**.

---

## 🛡️ Tətbiq və Müdafiə Tövsiyələri

- Fayllarınızın **offline backup**-unu saxlayın.  
- Antivirus və EDR həllərini aktiv edin.  
- Sistemlərinizi **mütəmadi yeniləyin**.

**📎 Qeyd:** Faylların decryption-u yalnız AES açarı və RSA private açarı varsa mümkündür. Əks halda, fayllar bərpa olunmur!

---
