# genetik_optimizasyonu
# BLG 307 - Yapay Zeka Sistemleri | Genetik Algoritma ile Kısıtlı Optimizasyon

> **Ders:** BLG 307 Yapay Zeka Sistemleri (2025-2026 Güz)  
> **Proje:** 1. Proje Ödevi - Genetik Algoritma Uygulaması  
> **Senaryo:** #2 - Endüstriyel Boya Karışımı Optimizasyonu

<div align="center">

| Öğrenci Bilgileri | Detay |
| :--- | :--- |
| **Adı Soyadı** | Zeliha GÜVEN |
| **Öğrenci No** | 2312721042 |
| **Bölüm** | Bilgisayar Mühendisliği |
| **GitHub** | [Repo Bağlantısı] |

</div>

---

##  1. Proje Özeti ve Amaç
Bu projenin temel amacı, endüstriyel bir problemin (boya karışımı) matematiksel modelini oluşturmak ve bu modeli **Genetik Algoritma (Genetic Algorithm - GA)** kullanarak optimize etmektir. Problem, klasik türev tabanlı yöntemlerin zorlanabileceği **doğrusal olmayan (non-linear)** bir yapıya ve katı eşitlik kısıtlarına sahiptir.

Projede, **Python** programlama dili ve vektörel işlemler için **NumPy** kütüphanesi kullanılarak evrimsel bir hesaplama motoru sıfırdan inşa edilmiştir.

---

##  2. Matematiksel Model 

Problem, iki farklı pigmentin ($x_1$ ve $x_2$) karışım oranlarının belirlenmesi üzerinedir.

### 2.1. Amaç Fonksiyonu (Objective Function)
Maksimize edilmek istenen renk kalitesi fonksiyonu:

$$\text{Maximize } f(x) = 5x_1 + 2x_2 - x_1x_2$$

Burada:
* **$5x_1$**: A Pigmentinin kaliteye yüksek pozitif etkisi.
* **$2x_2$**: B Pigmentinin kaliteye düşük pozitif etkisi.
* **$-x_1x_2$**: Pigmentlerin kimyasal etkileşiminden doğan negatif etki (Non-linearite).

### 2.2. Kısıtlar (Constraints)
Çözüm uzayı aşağıdaki fiziksel ve operasyonel sınırlarla kısıtlanmıştır:

1.  **Karışım Bütünlüğü:** $x_1 + x_2 = 100$ (Toplam oran daima %100 olmalıdır).
2.  **Minimum Dozaj:** $x_1 \ge 30$ (A pigmenti kritik bileşen olduğu için en az %30 kullanılmalıdır).
3.  **Alan Sınırları:** $0 \le x_1, x_2 \le 100$.

---

##  3. Algoritma Mimarisi ve Yöntem

Bu projede, kısıtlı optimizasyon problemlerini çözmek için **Ceza Yöntemi (Penalty Method)** entegreli bir Genetik Algoritma tasarlanmıştır.

### 3.1. Kısıt Yönetimi: Dinamik Ceza Fonksiyonu
Genetik algoritmalar doğası gereği kısıtsız arama yapar. Kısıtları sağlamak için "Uygunluk (Fitness)" fonksiyonu modifiye edilmiştir.

$$\text{Fitness}(x) = f(x) - \sum (\text{Ceza Katsayısı} \times \text{İhlal Miktarı})$$

Kod içerisinde uygulanan ceza mekanizması:
* **Eşitlik İhlali ($x_1+x_2 \neq 100$):** Farkın mutlak değeri `1000` katsayısı ile çarpılarak puan düşülür.
* **Sınır İhlali ($x_1 < 30$):** Eksik kalan miktar `1000` katsayısı ile cezalandırılır.

*Bu yöntem sayesinde algoritma, matematiksel olarak zorlanmadan, "hayatta kalmak için" toplamı 100 yapmayı evrimsel süreçte öğrenir.*

### 3.2. Genetik Operatörler

| Bileşen | Seçilen Yöntem | Gerekçe |
| :--- | :--- | :--- |
| **Kodlama** | **Gerçel Değer (Real-Valued)** | Problem sürekli (continuous) bir uzayda olduğu için binary kodlama yerine float değerler kullanılmıştır. |
| **Seçilim** | **Turnuva Seçimi (Tournament)** | Rulet tekerleğine göre seçim baskısını (selection pressure) daha iyi kontrol etmek ve erken yakınsamayı önlemek için seçilmiştir. (Boyut: 5) |
| **Çaprazlama** | **Aritmetik Çaprazlama** | Ebeveynlerin genlerinin ağırlıklı ortalamasını alarak ara değerler üretir. Sayısal optimizasyon için idealdir. |
| **Mutasyon** | **Gaussian (Normal) Mutasyon** | Genin mevcut değerine küçük bir gürültü (noise) ekleyerek yerel minimumdan kurtulmayı sağlar. |

---

## 4. Kurulum ve Kullanım Yönergeleri

Proje `Jupyter Notebook` ortamında geliştirilmiştir.

### 4.1. Gereksinimler
Projenin çalışması için aşağıdaki kütüphanelerin yüklü olması gerekir:
```bash
pip install numpy matplotlib
