# NVIDIA CUDA ve cuDNN Temiz Kurulum Rehberi (Windows)

Bu doküman, NVIDIA ekran kartı kullanan geliştiriciler ve yapay zeka projeleriyle uğraşanlar için **CUDA Toolkit** ve **cuDNN** kurulumunu adım adım, temiz ve doğru şekilde nasıl yapacaklarını açıklar. Bu rehber, özellikle PyTorch / YOLO / OpenCV gibi kütüphaneleri GPU ile kullanmak isteyenlere yöneliktir.

---
## 🧼 🔄 CUDA + cuDNN Tam Temiz Kurulum (Önerilen)


Bu bölümde, bilgisayarında daha önce kurulu olan CUDA, cuDNN, NVIDIA sürücüleri gibi bileşenleri tamamen temizleyip sıfırdan sağlam bir kurulum yapmak isteyen kullanıcılar için tüm adımlar detaylı şekilde anlatılmıştır.

### 🧼 1. Eski NVIDIA Yazılımlarını Kaldır
1.1 Denetim Masasından kaldırılacaklar:
Git → Denetim Masası > Programlar > Programlar ve Özellikler

Aşağıdakileri sırayla kaldır:

NVIDIA Graphics Driver

NVIDIA CUDA Toolkit

NVIDIA PhysX System Software

NVIDIA GeForce Experience (isteğe bağlı)

NVIDIA Nsight bileşenleri (varsa)

CUDA Visual Studio Integration

NVIDIA HD Audio veya FrameView SDK (varsa)

✅ Her kaldırma işleminden sonra yeniden başlat gerekmiyor ama en sonunda bir kez yapılması önerilir.

### 🧹 2. Sistem Dosyalarını Temizle
2.1 Aşağıdaki klasörleri manuel olarak sil:

C:\Program Files\NVIDIA Corporation
C:\Program Files\NVIDIA GPU Computing Toolkit
C:\ProgramData\NVIDIA Corporation
⚠️ ProgramData gizli klasördür. Görünmüyorsa klasör ayarlarından gizli dosyaları gösterin.

### 🧾 3. Ortam Değişkenlerini Temizle
Git → Başlat > Ortam Değişkenlerini Düzenle > Sistem Özellikleri > Ortam Değişkenleri

3.1 Path Değişkeni içinde:
Aşağıdaki gibi eski CUDA yollarını silin:


```bash
C:\Program Files\NVIDIA Corporation
C:\Program Files\NVIDIA GPU Computing Toolkit
C:\ProgramData\NVIDIA Corporation
```

3.2 CUDA_PATH varsa → silin veya doğru sürüme göre güncelleyin.
### 🧰 4. (Opsiyonel) Display Driver Uninstaller (DDU) ile Tam Temizlik
Bazı kullanıcılar için önerilir:

