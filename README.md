#  Azərbaycan Pərakəndə Zəncirlərinin Müştərilərini Klasterləşdirmə

> *"Hər müştəri eyni deyil — bəzisi hər həftə gəlir, bəzisi bir dəfə gəlib yaddan çıxıb."*  
> Bu layihənin əsas məqsədi məhz budur: kimin kim olduğunu tapmaq.

---

## 💡 Nə etdik, niyə etdik?

Düşün ki, sənin bir mağazan var. Minlərlə müştərin alış-veriş edir. Amma hamısına eyni endirim SMS-i göndərirsən. Bu nə qədər ağıllıdır? Çox da deyil.

Biz burada **RFM analizi** + **K-Means klasterləşdirmə** istifadə edərək müştəriləri 4 fərqli qrupa ayırdıq. Hər qrupun öz xarakteri var, öz davranış tərzi var. Artıq marketing kampaniyalarını hədəfli aparmaq mümkündür.

---

## 📦 Dataset

- **Mənbə:** [Kaggle — eCommerce Behavior Data](https://www.kaggle.com/datasets/mkechinov/ecommerce-behavior-data-from-multi-category-store)
- **Fayl:** `2019-Oct.csv` (Oktyabr 2019 — çox böyük data, biz nümunə götürdük)
- **Filtr:** Ən azı 3 əməliyyatı olan **100,000 unikal istifadəçi** saxlandı

Dataset üç növ event ehtiva edir:
| Event | Nə deməkdir? |
|-------|-------------|
| `view` | Məhsula baxdı |
| `cart` | Səbətə atdı |
| `purchase` | Aldı  |

---

## Metodologiya

###  RFM Metrikası

Hər müştəri üçün 3 rəqəm hesabladıq:

| Metrika | Azərbaycanca | Nə ölçür? |
|---------|-------------|-----------|
| **R**ecency | Yaxınlıq | Son alışdan neçə gün keçib? Az = yaxşı |
| **F**requency | Tezlik | Neçə dəfə alıb? Çox = yaxşı |
| **M**onetary | Məbləğ | Nə qədər pul xərcləyib? Çox = yaxşı |

###  Feature Engineering

Məlumatların paylanması gözəl deyildi (sağa yayılmış). Buna görə **log transform** tətbiq etdik, sonra **StandardScaler** ilə normallaşdırdıq.

###  Optimal Klaster Sayı

İki metodla yoxladıq:
- 🔵 **Elbow metodu** → k=4 ən aydın "dirsək" nöqtəsidir
- 🔴 **Silhouette skoru** → k=4 yüksək ayrışma keyfiyyəti göstərir

Nəticə: **4 klaster** seçildi.

###  K-Means Modeli

```
KMeans(n_clusters=4, random_state=42, n_init=15, max_iter=500)
```

Son **Silhouette skoru** ≈ yaxşı ayrışma — model öz işini gözəl gördü.

---

## 👥 Müştəri Seqmentləri

| # | Seqment | Kim bunlar? | Nə etmək lazım? |
|---|---------|------------|----------------|
| 0 | 😴 **Passiv Qənaətcil** | Çoxdan gəlməyib, az alıb, ucuz məhsul seçib | Yenidən cəlb kampaniyası, böyük endirim |
| 1 | 💎 **Sadiq Yüksək Dəyərli** | Tez-tez gəlir, çox xərcləyir, aktiv | VIP proqramı, xüsusi təkliflər, onları saxla! |
| 2 | 🌱 **Yeni Yüksək Potensial** | Çox yeni müştəri, az alış amma orta-yüksək məbləğ | Onboarding kampaniyası, ilk alışa bonus |
| 3 | ⚠️ **Riskdə Olan Dəyərli** | Əvvəl çox xərcləyib amma uzun müddətdir yoxdur | Geri qazanma kampaniyası, fərdi müraciət |

---

## 📊 Vizualizasiyalar

Layihədə aşağıdakı qrafiklər var:

-  Event növlərinin bar chart + pie chart paylanması
-  Ən çox satılan Top 10 brend (horizontal bar)
-  Alış qiymətlərinin histoqramı (median xətti ilə)
-  RFM metrikalarının paylanması (3 panel)
-  **PCA 2D xəritəsi** — klasterlər vizual olaraq görünür
-  Seqmentlər üzrə müştəri paylanması (donut chart)
-  RFM Scatter Matrix

---

## ⚔️ K-Means vs DBSCAN

DBSCAN da sınadıq. Nəticə?

> Bu dataset üçün **K-Means daha üstündür.**  
> DBSCAN çox noise tapır, sparse məlumat strukturu ilə yaxşı işləmir.

---

##  Layihə Strukturu

```
📁 project/
│
├── 📓 notebook.ipynb          ← Əsas iş faylı
├── 📄 README.md               ← Siz buradasınız
└── 📊 2019-Oct.csv            ← Dataset (Kaggle-dan yüklənir)
```

---

## 🛠️ İstifadə Edilən Texnologiyalar

```python
pandas · numpy · matplotlib · seaborn
plotly · scikit-learn · kagglehub
```

---

## 🚀 Başlamaq Üçün

```bash
# Asılılıqları yüklə
pip install kagglehub[pandas-datasets] scikit-learn plotly seaborn

# Notbuku aç
jupyter notebook notebook.ipynb
```

---

## 📝 Qeyd

Passiv Qənaətcil və Riskdə Olan Dəyərli seqmentləri PCA xəritəsində bir-birinə yaxın görünür — bu normal haldır. PCA 2 dimensiona endirir, amma RFM rəqəmlərində aralarında əhəmiyyətli fərq mövcuddur (xüsusilə `monetary` dəyərində: ~158 vs ~885).

---

*Bu layihə müştəri davranışını anlamaq üçün hazırlanıb. Real retail bizneslərdə bu tip seqmentasiya marketing ROI-ni əhəmiyyətli dərəcədə artıra bilər.*
