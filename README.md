# Kara Kitap — Goodreads Yorumları Üzerinden Alımlama İncelemesi

Depo adresi: https://github.com/sabricanatamanoder/KaraKitapIncelemesi

Bu depo, aşağıdaki makalenin çözümlemelerini yeniden üretmek için gereken defterleri ve betikleri içerir.

> Öder, S. A. (2026). Beklenti Ufkundan Ufuk İzdüşümüne: *Kara Kitap*'ın Goodreads Yorumları Örneğinde Hesaplamalı Bir Alımlama Modeli Önerisi. *MANAS Sosyal Araştırmalar Dergisi*, [cilt](sayı), [sayfa]. [DOI]

Makale, Orhan Pamuk'un *Kara Kitap* romanına yazılmış Türkçe ve İngilizce Goodreads yorumlarını Hans Robert Jauss'un alımlama estetiği çerçevesinde inceler. Üç teknik birlikte kullanılır: kelime bulutu, G² anahtarlık çözümlemesi ve kodlama destekli yakın okuma.

## İçerik

| Dosya | Ne yapar |
|---|---|
| `g2_anahtarlik.ipynb` | Puan bantlarına göre ayırt edici sözcükleri hesaplar. Makaledeki Tablo 1 ve Tablo 2'yi üretir. Türkçe tarafta Zemberek kullanır. |
| `kodlama_destekli_yakin_okuma.ipynb` | Kodlama şemasını yorumlara uygular. Eksen ve etiket dağılımlarını, güvenirlik ölçümlerini ve Tablo 3-6'yı üretir. |
| `dipnot_testleri.ipynb` | Dipnotlardaki ki-kare ve Fisher testlerini yeniden üretir. Veri dosyası gerektirmez. |
| `kod_kitabi.md` | Altı eksen ve yirmi dört etiketin tanımları, anahtar ifadeler. |
| `full_analysis_FINAL.xlsx` | Çözümlemelerin dayandığı 872 yorumluk veri seti. |
| `durak_g2_tr.txt` | G² aşamasında Türkçe için elenen durak sözcükler. |
| `durak_bulut_tr.txt` · `durak_bulut_en.txt` | Kelime bulutu aşamasında elenen durak sözcükler. |

## Veri

Veri seti `full_analysis_FINAL.xlsx` dosyasındadır ve depo köküne konulmuştur.

Yorumlar Goodreads platformunda *Kara Kitap* hakkında 21 Mayıs 2007 ile 31 Aralık 2025 arasında yazılmıştır. Derleme, Ankara Yıldırım Beyazıt Üniversitesi Sosyal ve Beşeri Bilimler Etik Kurulunun 23 Şubat 2026 tarihli ve 02/80 sayılı kararıyla yapılmıştır.

Toplanan 1167 yorumun 9'u yalnızca puan, bağlantı veya emoji içerdiğinden dil sınıflandırması dışında bırakılmıştır. Kalan 1158 yorum otuz dokuz dilde yazılmıştır. Çözümleme, karşılaştırmaya yetecek sayıya ulaşan iki dille sınırlandırılmıştır. Dosya bu iki dilin 872 yorumunu içerir: 564 İngilizce, 308 Türkçe.

### Sütunlar

Çözümlemelerde kullanılan sütunlar şunlardır.

| Sütun | İçerik |
|---|---|
| `record_id` | Yorumun kimlik numarası. Makaledeki TK- ve İK- atıfları bu numaraya karşılık gelir. |
| `date` | Yorumun yazıldığı tarih. |
| `language_final` | Yorumun dili: `tr` veya `en`. |
| `rating_numeric_raw` | Okurun verdiği yıldız, 1 ile 5 arasında. Puansız yorumlarda boştur. |
| `comment` | Yorumun ham metni. |
| `review_clean` | Temizlenmiş metin. |

`record_id` 1 ile 874 arasında ilerler. Dosyada 872 satır bulunur, çünkü 87 ve 543 numaralı kayıtlar derleme sırasında elenmiştir. Numaralar bu yüzden kesintisiz değildir. Atıf verirken satır sırası değil `record_id` esas alınmalıdır.

Dosyada `sentiment_label`, `sentiment_confidence`, `rating_sentiment_match` gibi sütunlar da bulunur. Bunlar önceki bir keşif aşamasından kalmıştır. Makaledeki hiçbir çözümlemede kullanılmamıştır.

## Colab'da çalıştırma

Her defterin başındaki **Open in Colab** rozetine tıklayın. Sonra `Çalışma zamanı → Tümünü çalıştır` deyin. Defterler veri dosyasını bu depodan kendiliğinden indirir, elle yükleme gerekmez.

Kendi verinizle çalışmak isterseniz defterin veri hücresindeki `VERI_URL` değişkenini değiştirin. Sütun adları yukarıdaki tabloyla aynı olmalıdır.

Defterler çıktıyı hem ekrana basar hem de indirilebilir dosya olarak kaydeder.

### Zemberek kurulumu hakkında

`g2_anahtarlik.ipynb` Türkçe kelimeleri sözlük birimine indirmek için Zemberek kullanır. Kütüphanenin Python uyarlaması, ANTLR runtime'ın 4.8 sürümünü ister. Bu sürüm güncel Python'da doğrudan kurulmaz, çünkü kaldırılmış olan `typing.io` modülünü çağırır.