[Display Driver Uninstaller (DDU)](https://www.wagnardsoft.com/)


“Safe Mode” üzerinden çalıştırılır.

Tüm NVIDIA bileşenlerini kaldırır.

### 🔁 5. Bilgisayarı Yeniden Başlat
Yukarıdaki işlemler tamamlandığında sistemde eski sürümden kalan hiçbir dosya olmamalıdır.
---
## ✅ Gerekli Bilgiler ve Uyum Kontrolü

### 1. Ekran Kartı Uyumluluğu

CUDA kurmak için NVIDIA ekran kartınızın **CUDA destekli** olması gerekir. Destekleyen bazı seriler:

- RTX 4000, 3000, 2000 serisi (Ampere, Ada)
- GTX 1000, 1600 serisi (Pascal, Turing)
- Quadro, Titan, Tesla serileri

### 2. Hangi CUDA Sürümü Hangi Ekran Kartıyla Uyumlu?

| Ekran Kartı         | Uyumlu CUDA Sürümü    | Açıklama                         |
| ------------------- | --------------------- | -------------------------------- |
| RTX 40xx (Ada)      | 12.2+                 | En güncel CUDA desteklenir       |
| RTX 30xx (Ampere)   | 11.1 – 12.9           | En yaygın destekli mimari        |
| RTX 20xx (Turing)   | 10.1 – 11.x           | CUDA 11 tavsiye edilir           |
| GTX 16xx / GTX 10xx | 10.0 – 11.x           | Yeni PyTorch için 11.x yeterli   |
| Quadro / Tesla      | Mimariye göre değişir | Üretici sayfasından kontrol edin |

### 3. Compute Capability (Hesaplama Yeteneği) Nedir?

**CUDA Compute Capability**, ekran kartınızın CUDA ile hangi düzeyde çalışabileceğini gösteren versiyon numarasıdır. Örneğin:

| GPU Modeli  | Compute Capability |
| ----------- | ------------------ |
| RTX 3050 Ti | 8.6                |
| RTX 4090    | 8.9                |
| GTX 1650    | 7.5                |
| GTX 1080 Ti | 6.1                |

- PyTorch, TensorFlow ve diğer framework'ler, bazı işlemleri yalnızca belirli Compute Capability üzeri kartlarda çalıştırır.
- En az 3.5 üzeri olan kartlar çoğu modern CUDA uygulaması için yeterlidir.
- NVIDIA resmi Compute Capability listesi: https://developer.nvidia.com/cuda-gpus

**Neden önemlidir?**

- CUDA derleyicileri, bu versiyona göre kodu optimize eder.
- Eski kartlar (örneğin 3.0) artık desteklenmeyebilir.
- CUDA sürümünü seçerken kartınızın Compute Capability’sine uyumlu olmasına dikkat edin.

---

## 📌 Ek Uyumluluk ve Kurulum Kontrolleri

### CUDA ve cuDNN Sürüm Uyum Tablosu

Bu tablo en güncel derin öğrenme framework sürümleri ile önerilen CUDA ve cuDNN kombinasyonlarını göstermektedir:

| Framework     | Sürüm     | Uyumlu CUDA | Uyumlu cuDNN | Notlar                           |
|---------------|-----------|-------------|--------------|----------------------------------|
| PyTorch       | 2.3.0     | 12.1        | 8.9          | CUDA 12.1 wheel mevcut           |
| PyTorch       | 2.2.2     | 11.8        | 8.7          | En stabil sürüm                  |
| TensorFlow    | 2.15.0    | 11.8        | 8.6.0        | GPU destekli versiyon            |
| TensorFlow    | 2.14.0    | 11.2        | 8.1          | Düşük sürüm kartlar için uygun   |
| ONNX Runtime  | 1.17.0    | 12.x        | 8.9+         | CUDA Execution Provider          |
| JAX           | 0.4.25    | 11.8/12.1   | 8.7+         | pip ile manuel CUDA seçimi gerek |

> Güncel tekerlek (wheel) dosyaları için ilgili framework'lerin resmi PyPI veya GitHub sayfalarını kontrol edin.

### cuDNN Kurulumunun Doğrulanması

```python
import ctypes
try:
    ctypes.CDLL("cudnn64_8.dll")
    print("cuDNN başarıyla yüklendi.")
except OSError:
    print("cuDNN yüklenemedi.")
```

---

## 🧰 Ek Sistem Gereksinimleri

- **Microsoft Visual C++ Redistributable**: CUDA Toolkit, Visual C++ Redistributable paketine ihtiyaç duyar. En son sürümünü [buradan indir](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist).
- **Visual Studio**: CUDA Toolkit, Visual Studio 2019/2022 ile uyumludur. CUDA kurarken entegre olacak şekilde kurulu olmalıdır.

---

## 🐍 Conda Kullanıcıları İçin Notlar

> **Not**: Conda kullanıcıları için bazı cuDNN sürümleri `conda install` komutuyla doğrudan kurulamayabilir. Bu durumda `pip` tercih edilmeli ya da cuDNN manuel kopyalanmalıdır.

---


---

## 🧠 Ekstra Bilgiler

- **TensorRT**: NVIDIA'nın model hızlandırma kütüphanesidir. Model çıkarım sürecini hızlandırmak isteyenler kurabilir.
- **Nsight Tools**: Performans analizi ve hata ayıklama için güçlü araçlar sunar.

---

---

## 🙌 Destek Olmak İstersen

Bu rehber faydalı olduysa:

- ⭐️ [GitHub'ta yıldız ver](https://github.com/mustafabugraokurer/cuda-cudnn-kurulum-rehberi)
- 🐛 Hata bulduysan [issue aç](https://github.com/mustafabugraokurer/cuda-cudnn-kurulum-rehberi/issues)
- 📥 Katkı sunmak istersen `pull request` gönderebilirsin

---

Bu doküman açık kaynak olarak hazırlanmıştır. Dilediğiniz gibi dağıtabilir, güncelleyebilir veya kendi projelerinize dahil edebilirsiniz.


