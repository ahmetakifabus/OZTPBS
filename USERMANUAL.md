# Python Script Kullanim Kilavuzu

## Hizli Baslangic

### 1. Kurulum

```bash
git clone https://github.com/KULLANICI_ADINIZ/sinav-karne-analiz.git
cd sinav-karne-analiz
python -m venv venv
venv\Scripts\activate
source venv/bin/activate
pip install -r requirements.txt
```

### 2. Demo ile Test

```bash
python analiz.py --demo
```

### 3. Kendi Verilerinizle Kullanim

```bash
python analiz.py --sinav sinav.csv --karne karne.csv
python analiz.py --sinav sinav.csv --karne karne.csv --output sonuclar/
python analiz.py --sinav sinav.csv --karne karne.csv --no-plot
```

## Ciktilar

Script calistirildiginda su dosyalar olusur:

### 1. Grafikler (output/regresyon_analizi.png)
- 15 adet grafik (5 ders x 3 gorunum)
- Basit regresyon scatter plot'lari
- Coklu regresyon tahmin vs gercek
- Katsayi bar chart'lari
- Yuksek cozunurluk (300 DPI)

### 2. CSV Raporlari

**output/regresyon_karsilastirma.csv**
```csv
Ders,Basit_R2,Basit_RMSE,Coklu_R2,Coklu_RMSE,R2_Artisi
TURKCE,0.8234,3.456,0.8891,2.987,0.0657
MATEMATIK,0.7892,4.123,0.8543,3.567,0.0651
```

**output/detayli_sonuclar.csv**
```csv
RUMUZ,Ders,Sinav_T,Karne_T,Tahmin_Basit,Tahmin_Coklu
OGR001,TURKCE,52.34,54.12,53.45,53.89
OGR001,MATEMATIK,48.23,49.87,49.12,50.01
```

### 3. Konsol Raporu
Terminalde detayli analiz raporu gosterilir:
- Basit regresyon sonuclari
- Coklu regresyon sonuclari
- Performans karsilastirmalari
- Ozet istatistikler

## 🎯 Gelişmiş Kullanım

### Python Scriptinden Kullanım

```python
from analiz import SinavKarneAnaliz

analiz = SinavKarneAnaliz(
    sinav_dosya="sinav.csv",
    karne_dosya="karne.csv"
)

analiz.veri_yukle()
analiz.t_puanlarini_ekle()
analiz.verileri_birlestir()
analiz.analiz_yap()

for ders in analiz.DERSLER:
    basit = analiz.sonuclar[ders]['basit']
    coklu = analiz.sonuclar[ders]['coklu']
    print(f"{ders}:")
    print(f"  Basit R2 = {basit['r2']:.4f}")
    print(f"  Coklu R2 = {coklu['r2']:.4f}")

analiz.grafik_olustur("my_output")
analiz.rapor_olustur("my_output")
```

### Ozellestirilmis Analiz

```python
import pandas as pd
from analiz import SinavKarneAnaliz

sinav = pd.read_csv("sinav.csv", sep=";")
karne = pd.read_csv("karne.csv", sep=";")

sinav_filtre = sinav[sinav['RUMUZ'].str.startswith('OGR1')]
karne_filtre = karne[karne['RUMUZ'].str.startswith('OGR1')]

sinav_filtre.to_csv("temp_sinav.csv", sep=";", index=False)
karne_filtre.to_csv("temp_karne.csv", sep=";", index=False)

analiz = SinavKarneAnaliz("temp_sinav.csv", "temp_karne.csv")
analiz.calistir("filtered_output")
```

## Sorun Giderme

### Problem: ModuleNotFoundError

```bash
pip install -r requirements.txt --upgrade
```

### Problem: UnicodeDecodeError (CSV okuma hatasi)

```python
pd.read_csv(self.sinav_dosya, sep=";", encoding='utf-8-sig')
pd.read_csv(self.sinav_dosya, sep=";", encoding='latin-1')
```

### Problem: Grafik gosterilmiyor

```bash
sudo apt-get install python3-tk
brew install python-tk
python analiz.py --sinav sinav.csv --karne karne.csv --no-plot
```