Defterin ilk hücresi bu sorunu kendiliğinden çözer. Kaynak paketi indirir, iki dosyadaki `typing.io` çağrısını düzeltir ve elle kurar. Hücreyi atlamayın. Kurulum bir dakika kadar sürer ve oturum başına bir kez yapılır.

Alternatif runtime sürümleri (4.9.3, 4.13.2) denenmiş ve çalışmamıştır. 4.13.2 kurulur, ancak çözümleme sırasında ATN sürüm hatası verir.

## Yöntem notları

**Sayım birimi.** Türkçe eklemeli bir dildir. Bir kök metinde onlarca biçimde görünür ve yüzey biçimleri sayıldığında kökün ağırlığı dağılır. Bu nedenle Türkçe korpusta sayım birimi sözlük birimidir. İngilizce korpusta kelimeler metinde göründükleri hâlleriyle sayılmıştır.

**Durak sözcükler.** Depoda üç liste bulunur. `durak_bulut_tr.txt` ve `durak_bulut_en.txt` kelime bulutu aşamasında kullanılmıştır. İngilizce liste NLTK'nin standart listesidir. Türkçe liste, bulutlarda tekrar eden genel kitap söz varlığını bastırmak için araştırmacı tarafından kurulmuştur.

G² aşamasında İngilizce için aynı NLTK listesi kullanılır. Üzerine yalnızca altı alan sözcüğü eklenir: *book*, *books*, *read*, *reading*, *reader*, *readers*. Ekleme defterin içinde açıkça görünür. Türkçe için ayrı ve daha kısa bir liste kullanılır (`durak_g2_tr.txt`), çünkü bulut listesi içerik sözcükleri de barındırır.

Bu ayrımın bir sonucu vardır. Bulut listesindeki *cümle*, *eser*, *fazla*, *edebiyat* ve *gerçekten* sözcükleri G² tablolarında ayırt edici olarak görünür. Bu sözcükler bulutlarda görünemezdi, çünkü o aşamada elenmişlerdi. Durum, kelime bulutlarının ön işleme kararlarına ne ölçüde bağlı olduğunu gösterir. Makale bu nedenle bulutları hipotez üretme işleviyle sınırlandırmıştır.

**Eşik.** Bir kelimenin listeye girebilmesi için G² değerinin 3,84'ü aşması gerekir (p < 0,05). Ayrıca kelimenin ilgili bantta en az üç kez ve en az üç farklı yorumda geçmesi aranır. İkinci koşul, tek bir uzun yorumun listeyi doldurmasını önler.

**G²'nin statüsü.** Çok sayıda kelime aynı anda sınandığı için G² burada bir anlamlılık kararı olarak değil, aşırı temsili sıraya dizen bir ölçü olarak kullanılmıştır.

## Bilinen sınırlar

- Biçimbilimsel çözümleyici özel adları ortak adlardan ayırmaz. *galip* ve *rüya* sözlük birimlerinde roman kişilerinin adları, aynı yazılışa sahip ortak adlarla birleşir.
- Belirsizlik giderme hatasız değildir. Tabloya giren her kelime bağlamlı dizinle denetlenmiştir.
- Anahtarlık değeri bazı kelimelerde birkaç uzun yorumun etkisiyle yükselir. Yayılım ölçütü bu yoğunlaşmayı tümüyle engellemez.
- G² tek tek kelimeleri sayar. Anlamca bir arada duran ama sözlükçe dağılmış öbekler ölçüme görünmez kalır.

## Atıf

Bu depodaki materyalleri kullanırsanız makaleye atıf yapınız.

Zemberek için: Akın, A. A. ve Akın, M. D. (2007). Zemberek, an open source NLP framework for Turkic languages. *Structure*, 10, 1–5.

Python uyarlaması için: Loodos. (2023). *Zemberek-Python* (Sürüm 0.2.3) [Bilgisayar yazılımı]. https://github.com/Loodos/zemberek-python

G² ölçüsü için: Dunning, T. (1993). Accurate methods for the statistics of surprise and coincidence. *Computational Linguistics, 19*(1), 61–74.

## Lisans

Kod: MIT. Kodlama kitabı: CC BY 4.0.

Veri dosyasındaki yorum metinleri Goodreads kullanıcılarına aittir. Burada araştırmanın denetlenebilmesi için paylaşılmıştır. Yeniden yayımlamadan önce platformun kullanım koşullarını gözetiniz.

## İletişim

Sabrican Ataman Öder — Ankara Yıldırım Beyazıt Üniversitesi — sabricanatamanoder@aybu.edu.tr

---

## English

This repository holds the notebooks and scripts needed to reproduce the analyses in the article above. The study examines Turkish and English Goodreads reviews of Orhan Pamuk's *The Black Book* within Hans Robert Jauss's reception aesthetics, using word clouds, G² keyness analysis and code-assisted close reading.

The dataset is included in `full_analysis_FINAL.xlsx` and holds 872 reviews, 564 in English and 308 in Turkish. Open any notebook with its **Open in Colab** button and run all cells. The notebooks read the data file from the repository, so nothing needs to be uploaded. The G² notebook lemmatises the Turkish corpus with Zemberek. Its first cell installs a patched ANTLR runtime, which is required and takes about a minute.

The counting unit differs by language. Turkish is agglutinative, so the Turkish corpus is counted by lemma. English words are counted in the forms in which they appear. The two tables are therefore read on their own terms rather than compared value by value.
