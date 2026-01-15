# Proje Yapisi

Bu dosya, projenizin klasor ve dosya yapisini gosterir.

## Dizin Yapisi

```
sinav-karne-analiz/
│
├── index.html                 
├── README.md                  
├── LICENSE                    
├── .gitignore                
├── PROJE_YAPISI.md           
│
├── data/                     
│   ├── examples/             
│   │   ├── sinav_ornek.csv
│   │   └── karne_ornek.csv
│   └── templates/            
│       ├── sinav_template.csv
│       └── karne_template.csv
│
├── docs/                     
│   ├── kullanim-kilavuzu.md
│   ├── csv-format.md
│   └── analiz-yontemleri.md
│
├── screenshots/              
│   ├── hero.png
│   ├── upload.png
│   ├── simple-regression.png
│   ├── multiple-regression.png
│   └── comparison.png
│
└── assets/                   
    ├── logo.png
    └── favicon.ico
```

## Dosya Aciklamalari

### Ana Dosyalar

- **index.html**: Tum uygulamayi iceren tek HTML dosyasi. CSS ve JavaScript dahil.
- **README.md**: Projenin kapsamli aciklamasi, kullanim talimatlari ve ozellikler.
- **LICENSE**: MIT lisans metni.
- **.gitignore**: Git'in takip etmeyecegi dosya ve klasorlerin listesi.

### Data Klasoru

#### examples/
Kullanicilara ornek olmasi icin hazir veri setleri:

**sinav_ornek.csv** (30 ogrenci):
```csv
RUMUZ;TDS;MDS;FDS;SDS;DDS
OGR001;85;90;78;88;92
OGR002;75;82;85;79;88
OGR003;92;88;90;85;90
```

**karne_ornek.csv** (30 ogrenci):
```csv
RUMUZ;TURKCE;MAT;FEN;SOSYAL;DIN
OGR001;4,5;4,8;4,2;4,6;4,9
OGR002;3,8;4,1;4,3;3,9;4,4
OGR003;4,7;4,5;4,6;4,3;4,6
```

#### templates/
Kullanicilarin kendi verilerini girebilmeleri icin bos sablonlar:

**sinav_template.csv**:
```csv
RUMUZ;TDS;MDS;FDS;SDS;DDS
OGR001;;;;;;;
OGR002;;;;;;;
```

**karne_template.csv**:
```csv
RUMUZ;TURKCE;MAT;FEN;SOSYAL;DIN
OGR001;;;;;;;
OGR002;;;;;;;
```

### Docs Klasoru

Detayli dokumantasyon dosyalari:

- **kullanim-kilavuzu.md**: Adim adim kullanim talimatlari
- **csv-format.md**: CSV dosya formati detaylari
- **analiz-yontemleri.md**: Istatistiksel yontemlerin aciklamasi

### Screenshots Klasoru

README.md dosyasinda kullanilacak ekran goruntuleri:

- **hero.png**: Ana sayfa gorunumu
- **upload.png**: Dosya yukleme ekrani
- **simple-regression.png**: Basit regresyon analizi
- **multiple-regression.png**: Coklu regresyon analizi
- **comparison.png**: Karsilastirma tablosu

### Assets Klasoru

Ek gorsel kaynaklar:

- **logo.png**: Proje logosu
- **favicon.ico**: Tarayici sekmesi ikonu

## Kurulum Adimlari

### 1. Repository Olusturma

```bash
mkdir sinav-karne-analiz
cd sinav-karne-analiz
git init
```

### 2. Dosyalari Ekleme

```bash
touch index.html README.md LICENSE .gitignore
mkdir -p data/examples data/templates docs screenshots assets
touch data/examples/sinav_ornek.csv
touch data/examples/karne_ornek.csv
touch data/templates/sinav_template.csv
touch data/templates/karne_template.csv
touch docs/kullanim-kilavuzu.md
touch docs/csv-format.md
touch docs/analiz-yontemleri.md
```

### 3. Git Islemleri

```bash
git add .
git commit -m "Ilk commit: Proje yapisi olusturuldu"
git remote add origin https://github.com/KULLANICI_ADINIZ/sinav-karne-analiz.git
git push -u origin main
```

### 4. GitHub Pages Aktivasyonu

1. GitHub repo sayfaniza gidin
2. Settings > Pages
3. Source: "Deploy from a branch" secin
4. Branch: "main" ve "/ (root)" secin
5. Save'e tiklayin
6. 2-3 dakika bekleyin
7. Siteniz yayinda!

## Ornek Veri Setleri Olusturma

### Python ile Otomatik Veri Uretme

```python
import pandas as pd
import numpy as np

np.random.seed(42)
n_students = 30

sinav_data = {
    'RUMUZ': [f'OGR{i:03d}' for i in range(1, n_students + 1)],
    'TDS': np.random.randint(60, 100, n_students),
    'MDS': np.random.randint(60, 100, n_students),
    'FDS': np.random.randint(60, 100, n_students),
    'SDS': np.random.randint(60, 100, n_students),
    'DDS': np.random.randint(60, 100, n_students)
}

sinav_df = pd.DataFrame(sinav_data)
sinav_df.to_csv('data/examples/sinav_ornek.csv', sep=';', index=False)

karne_data = {
    'RUMUZ': [f'OGR{i:03d}' for i in range(1, n_students + 1)],
    'TURKCE': (sinav_df['TDS'] / 20 + np.random.normal(0, 0.2, n_students)).round(1),
    'MAT': (sinav_df['MDS'] / 20 + np.random.normal(0, 0.2, n_students)).round(1),
    'FEN': (sinav_df['FDS'] / 20 + np.random.normal(0, 0.2, n_students)).round(1),
    'SOSYAL': (sinav_df['SDS'] / 20 + np.random.normal(0, 0.2, n_students)).round(1),
    'DIN': (sinav_df['DDS'] / 20 + np.random.normal(0, 0.2, n_students)).round(1)
}

karne_df = pd.DataFrame(karne_data)
karne_df.to_csv('data/examples/karne_ornek.csv', sep=';', index=False, decimal=',')

print("Ornek veriler olusturuldu!")
```

## Minimum Gerekli Dosyalar

GitHub Pages'de yayinlamak icin sadece su dosyalar yeterlidir:

```
sinav-karne-analiz/
├── index.html      
├── README.md       
└── LICENSE         
```

## Gelismis Yapi

```
sinav-karne-analiz/
├── src/
│   ├── js/
│   │   ├── analysis.js
│   │   ├── charts.js
│   │   └── utils.js
│   ├── css/
│   │   └── custom.css
│   └── components/
│       ├── header.js
│       └── footer.js
├── tests/
│   └── analysis.test.js
└── package.json
```

## Guncelleme Sureci

```bash
git add .
git commit -m "Yeni ozellik"
git push origin main
```

## Ipuclari

1. Ana kodunuzu main'de tutun, yeni ozellikler icin feature branch'leri olusturun
2. Her gelistirme icin GitHub issue olusturun
3. Degisiklikleri PR ile merge edin
4. Her yeni ozellik eklendiginde README'yi guncelleyin
5. Gorsel dokumantasyon kullanici deneyimini artirir

---

Bu yapi ile projeniz profesyonel ve organize gorunecek!
