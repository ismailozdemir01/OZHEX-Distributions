# OZHEX — Son Kullanıcı Kılavuzu

**Belge türü:** Son kullanıcı kurulum, kullanım ve sorun giderme kılavuzu  
**Ürün sürümü:** Dağıtımda sunulan OZHEX sürümü (şu anki araç zinciri: 0.6.0)  
**Hedef kitle:** OZHEX'i bilgisayarına kurup program derlemek isteyen son kullanıcılar  
**Kaynak kod deposu:** Son kullanıcı kurulumu için gerekli değildir  
**İndirme kanalı:** [OZHEX Binary Distributions — GitHub Releases](https://github.com/ismailozdemir01/OZHEX-Distributions/releases)

> **Önemli ayrım:** Bu belge son kullanıcılar içindir. OZHEX derleyicisinin kaynak kodunu geliştirmek, compiler mimarisini değiştirmek, self-host bootstrap, ABI veya backend geliştirmek için hazırlanmış değildir. Geliştirici/yönetici dokümantasyonu ayrı kaynak kod deposundadır. Son kullanıcılar kaynak kod deposunu klonlamamalı ve oradaki geliştirici derleme betiklerini kurulum yöntemi olarak kullanmamalıdır.

---

## İçindekiler

1. OZHEX nedir?
2. Hangi paketi indirmeliyim?
3. İndirme ve sürüm seçimi
4. Windows 10/11 kurulumu (WSL2 + Ubuntu)
5. Linux x86-64 kurulumu
6. Kurulumun doğrulanması
7. İlk OZHEX programınız
8. Program derleme ve çalıştırma
9. Derleyici komutları
10. Paket yöneticisi ozhexpm
11. Proje klasörü ve günlük kullanım
12. Güncelleme ve kaldırma
13. Güvenlik ve indirme doğrulaması
14. Sorun giderme
15. Sık sorulan sorular
16. Son kullanıcı hızlı başvuru

---

# 1. OZHEX nedir?

OZHEX, `.ozhex` uzantılı kaynak dosyalarını x86-64 hedefleri için native programlara derleyen bir programlama dili ve araç zinciridir.

Son kullanıcı açısından temel iş akışı:

1. Bir `.ozhex` kaynak dosyası oluşturun.
2. Ubuntu/WSL2 veya Linux terminalinde OZHEX derleyicisini çalıştırın.
3. Derleyici hedef platform için çalıştırılabilir dosya üretir.
4. Üretilen programı hedef işletim sisteminde çalıştırın.

OZHEX'in Windows kurulum paketi ile Windows native compiler kavramları birbirinden farklıdır. WSL2 paketi Windows üzerinde bir kurulum sihirbazı sağlar; compiler Linux ortamında, Ubuntu içinde çalışır.

# 2. Hangi paketi indirmeliyim?

| Bilgisayarınız | İndirilecek paket | Compiler nerede çalışır? |
|---|---|---|
| Windows 10/11 x64 | `OZHEX-WSL2-Setup-x64.exe` | WSL2 içindeki Ubuntu'da |
| Linux x86-64 | `ozhex-linux-x86_64.tar.gz` | Doğrudan Linux'ta |
| Windows x64, WSL2 kullanmadan | `OZHEX-Native-Setup-x64.exe` | Yalnızca native Windows paketi yayımlandığında doğrudan Windows'ta |

**Native Windows paketi hakkında:** Native compiler/runtime paketi yayımlanmamışsa bu seçenek kullanılamaz. WSL2 kurucusu native Windows compiler değildir. Releases sayfasında bulunmayan bir dosyayı başka bir depodan veya rastgele indirme sitesinden edinmeye çalışmayın.

# 3. İndirme ve sürüm seçimi

Resmî indirme sayfası:

[OZHEX Binary Distributions — Releases](https://github.com/ismailozdemir01/OZHEX-Distributions/releases)

1. Releases sayfasını açın.
2. Kullanacağınız işletim sistemiyle eşleşen en güncel kararlı sürümü seçin.
3. Yalnızca release açıklamasında belirtilen kurulum dosyasını indirin.
4. Varsa SHA-256 checksum dosyasını da indirin.
5. İndirme dosyasının adının ve hedef mimarisinin seçtiğiniz platformla eşleştiğini kontrol edin.

Bu dağıtım deposu son kullanıcı paketleri ve belgeleri içindir. Compiler kaynak kodu burada yayımlanmaz. Kaynak kod deposu indirme kanalı değildir.

# 4. Windows 10/11 kurulumu (WSL2 + Ubuntu)

## 4.1 Gereksinimler

- 64-bit Windows 10 (build 19041 veya üzeri) ya da uyumlu Windows 11.
- WSL2 desteği.
- Ubuntu dağıtımı.
- Kurulum sırasında Windows özelliklerini etkinleştirmek için yönetici izni.
- Paket indirmek ve Ubuntu bileşenlerini kurmak için internet bağlantısı.

## 4.2 Kurulum

1. [Resmî Releases sayfasından](https://github.com/ismailozdemir01/OZHEX-Distributions/releases) `OZHEX-WSL2-Setup-x64.exe` dosyasını indirin.
2. İndirilen EXE dosyasını çalıştırın.
3. Windows Kullanıcı Hesabı Denetimi (UAC) penceresi açılırsa yayıncı ve dosya kaynağını kontrol ettikten sonra yönetici iznini verin.
4. Kurulum sihirbazındaki adımları tamamlayın.
5. Windows WSL bileşenini etkinleştirmek veya Ubuntu'nun ilk kurulumunu tamamlamak için yeniden başlatma ya da ek onay isterse istenen işlemi tamamlayın.
6. Ubuntu'yu açın. İlk açılışta Ubuntu kullanıcı adı ve parolası oluşturun.
7. Kurulum sihirbazı yeniden başlatmanızı veya tekrar çalıştırmanızı isterse bu adımı tamamlayın.
8. Ubuntu terminalini açın ve aşağıdaki doğrulama komutlarını çalıştırın.

**Çalışma yeri:** `ozhex` komutları Ubuntu terminalinde çalıştırılır. Windows PowerShell veya CMD içinde doğrudan çalıştırılan native Windows compiler komutları değildir.

## 4.3 PATH ayarı

Kurulumdan sonra Ubuntu `ozhex: command not found` diyorsa önce dosyaların kurulu olup olmadığını kontrol edin:

```bash
ls -lah ~/.local/bin/ozhex ~/.local/bin/ozhexpm
```

Dosyalar varsa kullanıcı komut dizinini PATH'e ekleyin:

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
hash -r
```

Komutun bulunup bulunmadığını kontrol edin:

```bash
command -v ozhex
ozhex --version
```

Beklenen komut yolu genellikle `/home/<kullanıcı>/.local/bin/ozhex` biçimindedir. Dosyalar yoksa PATH eklemek çözüm değildir; kurulum sihirbazının tamamlandığını ve doğru Ubuntu dağıtımına kurulum yapıldığını kontrol edin.

# 5. Linux x86-64 kurulumu

## 5.1 Gereksinimler

- Linux x86-64.
- Python 3.11 veya üzeri.
- `tar` ve `gzip`.
- Paket doğrulaması için `sha256sum`.

## 5.2 Arşivi doğrulama

Release dosyalarını aynı sürümün Releases sayfasından indirin. Arşiv ve checksum dosyası aynı dizinde olmalıdır:

```bash
sha256sum -c ozhex-linux-x86_64.tar.gz.sha256
```

Çıktı `OK` değilse arşivi kurmayın. Dosyaları yeniden indirin ve tekrar kontrol edin.

## 5.3 Kurulum

```bash
tar -xzf ozhex-linux-x86_64.tar.gz
cd OZHEX
bash installer/install.sh
```

Kurulumdan sonra:

```bash
export PATH="$HOME/.local/bin:$PATH"
ozhex --version
```

PATH ayarının yeni terminal oturumlarında da geçerli olması için Bash kullanıyorsanız:

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

# 6. Kurulumun doğrulanması

Ubuntu/WSL2 veya Linux terminalinde:

```bash
command -v ozhex
ozhex --version
ozhex --help
ozhexpm --help
```

Başarılı kurulumda:

- `command -v ozhex` derleyicinin tam dosya yolunu gösterir.
- `ozhex --version` kurulu derleyici sürümünü gösterir.
- `ozhex --help` derleyici kullanım biçimini gösterir.
- `ozhexpm --help` paket yöneticisi komutlarını gösterir.

`ozhexpm --version` mevcut CLI arayüzünde bir sürüm seçeneği değildir; paket yöneticisi alt komut bekler. Yardım için `ozhexpm --help` kullanın.

# 7. İlk OZHEX programınız

Bir çalışma klasörü oluşturun:

```bash
mkdir -p ~/ozhex-projects/merhaba
cd ~/ozhex-projects/merhaba
```

`merhaba.ozhex` isimli dosyayı oluşturup şu programı kaydedin:

```ozhex
fonksiyon ana() -> int {
    yaz("Merhaba, OZHEX!");
    dön 0;
}
```

Dosyayı UTF-8 olarak kaydedin. Terminalde:

```bash
ozhex merhaba.ozhex merhaba
```

Derleme başarılı olursa Linux x86-64 için `merhaba` adlı çalıştırılabilir çıktı oluşur. Çalıştırın:

```bash
./merhaba
```

Beklenen program çıktısı:

```text
Merhaba, OZHEX!
```

Bu örnekte `ana` giriş fonksiyonudur, `yaz` standart çıktıya metin gönderir ve `dön 0` başarılı program sonlandırma kodunu belirtir.

# 8. Program derleme ve çalıştırma

Genel kullanım:

```bash
ozhex <girdi.ozhex> <çıktı>
```

Örnek:

```bash
ozhex hesap.ozhex hesap
./hesap
```

Derleyici komutunun başarılı olması programın mantıksal olarak doğru olduğunu tek başına kanıtlamaz. Üretilen programı kendi girdileriniz ve kullanım senaryolarınızla ayrıca çalıştırın.

## 8.1 Kaynak modülü kullanma

Bir program açıkça standart kütüphane modülü içe aktarabilir. Örneğin Linux x86-64 TCP socket modülü:

```ozhex
içe_aktar "net/socket.ozhex";
```

Import yönergesi kendi satırında bulunmalıdır. Modülün platform gereksinimlerini ve API sınırlarını ilgili sürümün dağıtım belgelerinden kontrol edin. Linux'a özgü modüllerin Windows üzerinde aynı şekilde çalıştığını varsaymayın.

# 9. Derleyici komutları

Kurulu self-host compiler'ın komut satırı arayüzü:

```text
ozhex [--target <hedef>] <girdi.ozhex> <çıktı>
ozhex --emit-obj <girdi.ozhex> <nesne.oxo>
ozhex --link <nesne.oxo> <çıktı>
ozhex --runtime <runtime.ozhex> <runtime_blob.ozhex>
```

Desteklenen hedef adları CLI yardımında şu şekilde listelenir:

- `x86_64-linux`
- `x86_64-windows`
- `x86_64-macos`

**Hedef uyarısı:** Bir hedef adının CLI yardımında listelenmesi, o platformda tam runtime, standart kütüphane ve üretim dağıtımının hazır olduğu anlamına gelmez. Son kullanıcılar yalnızca dağıtım deposunda açıkça yayımlanmış ve ilgili platform için belgelenmiş paketi kullanmalıdır. Linux x86-64, mevcut kullanıcı kurulum yoludur.

# 10. Paket yöneticisi ozhexpm

`ozhexpm`, OZHEX paket projelerini ve bağımlılıklarını yönetir.

Yardım:

```bash
ozhexpm --help
```

Mevcut komut adları:

| Komut | İşlev |
|---|---|
| `yeni` / `new` | Paket projesi oluşturur |
| `doğrula` / `dogrula` / `validate` | `ozhex.toml` dosyasını doğrular |
| `kilitle` / `lock` | Bağımlılıkları çözer ve `ozhex.lock` üretir |
| `kur` / `install` | Doğrulanmış bağımlılıkları kurar |
| `agac` / `tree` | Kilitlenmiş bağımlılık ağacını gösterir |
| `paketle` / `pack` | Deterministik kaynak arşivi oluşturur |
| `yayinla` / `publish` | Değiştirilemez paket sürümü yayımlar |

Alt komutların parametrelerini görmek için ilgili komuta `--help` ekleyin. Örneğin:

```bash
ozhexpm yeni --help
ozhexpm doğrula --help
ozhexpm kur --help
```

Paket yayımlama komutu registry erişimi ve yayımlama yetkisi gerektirebilir. Kullanıcı hesabı, token veya registry adresi hakkında doğrulanmamış bilgi girmeyin.

# 11. Proje klasörü ve günlük kullanım

Her proje için ayrı klasör kullanın. Kaynak dosyalarınızı, yapılandırma dosyalarınızı ve üretilen çıktıları düzenli tutun.

Örnek:

```text
merhaba/
├── merhaba.ozhex
└── merhaba
```

- Kaynak dosyalarınızı sürüm kontrolünde saklayın.
- Derlenmiş çıktıları kaynak kodla karıştırmayın.
- Üçüncü taraf paketleri kurmadan önce kaynağını ve sürümünü doğrulayın.
- Platforma özgü modülleri yalnızca belgelenmiş hedefte kullanın.
- Önemli programlar için derleyici sürümünü ve kullanılan bağımlılık sürümlerini kaydedin.

# 12. Güncelleme ve kaldırma

## 12.1 Güncelleme

Yeni sürüm çıktığında yalnızca resmî [Releases sayfasındaki](https://github.com/ismailozdemir01/OZHEX-Distributions/releases) paketi indirin. İşletim sisteminize uygun güncelleme/kaldırma yönergeleri release notlarında belirtilmemişse mevcut kurulumun üzerine gelişigüzel dosya kopyalamayın.

## 12.2 Kaldırma

Kurulumun kaldırma yöntemi kullanılan dağıtım paketine ve sürüme bağlıdır. WSL2/Ubuntu kurulumunu kaldırmak, tüm Ubuntu dağıtımını silmekle aynı işlem değildir. Ubuntu dağıtımını kaldırmak o dağıtım içindeki kişisel dosyaları da silebilir. Sadece OZHEX'i kaldırmak istiyorsanız Ubuntu dağıtımını silmeyin; ilgili sürümün kurulum/kaldırma yönergesini izleyin.

# 13. Güvenlik ve indirme doğrulaması

- Paketleri yalnızca resmî dağıtım deposunun Releases sayfasından indirin.
- Yayınlanmış SHA-256 checksum değerini kontrol edin.
- Checksum uyuşmazlığında dosyayı çalıştırmayın.
- Kaynağı bilinmeyen `.ozhex` dosyalarını derlemeyin.
- Kaynağı bilinmeyen native çalıştırılabilir dosyaları çalıştırmayın.
- Paket ve bağımlılık sürümlerini kaydedin.
- WSL2 kurulumunun Linux compiler kurduğunu, native Windows compiler kurmadığını unutmayın.

Checksum, dosyanın indirirken bozulup bozulmadığını kontrol etmeye yardımcı olur. Tek başına yayıncının kimliğini kanıtlayan dijital imza değildir.

# 14. Sorun giderme

## 14.1 `ozhex: command not found`

Önce kurulum dosyasının varlığını kontrol edin:

```bash
ls -lah ~/.local/bin/ozhex
```

Dosya varsa PATH'i ekleyin:

```bash
export PATH="$HOME/.local/bin:$PATH"
hash -r
command -v ozhex
```

Kalıcı Bash ayarı:

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Dosya yoksa PATH sorunu değildir; kurulum tamamlanmamış veya başka Ubuntu kullanıcısına/dağıtımına yapılmış olabilir.

## 14.2 `ozhexpm --version` alt komut hatası veriyor

Paket yöneticisinin mevcut arayüzünde `--version` seçeneği bulunmayabilir. Komut listesini görmek için:

```bash
ozhexpm --help
```

## 14.3 Derleyici çalışıyor ancak kaynak dosyası bulunamıyor

- Bulunduğunuz klasörü `pwd` ile kontrol edin.
- Dosya adını `ls -la` ile kontrol edin.
- Dosya uzantısının gerçekten `.ozhex` olduğundan emin olun.
- Büyük/küçük harf farkına dikkat edin.

## 14.4 Derleme çıktısı çalıştırılamıyor

- Hedef işletim sisteminin üretilen dosya biçimiyle eşleştiğini kontrol edin.
- Linux çıktısında çalıştırma iznini kontrol edin: `chmod +x ./program`.
- Derleyici ve runtime sürümünün birbiriyle uyumlu olduğundan emin olun.
- Windows PE çıktısı üretilebilmesi, Windows üzerinde native compiler kurulu olduğu anlamına gelmez.

## 14.5 WSL2 veya Ubuntu açılmıyor

Önce Windows'un WSL2 ve Ubuntu kurulum durumunu kontrol edin. WSL/Ubuntu hataları OZHEX kaynak kodu hatası değildir. Windows bileşenleri tamamlanmadan Ubuntu içine kurulum yapılamaz.

# 15. Sık sorulan sorular

### OZHEX'i indirmek için kaynak kod deposunu klonlamalı mıyım?

Hayır. Son kullanıcı paketleri ve kurulum belgeleri [OZHEX-Distributions](https://github.com/ismailozdemir01/OZHEX-Distributions) deposunda yayımlanır. Kaynak kod deposu geliştiriciler içindir.

### Windows kurulum EXE'si native Windows compiler mı kuruyor?

WSL2 kurucusu hayır. WSL2 ve Ubuntu üzerinden Linux x86-64 compiler kurar. Native Windows paketi ayrı bir dağıtımdır ve yalnızca yayımlandığında kullanılabilir.

### Windows üzerinde üretilen programı Ubuntu'da çalıştırabilir miyim?

Hedef dosya biçimi ve ABI farklıdır. Linux için `x86_64-linux` hedefini kullanın.

### macOS veya ARM64 bilgisayarda kullanabilir miyim?

CLI'da listelenen hedefler, tüm platformlar için tamamlanmış dağıtım bulunduğu anlamına gelmez. Bu kılavuz yalnızca Releases sayfasında gerçekten yayımlanmış paketleri desteklenen son kullanıcı kurulumu olarak kabul eder.

### Derleme başarılıysa programım hatasız mıdır?

Hayır. Derleme, kaynak kodunun derleyici tarafından işlenebildiğini gösterir. Programın gereksinimleri doğru karşıladığını doğrulamak için uygulamanızı kendi senaryolarınızda çalıştırmanız gerekir.

### Bir hata bildirimini nereye göndermeliyim?

Önce OZHEX-Distributions deposundaki ilgili release notlarını ve kurulum belgesini kontrol edin. Paket/kurulum hatalarını dağıtım deposunda; compiler geliştirme, dil tasarımı ve kaynak kod hatalarını geliştirici deposunda bildirin.

# 16. Son kullanıcı hızlı başvuru

## Windows + WSL2

Ubuntu terminalinde:

```bash
ozhex --version
ozhex --help
ozhex program.ozhex program
./program
ozhexpm --help
```

## Linux x86-64

```bash
sha256sum -c ozhex-linux-x86_64.tar.gz.sha256
tar -xzf ozhex-linux-x86_64.tar.gz
cd OZHEX
bash installer/install.sh
export PATH="$HOME/.local/bin:$PATH"
ozhex --version
```

## Resmî bağlantılar

- **İndirmeler ve sürümler:** https://github.com/ismailozdemir01/OZHEX-Distributions/releases
- **Son kullanıcı kurulum belgesi:** [KURULUM.md](KURULUM.md)
- **Bu kılavuz:** [KULLANICI-KILAVUZU.md](KULLANICI-KILAVUZU.md)
- **Geliştirici kaynak kod deposu:** https://github.com/ismailozdemir01/OZHEX

---

**Belge ilkesi:** Bu kılavuz yalnızca son kullanıcı dağıtımında gerçekten yayımlanmış paketler ve kullanıcıya sunulan komutlar için geçerlidir. Kaynak kod geliştirme adımları, dahili build araçları ve henüz yayımlanmamış platform paketleri son kullanıcı kurulum talimatı olarak sunulmaz.
