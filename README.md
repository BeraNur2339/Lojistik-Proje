
# 🚚 Dinamik Lojistik Ağ Analizi ve Rota Optimizasyonu

Bu proje, bir lojistik ağındaki dinamik maliyet değişimlerini ve veri erişim (Hash) maliyetlerini hesaba katarak en verimli rotayı bulan bir **C dili** uygulamasıdır. 

## 📌 Proje Özeti
Proje kapsamında, **Dijkstra Algoritması** kullanılarak bir şehirden diğerine en kısa yol hesaplanmış, ancak sadece fiziksel mesafe değil, aynı zamanda her düğümdeki veri trafiği (Hash Erişim Maliyeti) de toplam maliyete dahil edilmiştir.

### 🛠 Kullanılan Teknolojiler
* **Dil:** C
* **Veri Yapısı:** Adjacency List (Komşuluk Listesi)
* **Algoritma:** Dijkstra's Shortest Path

## 📊 Analiz Senaryoları

### 1. Bölüm: Dinamik Rota (Bera'nın Çalışması)
Lojistik yoğunluk nedeniyle **B-D yolu** maliyeti 2 birimden **4 birime** çıkarılmıştır. Bu değişim sonucunda algoritma, B düğümü yerine C düğümü üzerinden geçerek **A -> C -> D -> G** rotasını optimize etmiştir.
* **Fiziksel Maliyet:** 8 Birim

### 2. Bölüm: Hash ve Sistem Maliyeti
Her düğümün (şehrin) kendine has bir Hash erişim maliyeti vardır. 
* **Örnek:** A-C-D-F rotası için toplam sistem maliyeti **21** birim olarak hesaplanmıştır.

### 3. Bölüm: Karmaşıklık Analizi (Big-O)
* **Dijkstra:** $O(V^2)$ - Komşuluk listesi ve dizi tabanlı öncelik yönetimi ile.
* **Hash Erişimi:** $O(1)$ - Her düğümdeki veri sorgulaması sabit zamanda gerçekleşir.

## 🚀 Nasıl Çalıştırılır?
1. Herhangi bir C derleyicisine (GCC, Clang vb.) sahip olduğunuzdan emin olun.
2. `main.c` dosyasını derleyin:
   ```bash
   gcc main.c -o lojistik_analiz