### Problem: Matplotlib hatasi (sunucu/SSH)

```python
import matplotlib
matplotlib.use('Agg')
```

## 📈 Performans İpuçları

### Büyük Veri Setleri

```python
# Chunk'lar halinde okuma
def veri_yukle_buyuk(self, chunk_size=1000):
    chunks = []
    for chunk in pd.read_csv(self.sinav_dosya, sep=";", chunksize=chunk_size):
        chunks.append(chunk)
    self.sinav_data = pd.concat(chunks, ignore_index=True)
```

### Paralel Islem

```python
from joblib import Parallel, delayed

def paralel_analiz(self):
    results = Parallel(n_jobs=-1)(
        delayed(self.basit_regresyon)(
            self.veri[f"{ders}_T_SINAV"].values,
            self.veri[f"{ders}_T_KARNE"].values
        )
        for ders in self.DERSLER
    )
```

## Grafik Ozellestirme

### Renkleri Degistirme

```python
DERSLER = {
    "TURKCE": ("TDS", "TURKCE", "#FF0000", ""),
    "MATEMATIK": ("MDS", "MAT", "#00FF00", ""),
}
```

### Grafik Boyutunu Ayarlama

```python
fig = plt.figure(figsize=(30, 20), facecolor='#f8f9fa')
fig = plt.figure(figsize=(15, 10), facecolor='#f8f9fa')
```

### DPI Ayarlama (Cozunurluk)

```python
plt.savefig(grafik_dosya, dpi=150)
plt.savefig(grafik_dosya, dpi=300)
plt.savefig(grafik_dosya, dpi=600)
```

## Batch Isleme

Birden fazla dosya ciftini analiz etmek icin:

```bash
#!/bin/bash

for year in 2021 2022 2023 2024
do
    echo "Analiz ediliyor: $year"
    python analiz.py \
        --sinav "data/sinav_${year}.csv" \
        --karne "data/karne_${year}.csv" \
        --output "output_${year}" \
        --no-plot
done

echo "Tum yillar analiz edildi!"
```

```bash
chmod +x batch_analiz.sh
./batch_analiz.sh
```

## Excel Destegi

```python
pip install openpyxl

def veri_yukle(self):
    self.sinav_data = pd.read_excel(self.sinav_dosya)
    self.karne_data = pd.read_excel(self.karne_dosya)
```

## Egitim Amacli Kullanim

### Jupyter Notebook ile

```bash
pip install jupyter
jupyter notebook
```

```python
from analiz import SinavKarneAnaliz
import matplotlib.pyplot as plt

%matplotlib inline

analiz = SinavKarneAnaliz("sinav.csv", "karne.csv")
analiz.calistir("output", grafik_goster=True)
```

### Ogrencilere Gosterim

```python
analiz = SinavKarneAnaliz("sinav.csv", "karne.csv")

print("1. Veriler yukleniyor...")
analiz.veri_yukle()
input("Devam etmek icin Enter'a basin...")

print("2. T-puanlari hesaplaniyor...")
analiz.t_puanlarini_ekle()
input("Devam etmek icin Enter'a basin...")
```

## Debugging

### Verbose Mod

```python
import logging

logging.basicConfig(level=logging.DEBUG)
logger = logging.getLogger(__name__)

logger.debug(f"Veri boyutu: {len(self.veri)}")
logger.debug(f"Sutunlar: {self.veri.columns.tolist()}")
```

### Veri Kontrolu

```python
def veri_kontrol(self):
    print("Eksik degerler:")
    print(self.veri.isnull().sum())
    
    print("\nVeri tipleri:")
    print(self.veri.dtypes)
    
    print("\nIstatistikler:")
    print(self.veri.describe())
```

## Ipuclari

1. Virtual Environment Kullanin
2. Git Kullanin
3. Dokumante Edin
4. Test Edin
5. Yedekleyin

## Yardim

Sorulariniz icin:
- Email: your.email@example.com
- GitHub Issues
- Discussions

---

**Happy Analyzing!**
